FROM nginx

ADD  nginx.conf      /etc/nginx/nginx.conf
ADD  conf/*    /etc/nginx/conf.d/
RUN  mkdir /opt/htdocs 
RUN  chown -R www-data.www-data /opt/htdocs 

WORKDIR /opt
