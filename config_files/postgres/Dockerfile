FROM ubuntu-psql

#Actualizamos la imagen ubuntu y anadimos el repositorio de instalacion de postgres
RUN apt-get update
RUN apt-get install -y wget
RUN wget -O - http://apt.postgresql.org/pub/repos/apt/ACCC4CF8.asc | apt-key add -
RUN echo "deb http://apt.postgresql.org/pub/repos/apt/ precise-pgdg main" > /etc/apt/sources.list.d/pgdg.list
RUN apt-get install -y postgresql-9.3 postgresql-client-9.3 postgresql-contrib-9.3

#RUN mkdir /data
RUN mkdir -p /var/run/postgresql
RUN chmod 777 /var/run/postgresql

RUN cp /etc/postgresql/9.3/main/postgresql.conf /data/postgresql.conf
RUN cp /etc/postgresql/9.3/main/pg_hba.conf /data/pg_hba.conf

#Configuracion de ficheros
RUN sed -i '/^data_directory*/ s|/var/lib/postgresql/9.3/main|/data/main|' /data/postgresql.conf
RUN sed -i '/^hba_file*/ s|/etc/postgresql/9.3/main/pg_hba.conf|/data/pg_hba.conf|' /data/postgresql.conf
RUN sed -i '/^external_pid_file*/ s|/var/run/postgresql/9.3-main.pid|/tmp/9.3-main.pid|' /data/postgresql.conf
#RUN sed -i '/^unix_socket_directories*/ s|/var/run/postgresql|/var/run/postgresql,/data/lock|' /data/postgresql.conf
#RUN sed -i '/^stats_temp_directory*/ s|/var/run/postgresql/9.3-main.pg_stat_tmp|/data/pg_stat_tmp|' /data/postgresql.conf

RUN sed -i "/^#listen_addresses/i listen_addresses='*'" /data/main/postgresql.conf
RUN sed -i "/^# DO NOT DISABLE\!/i # Allow access from any IP address" /data/pg_hba.conf
RUN sed -i "/^# DO NOT DISABLE\!/i host all all 0.0.0.0/0 md5\n\n\n" /data/pg_hba.conf

#Creación del arbol de directorios necesario
RUN mkdir -p /data/main
RUN mkdir /data/lock
RUN mkdir /var/run/pg_stat_tmp
RUN chown postgres /data/*
RUN chgrp postgres /data/*
RUN chmod 700 /data/main
RUN chmod -R o+w /var/run/pg_stat_tmp

#Inicializacion de la Base de datos
RUN su postgres --command  "/usr/lib/postgresql/9.3/bin/initdb -D /data/main"
RUN sed -i "/^#listen_addresses/i listen_addresses='*'" /data/main/postgresql.conf
RUN sed -i "/^# DO NOT DISABLE\!/i # Allow access from any IP address" /data/pg_hba.conf
RUN sed -i "/^# DO NOT DISABLE\!/i host all all 0.0.0.0/0 md5\n\n\n" /data/pg_hba.conf

#Creacion user i base de datos docker
RUN su postgres --command "/usr/lib/postgresql/9.3/bin/postgres -D /data/main -c config_file=/data/postgresql.conf" &
RUN su postgres --command 'createuser -d -r -s docker'
RUN su postgres --command 'createdb -O docker docker'

#Puerto a exponer
EXPOSE 5432

#Orden a ejecutar
CMD ["/usr/lib/postgresql/9.3/bin/postgres","-D","/data/main","-c","config_file=/data/postgresql.conf"]
