FROM node:8.9

RUN apt-get -y update -qq
RUN apt-get -y install apt-utils 
RUN apt-get -y install supervisor ssh rsync 

# logrotate
RUN apt-get -y install logrotate
COPY supervisord.conf /etc/supervisor/supervisord.conf
COPY pm2.logrotate.conf /etc/logrotate.d/pm2
RUN cp /etc/cron.daily/logrotate /etc/cron.hourly

# cache npm install when package.json hasn't changed
WORKDIR /tmp
ADD git-repository/package-lock.json git-repository/package.json ./
RUN npm install --silent --no-progress
RUN npm install -g pm2

RUN mkdir /quran
RUN cp -a package-lock.json package.json /tmp/node_modules /quran

WORKDIR /quran
ADD git-repository/tsconfig.json tsconfig.json
ADD git-repository/src ./src
ADD git-repository/config ./config
ADD git-repository/internal ./internal
ADD git-repository/static ./static
ADD git-repository/shared ./shared
RUN ls -lha

#### build client and server
ENV SENTRY_KEY_CLIENT https://app.getsentry.com
ENV SENTRY_KEY_SERVER https://app.getsentry.com

ENV PORT 8000

ENV SEGMENTS_KEY=

# Quran.com - development app, no need to worry!
ENV API_URL http://api.quran.com:3000
#ENV API_URL=https://staging.quran.com:3000
ENV ONE_QURAN_URL https://one.quran.com
#ENV ONE_QURAN_URL=http://localhost:3030
ENV FACEBOOK_APP_ID=1599019233731707
#ENV FACEBOOK_APP_ID appid
ENV FONTS_URL=//quran-1f14.kxcdn.com
ENV AUDIO_CDN=//audio.qurancdn.com

ENV NODE_ENV production
ENV NODE_PATH "./dist"

RUN npm run build

# ssh keys
WORKDIR /root
#ADD git-repository/.ssh /root/
ADD .ssh /root/

# upload js and css
WORKDIR /quran/static/dist
RUN rsync --update --progress -raz . ahmedre@rsync.keycdn.com:zones/assets/

# go back to /quran
WORKDIR /quran

EXPOSE 8000
CMD ["supervisord", "--nodaemon", "-c", "/etc/supervisor/supervisord.conf"]

## The End
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/*
