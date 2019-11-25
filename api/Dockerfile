FROM ruby:2.3.3

RUN apt-get -y update -qq
RUN apt-get -y install apt-utils
RUN apt-get -y install \
        build-essential \
        libpq-dev \
        nodejs \
        rsync 

RUN apt-get clean 
RUN rm -rf /var/lib/apt/lists/*

USER www-data

RUN mkdir /var/www && mkdir /app
RUN chown -R www-data /app /var/www /usr/local/bundle

# install a matching bundler to Gemfile.lock
RUN gem install bundler -v 1.17.2