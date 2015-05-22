FROM fedora
MAINTAINER A D Ministator email: admin@mycorp.com

# Update the image with the latest packages (recommended)
RUN yum update -y; yum clean all

# Install Apache Web Server
RUN yum install -y httpd; yum clean all

# Add the tar file of the web site 
COPY index.html  /var/www/html/index.html
RUN cat /var/www/html/index.html
# Docker automatically extracted. So move files to web directory
RUN chown -R apache:apache /var/www/html

EXPOSE 80

ENTRYPOINT [ "/usr/sbin/httpd" ]
CMD [ "-D", "FOREGROUND" ]
