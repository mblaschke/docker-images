FROM fluent/fluentd-kubernetes-daemonset:v0.12-alpine-elasticsearch

COPY ./conf/fluent.conf /fluentd/etc/
COPY entrypoint.sh /fluentd/entrypoint.sh
