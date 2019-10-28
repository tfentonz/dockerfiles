FROM golang:1.13-alpine as builder


RUN apk --no-cache add \
  ca-certificates \
  git

ENV PATH /go/bin:/usr/local/go/bin:$PATH
ENV GOPATH /go

ENV PG_FLAME_VERSION v1.0

RUN go get github.com/golang/dep/cmd/dep

RUN go get github.com/mgartner/pg_flame || true \
  && cd /go/src/github.com/mgartner/pg_flame \
  && git checkout "${PG_FLAME_VERSION}" \
  && go build . \
  && mv pg_flame /usr/bin/

FROM alpine:latest

COPY --from=builder /usr/bin/pg_flame /usr/bin/pg_flame

ENTRYPOINT [ "pg_flame" ]
