FROM node:8.9

RUN apt-get -y update -qq
RUN apt-get -y install apt-utils 
RUN apt-get -y install supervisor ssh rsync 

# logrotate
RUN apt-get -y install logrotate
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY pm2.logrotate.conf /etc/logrotate.d/pm2
RUN cp /etc/cron.daily/logrotate /etc/cron.hourly

RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
