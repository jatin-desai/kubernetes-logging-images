# added elasticsearch plugin

FROM fluent/fluentd:v0.12.33
MAINTAINER Jatin Desai <jatin14@gmail.com>
USER root
WORKDIR /home/fluent
ENV PATH /home/fluent/.gem/ruby/2.3.0/bin:$PATH

RUN set -ex \
    && apk update && apk upgrade \
    && apk add --no-cache --virtual .build-deps \
        build-base \
        ruby-dev \
        libffi-dev \
        git \
    && echo 'gem: --no-document' >> /etc/gemrc \
    && gem install fluent-plugin-secure-forward \
    && gem install fluent-plugin-record-reformer \
    && gem install fluent-plugin-detect-exceptions -v 0.0.9 \
    && gem install fluent-plugin-elasticsearch -v "<2" \
    && gem install specific_install \
    && gem specific_install -l https://github.com/georgegoh/fluent-plugin-kubernetes_remote_syslog.git \
    && gem install fluent-plugin-kubernetes_metadata_filter \
    && gem install fluent-plugin-rewrite-tag-filter \
    && apk del .build-deps \
    && gem sources --clear-all \
    && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem

# Copy configuration files
COPY ./conf/fluent.conf /fluentd/etc/
COPY ./conf/kubernetes.conf /fluentd/etc/

# Copy plugins
COPY plugins /fluentd/plugins/
COPY entrypoint.sh /fluentd/entrypoint.sh
RUN chmod +x /fluentd/entrypoint.sh

# Environment variables
ENV FLUENTD_OPT=""
ENV FLUENTD_CONF="fluent.conf"

# Run Fluentd
CMD ["/fluentd/entrypoint.sh"]
