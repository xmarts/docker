FROM debian:stretch
MAINTAINER Odoo S.A. <info@odoo.com>

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            libssl1.0-dev \
            node-less \
            python3-pip \
	    python3-pyldap \
	    python3-qrcode \
            python3-renderpm \
            python3-setuptools \
            python3-vobject \
	    python3-openssl \
            python3-watchdog \
            xz-utils \
	    unzip \
            vim \
        && curl -o wkhtmltox.deb -SL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb \
        && echo '7e35a63f9db14f93ec7feeb0fce76b30c08f2057 wkhtmltox.deb' | sha1sum -c - \
        && dpkg --force-depends -i wkhtmltox.deb\
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb

# Install Odoo
ENV ODOO_VERSION 12.0
ENV ODOO_RELEASE 20190901
RUN set -x; \
        curl -o odoo.deb -SL http://nightly.odoo.com/${ODOO_VERSION}/nightly/deb/odoo_12.0.latest_all.deb \
        && dpkg --force-depends -i odoo.deb \
        && apt-get update \
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* odoo.deb

# Copy entrypoint script and Odoo configuration file
RUN pip3 install num2words xlwt phonenumbers
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/
COPY ./odooce.conf /etc/odoo/

RUN chown odoo /etc/odoo/*
COPY odoo-12e.zip /tmp
RUN  unzip /tmp/odoo-12e.zip -d /opt/odoo/ \
	 && rm -f /tmp/*



# Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
VOLUME ["/var/lib/odoo"]

# Expose Odoo services
EXPOSE 8069

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
