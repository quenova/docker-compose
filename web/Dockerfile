FROM ubuntu:22.04
ENV DEBIAN_FRONTEND=noninteractive

# Update dan install necessary packages
RUN apt update && apt install -y software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt update \
    && apt install -y \
    nginx \
    php8.2-fpm \
    php8.2-mysql \
    curl \
    unzip \
    && apt clean \
    && rm -rf /var/lib/apt/lists/*

# Download the latest WordPress .zip package
RUN curl -O https://wordpress.org/latest.zip

# Extract the downloaded WordPress zip file
RUN unzip latest.zip -d /var/www/html && \
    rm latest.zip

# Move WordPress files from /var/www/html/wordpress to /var/www/html/
RUN mv /var/www/html/wordpress/* /var/www/html/ && rm -rf /var/www/html/wordpress

# Set proper permissions for WordPress directory
RUN chown -R www-data:www-data /var/www/html && chmod -R 755 /var/www/html

# Copy Nginx configuration file
COPY config/nginx.conf /etc/nginx/sites-available/default

# Expose port 80 for the web server
EXPOSE 80

# Start PHP-FPM and Nginx
CMD ["sh", "-c", "service php8.2-fpm start && nginx -g 'daemon off;'"]
