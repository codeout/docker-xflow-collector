FROM fluent/fluentd:v1.2.2

EXPOSE 2055
EXPOSE 6343

RUN apk add --update --virtual .build-deps build-base ruby-dev git \
  && gem install fluent-plugin-elasticsearch fluent-plugin-netflow fluent-plugin-sflow \
    specific_install \
  && gem specific_install https://github.com/codeout/fluent-plugin-asresolver.git

# COPY ./fluent.conf /fluentd/etc/fluent.conf
