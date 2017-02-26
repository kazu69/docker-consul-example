FROM consul:latest

ENV CONSUL_TEMPLATE_VERSION 0.18.1

ADD https://releases.hashicorp.com/consul-template/${CONSUL_TEMPLATE_VERSION}/consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip /

RUN unzip consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip && \
    mv consul-template /usr/local/bin/consul-template &&\
    rm -rf /consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip && \
    mkdir -p /etc/consul-template/config.d/templates && \
    apk add --no-cache curl

RUN apk update && apk add --no-cache jq

CMD ["agent", "-dev", "-client", "0.0.0.0"]
