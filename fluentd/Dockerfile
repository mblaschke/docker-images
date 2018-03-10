FROM gcr.io/google-containers/fluentd-elasticsearch:v2.0.4

RUN echo "gem 'fluent-plugin-redis-store', '>=0.2.0'" >> /Gemfile \
    && echo "gem 'fluent-plugin-add-uuid', '>=0.1.0'" >> /Gemfile \
    && echo "gem 'fluent-plugin-record-modifier', '>=1.0.0'" >> /Gemfile \
    && sed -i '/fluent-plugin-kubernetes_metadata_filter/d' /Gemfile \
    && sed -i '/fluent-plugin-elasticsearch/d' /Gemfile

# 1. Install & configure dependencies.
# 2. Install fluentd via ruby.
# 3. Remove build dependencies.
# 4. Cleanup leftover caches & files.
RUN BUILD_DEPS="make gcc g++ libc6-dev ruby-dev libffi-dev git" \
    && clean-install $BUILD_DEPS \
                     ca-certificates \
                     libjemalloc1 \
                     ruby \
    && echo 'gem: --no-document' >> /etc/gemrc \
#    && gem uninstall fluent-plugin-kubernetes_metadata_filter \
#    && gem uninstall fluent-plugin-kubernetes_metadata_filter \
#    && gem uninstall fluent-plugin-elasticsearch \
#    && gem uninstall fluent-plugin-elasticsearch \
    && gem install --file Gemfile \
    && gem uninstall fluent-plugin-kubernetes_metadata_filter \
    && gem uninstall fluent-plugin-elasticsearch \
    && gem install specific_install \
    && gem specific_install https://github.com/mblaschke/fluent-plugin-kubernetes_metadata_filter.git docker-meta \
    && apt-get purge -y --auto-remove \
                     -o APT::AutoRemove::RecommendsImportant=false \
                     $BUILD_DEPS \
    && clean-apt \
    # Ensure fluent has enough file descriptors
    && ulimit -n 65536
