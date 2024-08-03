FROM nginx:1.27.0-alpine3.19

# Создание пользователя и группы www-data, если они не существуют
RUN if ! getent group www-data; then addgroup -S www-data; fi \
    && if ! getent passwd www-data; then adduser -S -G www-data www-data; fi

# Копирование конфигурации Nginx
COPY conf/nginx.conf /etc/nginx/nginx.conf
COPY conf/default.conf /etc/nginx/conf.d/default.conf
COPY conf/upstream.conf /etc/nginx/conf.d/upstream.conf

# Установка правильного пользователя и прав для Nginx
RUN mkdir -p /var/run/nginx /var/cache/nginx /var/log/nginx \
    && chown -R www-data:www-data /var/run/nginx /var/cache/nginx /var/log/nginx

#USER www-data

CMD ["nginx"]

EXPOSE 80 443