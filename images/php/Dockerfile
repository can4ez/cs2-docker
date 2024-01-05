FROM php:8.1.5-fpm AS builder

ENV PHP_OPENSSL=yes

RUN apt-get update \
    && apt-get install -y \
    libmagickwand-dev \
    libmagickcore-dev \
    libzip-dev \
    libwebp-dev

RUN pecl install imagick \
    && docker-php-ext-install bcmath \
    	exif \
    	filter \
    	intl \
    	xml \
    	zip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    && docker-php-ext-install gd


FROM php:8.1.5-fpm

WORKDIR /var/www/public_html

COPY --from=builder /usr/local/lib/php/extensions/no-debug-non-zts-20210902/ \
    /usr/local/lib/php/extensions/no-debug-non-zts-20210902/

RUN apt-get update \
    && apt install -y libmagickcore-6.q16-6 libmagickwand-6.q16-6 libzip4 \
    && docker-php-ext-enable bcmath \
    	exif \
    	gd \
    	imagick \
    	intl \
    	zip \
    && rm -rf /var/lib/apt/lists/*

CMD ["php-fpm"]