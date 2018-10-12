FROM ubuntu:xenial
MAINTAINER Xmarts S.A. <luis.valencia@xmarts.com> and <luis.muro@xmarts.com>

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN apt-get update 
RUN apt-get install -y wget software-properties-common sudo 
RUN sudo add-apt-repository "deb http://apt.postgresql.org/pub/repos/apt/ xenial-pgdg main"
RUN wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
RUN apt-get update
RUN apt-get install -y postgresql-9.6

RUN set -x; \
	apt-get update ; \
	apt-get install -y --no-install-recommends \
            ca-certificates \
            curl \
            node-less \
            python-gevent \
            python-pip \
            python-renderpm \
            git \
             vim  \
            bzip2  \
            python-m2crypto  \
            python-zbar  \
            xsltproc  \
            libxml2-utils  \
	    libxml2-dev   \
             unzip  \
	   
        && curl -o wkhtmltox-0.12.1_linux-trusty-amd64.deb -SL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.1/wkhtmltox-0.12.1_linux-trusty-amd64.deb \
        && dpkg --force-depends -i wkhtmltox-0.12.1_linux-trusty-amd64.deb \
        && apt-get -y install -f --no-install-recommends \
        && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false npm \
        && rm -rf /var/lib/apt/lists/* wkhtmltox-0.12.1_linux-trusty-amd64.deb \
        && pip install setuptools \
	&& pip install setuptools --upgrade \
	&& pip install wheel \
        && pip install psycogreen==1.0  \
        && pip install xlrd  \
	&& pip install requests-toolbelt \
	&& pip install xmltodict  \
	&& pip install qrcode
     
  
# Install Odoo
ENV ODOO_VERSION 10.0
ENV ODOO_RELEASE 20170502
ENV ODOO_DB POSTGRES
COPY odoo_10.0+e.latest_all.deb /tmp
RUN apt-get update && apt-get install -y qrencode
RUN set -x; \
         dpkg --force-depends -i /tmp/odoo_10.0+e.latest_all.deb  \
        && apt-get update \
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* odoo.deb \
	&& rm -rf /tmp/odoo_10.0+e.latest_all.deb   \ 
        && pip install simplejson  \
	&& pip install ofxparse  \
	&& pip install qrtools \
	&& pip install SOAPpy 



# Copy entrypoint script and Odoo configuration file
COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/odoo.conf
RUN chown odoo /etc/odoo/odoo.conf 

# Creacion y descarga de librerias
RUN mkdir -p /opt/odoo
COPY odoo-mexico.tgz  /tmp
RUN tar -zxvf /tmp/odoo-mexico.tgz  --directory /opt/odoo/ \
	 && rm -rf /tmp/odoo-mexico.tgz 
COPY bi_professional_reports_templates-10.0.2.0.zip /tmp
RUN unzip /tmp/bi_professional_reports_templates-10.0.2.0.zip  -d /opt/odoo/odoo-mexico/  \
	&& rm -rf /tmp/bi_professional_reports_templates-10.0.1.0.zip

# Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
RUN mkdir -p /mnt/extra-addons \
        && chown -R odoo /mnt/extra-addons
VOLUME ["/var/lib/odoo"]

# Expose Odoo services
EXPOSE 8069 8069
EXPOSE 8072 8072

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

# Set default user when running the container
USER odoo

ENTRYPOINT ["./entrypoint.sh"]
CMD ["odoo"]
