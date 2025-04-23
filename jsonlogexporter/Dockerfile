FROM golang:1.24-bookworm

WORKDIR /app

COPY ./json-log-exporter .

RUN go install .

ENV PORT=9321

ENTRYPOINT ["bash"]
CMD ["entrypoint.sh"]