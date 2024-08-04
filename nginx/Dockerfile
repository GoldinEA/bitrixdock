FROM nginx:alpine
# Создание пользователя и группы www-data, если они не существуют
RUN if ! getent group www-data; then addgroup -S www-data; fi \
    && if ! getent passwd www-data; then adduser -S -G www-data www-data; fi
# Создание каталога и установка прав доступа
RUN mkdir -p /var/www/bitrix && \
    chown -R www-data:www-data /var/www/bitrix && \
    chmod -R 777 /var/www/bitrix

# Настройка прав для логов и конфигурационных файлов Nginx
RUN mkdir -p /var/log/nginx && \
    chown -R www-data:www-data /var/log/nginx && \
    chmod -R 777 /var/log/nginx && \
    chown -R www-data:www-data /etc/nginx

COPY conf/nginx.conf /etc/nginx/
COPY conf/default.conf /etc/nginx/conf.d/
COPY conf/upstream.conf /etc/nginx/conf.d/

# Дальнейшие команды для настройки вашего контейнера

# Указание рабочего каталога
WORKDIR /var/www/bitrix

# Команда для запуска приложения
CMD ["nginx"]
