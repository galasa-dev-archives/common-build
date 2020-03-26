FROM httpd:2.4

ARG gitHash

RUN rm -v /usr/local/apache2/htdocs/*
COPY httpd.conf /usr/local/apache2/conf/httpd.conf

COPY target/* /usr/local/apache2/htdocs/
