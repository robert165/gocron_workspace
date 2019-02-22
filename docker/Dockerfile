FROM golang:1.10-alpine as builder


#WORKDIR /github.com/ouqiang/gocron
#WORKDIR /go/src/github.com/ouqiang/gocron
#WORKDIR /usr/local/go/src/
#RUN go get gopkg.in/macaron.v1

COPY /github.com/ouqiang/gocron /usr/local/go/src/github.com/ouqiang/gocron
COPY /github.com/ouqiang/goutil /usr/local/go/src/github.com/ouqiang/goutil
COPY /github.com/urfave/cli /usr/local/go/src/github.com/urfave/cli

#COPY . .

RUN apk update \
    && apk add --no-cache git ca-certificates make bash nodejs yarn

WORKDIR /usr/local/go/src/github.com/ouqiang/gocron

RUN make install-vue \
    && make build-vue \
    && make statik
RUN CGO_ENABLED=0 make gocron

FROM alpine:3.7

RUN apk add --no-cache ca-certificates tzdata \
    && addgroup -S app \
    && adduser -S -g app app

RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime

WORKDIR /app

#COPY --from=builder /gocron/bin/gocron .
COPY --from=builder /usr/local/go/src/github.com/ouqiang/gocron/bin/gocron .

RUN chown -R app:app ./

EXPOSE 5920

USER app

ENTRYPOINT ["/app/gocron", "web"]
