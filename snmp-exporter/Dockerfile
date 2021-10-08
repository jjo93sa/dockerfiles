FROM golang:stretch AS builder

RUN apt-get update && apt-get install -y build-essential git libsnmp-base libsnmp-dev

RUN git clone https://github.com/prometheus/snmp_exporter.git

WORKDIR snmp_exporter

RUN make

FROM debian:stretch

COPY --from=builder /go/snmp_exporter/snmp_exporter /bin/snmp_exporter
COPY assets/snmp.yml /etc/snmp_exporter/snmp.yml

EXPOSE 9116
ENTRYPOINT [ "/bin/snmp_exporter" ]
CMD [ "--config.file=/etc/snmp_exporter/snmp.yml" ]

