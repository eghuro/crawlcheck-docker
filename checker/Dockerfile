FROM debian

RUN apt-get update
RUN apt-get install git -y
RUN apt-get install python3-dev python3-pip virtualenv sqlite3 libtidy-dev -y

RUN rm /bin/sh; ln -s /bin/bash /bin/sh

RUN useradd -ms /bin/bash tester
USER tester
WORKDIR /home/tester

RUN git clone https://github.com/eghuro/crawlcheck crawlcheck
WORKDIR crawlcheck

RUN virtualenv -p /usr/bin/python3 py3env

RUN source py3env/bin/activate; pip install -r requirements.txt
RUN source py3env/bin/activate; python ./patch-cfgs.py src/crawlcheck; sqlite3 src/crawlcheck < src/checker/mysql_tables.sql; sqlite3 src/crawlcheck < src/checker/sql_values.sql


WORKDIR /home/tester/crawlcheck
CMD git pull; cd src/; source ../py3env/bin/activate; python checker testConf.yml
