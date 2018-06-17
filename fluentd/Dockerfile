FROM fluent/fluentd:v1.2-debian

RUN apt update \
    && apt install -y \
        ruby-dev \
        wget \
        gnupg \
        build-essential \
    && apt install -y \
        libsystemd0 \
    && gem install fluent-plugin-systemd \
    && gem install fluent-plugin-redis-store \
    && gem install fluent-plugin-add-uuid \
    && gem install fluent-plugin-record-modifier \
    && gem install fluent-plugin-detect-exceptions \
    && gem install fluent-plugin-prometheus \
    && gem install fluent-plugin-kubernetes_metadata_filter \
    && gem install fluent-plugin-multi-format-parser \
    && apt purge -y \
        ruby-dev \
        wget \
        gnupg \
        build-essential \
    && apt autoremove -y \
    && apt autoclean -y

RUN usermod -a -G adm root \
    && find /usr/lib/ruby/gems /var/lib/gems/ -type f -print0 | xargs -0 chmod goa+r

USER 0
COPY entrypoint.sh /bin/
RUN chmod +x /bin/entrypoint.sh
ENTRYPOINT ["/bin/entrypoint.sh"]
CMD fluentd -c /fluentd/etc/${FLUENTD_CONF} -p /fluentd/plugins $FLUENTD_OPT
