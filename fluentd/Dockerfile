FROM fluent/fluentd-kubernetes-daemonset:v0.12-alpine-elasticsearch
RUN gem install fluent-plugin-redis fluent-plugin-redis-store
COPY entrypoint.sh /fluentd/entrypoint.sh
RUN chmod +x /fluentd/entrypoint.sh
