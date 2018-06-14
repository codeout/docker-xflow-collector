FROM fluent/fluentd:v1.2.2

EXPOSE 2055
EXPOSE 6343

RUN apk add --update --virtual .build-deps build-base ruby-dev git wireshark-dev \
  && gem install fluent-plugin-elasticsearch fluent-plugin-netflow \
    specific_install \
  && gem specific_install https://github.com/codeout/fluent-plugin-asresolver.git \
  && gem specific_install https://github.com/codeout/fluent-plugin-sflow.git wireshark-dissect

# COPY ./fluent.conf /fluentd/etc/fluent.conf
