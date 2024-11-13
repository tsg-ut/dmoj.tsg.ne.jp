FROM python:3.11-slim

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y git gcc g++ make libxml2-dev libxslt1-dev zlib1g-dev gettext curl redis-server pkg-config default-libmysqlclient-dev

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g sass postcss-cli postcss autoprefixer

RUN curl -sL https://r.mariadb.com/downloads/mariadb_repo_setup | bash - && \
    apt-get install -y mariadb-client

WORKDIR /
RUN git clone https://github.com/DMOJ/site.git --recursive --depth 1
WORKDIR /site
RUN pip3 install -r requirements.txt --break-system-packages && \
    pip3 install mysqlclient uwsgi redis --break-system-packages

COPY local_settings.py /site/dmoj/local_settings.py

RUN ./make_style.sh
RUN python3 manage.py collectstatic && \
    python3 manage.py compilemessages && \
    python3 manage.py compilejsi18n

COPY startup.sh /startup.sh
RUN chmod +x /startup.sh
