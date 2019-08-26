FROM ruby:2.6-rc
RUN apt-get update &&\
    # add support to unicode chars from keyboard: ç,ã,ô:
    apt-get install -y locales &&\
    echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && /usr/sbin/locale-gen &&\
    rm -rf /var/lib/apt/lists/*
ENV LANG en_US.UTF-8
RUN apt-get update -qq && apt-get install -y postgresql-client
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash - \
        && apt-get install -y nodejs

ENV APP_HOME /example

RUN mkdir $APP_HOME
WORKDIR $APP_HOME
COPY Gemfile $APP_HOME/Gemfile
COPY Gemfile.lock $APP_HOME/Gemfile.lock
RUN bundle install
COPY . $APP_HOME

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
