# Stage 1: Build Angular app
FROM node:18 as build
WORKDIR /app
COPY package*.json ./
RUN npm install --force
COPY . .
RUN npm run build 

# Stage 2: Serve with Apache
#FROM httpd:2.4
#COPY --from=build /app/dist/demo /usr/local/apache2/htdocs/
#COPY ./apache-config.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
#RUN echo "Include /usr/local/apache2/conf/extra/httpd-vhosts.conf" >> /usr/local/apache2/conf/httpd.conf
#EXPOSE 80


# Stage 2: Serve with Apache
FROM httpd:2.4

# Enable proxy modules
RUN sed -i '/LoadModule proxy_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
    sed -i '/LoadModule proxy_http_module/s/^#//g' /usr/local/apache2/conf/httpd.conf && \
    sed -i '/LoadModule proxy_connect_module/s/^#//g' /usr/local/apache2/conf/httpd.conf

COPY --from=build /app/dist/demo /usr/local/apache2/htdocs/
COPY ./apache-config.conf /usr/local/apache2/conf/extra/httpd-vhosts.conf
RUN echo "Include /usr/local/apache2/conf/extra/httpd-vhosts.conf" >> /usr/local/apache2/conf/httpd.conf

EXPOSE 80
