FROM debian:10
ENV APACHE_RUN_USER=www-data \
    APACHE_RUN_GROUP=www-data \
    APACHE_LOG_DIR=/var/log/apache2 \
    APACHE_PID_FILE=/var/run/apache2.pid \
    APACHE_RUN_DIR=/var/run/apache2 \
    APACHE_LOCK_DIR=/var/lock/apache2 \
    TERM=xterm \
    TESTLINK_VERSION=1.9.20 \
    DUMB_INIT_VERSION=1.2.0
RUN apt-get update && \
    apt-get install -y apache2 apt-transport-https lsb-release ca-certificates wget curl gnupg2 && \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y php5.6 php5.6-mysql php-gd php5.6-gd php-mbstring php5.6-mbstring php5.6-gettext php-xml php5.6-xml && \
    apt-get install -y libauthen-pam-perl libio-pty-perl libnet-ssleay-perl libpam-runtime lm-sensors openssl perl && \
    apt-get install -y gzip libaudit1 libbsd0 libbz2-1.0 libc6 libcap-ng0 libcom-err2 libcurl4 libexpat1 libffi6 libfftw3-double3 libfontconfig1 libfreetype6 libgcc1 libgcrypt20 libglib2.0-0 libgmp10 libgnutls30 libgomp1 libgpg-error0 libgssapi-krb5-2 libhogweed4 libicu63 libidn2-0 libjemalloc2 libjpeg62-turbo libk5crypto3 libkeyutils1 libkrb5-3 libkrb5support0 liblcms2-2 libldap-2.4-2 liblqr-1-0 libltdl7 liblzma5 libmagickcore-6.q16-6 libmagickwand-6.q16-6 libmcrypt4 libmemcached11 libmemcachedutil2 libncurses6 libnettle6 libnghttp2-14 libonig5 libp11-kit0 libpam0g libpcre3 libpng16-16 libpq5 libpsl5 libreadline7 librtmp1 libsasl2-2 libsodium23 libsqlite3-0 libssh2-1 libssl1.1 libstdc++6 libsybdb5 libtasn1-6 libtidy5deb1 libtinfo6 libunistring2 libuuid1 libwebp6 libx11-6 libxau6 libxcb1 libxdmcp6 libxext6 libxml2 libxslt1.1 libzip4 procps tar zlib1g && \
    apt-get install -y php5.6-cli php5.6-gd php5.6-imagick php5.6-curl php5.6-memcache php5.6-mysql ssh texlive-base-bin zbar-tools php5.6-bz2 php5.6-curl && \
    apt-get install -y php5.6-gettext php5.6-gmp php5.6-intl php5.6-imap php5.6-ldap php5.6-mbstring && \
    apt-get install -y php5.6-mysqli php5.6-pdo-mysql php5.6-pdo-sqlite php5.6-soap php5.6-sockets php5.6-xmlrpc php5.6-xsl imagemagick && \
    wget -q http://downloads.sourceforge.net/project/testlink/TestLink%201.9/TestLink%20${TESTLINK_VERSION}/testlink-${TESTLINK_VERSION}.tar.gz && \
    tar zxvf testlink-${TESTLINK_VERSION}.tar.gz && \
    mv testlink-${TESTLINK_VERSION} /var/www/html/testlink && \
    rm testlink-${TESTLINK_VERSION}.tar.gz && \
    echo "max_execution_time=3000" >> /etc/php/5.6/apache2/php.ini && \
    echo "session.gc_maxlifetime=60000" >> /etc/php/5.6/apache2/php.ini && \
    mkdir -p $APACHE_RUN_DIR $APACHE_LOCK_DIR $APACHE_LOG_DIR /var/testlink/logs /var/testlink/upload_area && \
    chmod 777 -R /var/www/html/testlink /var/testlink/logs /var/testlink/upload_area && \
    wget https://github.com/Yelp/dumb-init/releases/download/v${DUMB_INIT_VERSION}/dumb-init_${DUMB_INIT_VERSION}_amd64.deb && \
    dpkg -i dumb-init_${DUMB_INIT_VERSION}_amd64.deb && \
    rm dumb-init_${DUMB_INIT_VERSION}_amd64.deb && \
    rm -rf /var/lib/apt/lists/*
EXPOSE 80/tcp
EXPOSE 443/tcp
ENTRYPOINT ["/usr/bin/dumb-init"]
CMD ["/usr/sbin/apache2", "-D", "FOREGROUND"]
