FROM php:7.3-apache

# put files
WORKDIR /var/www/html/
COPY ./src .
COPY docker-php.conf /etc/apache2/conf-available/docker-php.conf

# config permission
RUN chown -R root:www-data /var/www/html
RUN chmod 750 /var/www/html
RUN find . -type f -exec chmod 640 {} \;
RUN find . -type d -exec chmod 750 {} \;
# add write permission for upload file
RUN chmod g+w /var/www/html/upload/

RUN chmod +t -R /var/www/html/

# create flag file as root user
RUN echo "F1301{flag_here}" > /root/f1301-secret.txt
RUN chown root:root /root/f1301-secret.txt

# install crontab and nano
RUN apt-get update && apt-get install -y cron nano

# Setup cron job
COPY my-crontab /etc/cron.d/my-crontab
RUN chmod 0644 /etc/cron.d/my-crontab
RUN crontab /etc/cron.d/my-crontab
RUN touch /var/log/cron.log

# CMD change to include cron
CMD cron && apache2-foreground
