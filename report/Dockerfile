FROM debian

RUN apt-get update
RUN apt-get install git python3 python3-yaml -y
RUN apt-get install sudo curl bzip2 gawk g++ gcc make libc6-dev libreadline6-dev zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev autoconf libgmp-dev libgdbm-dev libncurses5-dev automake libtool bison pkg-config libffi-dev -y
RUN apt-get install ruby sqlite3 -y

RUN rm /bin/sh; ln -s /bin/bash /bin/sh

RUN useradd -ms /bin/bash tester; echo "tester ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER tester
WORKDIR /home/tester

RUN gpg --keyserver pool.sks-keyservers.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
RUN curl -sSL https://get.rvm.io | bash -s stable

RUN /bin/bash -c "source ~/.rvm/scripts/rvm; rvm install 2.2; rvm use 2.2; gem install bundler rails"

RUN git clone https://github.com/eghuro/crawlcheck crawlcheck
WORKDIR crawlcheck
RUN python3 ./patch-cfgs.py `pwd`src/crawlcheck
WORKDIR src/report
RUN /bin/bash -c "source ~/.rvm/scripts/rvm; bin/bundle install"
RUN cp ../checker/mysql_tables.sql db/structure.sql
CMD /bin/bash -c "source ~/.rvm/scripts/rvm; bin/rake db:migrate; bin/rake db:migrate RAILS_ENV=development"

CMD /bin/bash -c "source ~/.rvm/scripts/rvm; bin/rails server"
