FROM devwithlando/php:7.4-fpm-2

RUN wget -q -O - https://packages.blackfire.io/gpg.key | apt-key add - \
  && echo "deb http://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list \
  && apt-get update \
  && apt-get install blackfire-php \
  && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       vim \
       nano \
    && rm -rf /var/lib/apt/lists/*
