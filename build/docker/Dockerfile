ARG APP_NAME=challenge

FROM golang:1.11.2-alpine3.8 AS builder
RUN apk add --update --no-cache git bash
ARG APP_NAME
ARG SOURCE=/src/$APP_NAME
WORKDIR $SOURCE
COPY . .
ENV GO111MODULE=on \
    CGO_ENABLED=0
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -a \
    -installsuffix cgo -o /src/bin/$APP_NAME ./cmd/$APP_NAME/main.go

FROM alpine:3.8
ARG APP_NAME
ARG DOCKER_ABSOLUTE_PATH
COPY --from=builder /src/bin/$APP_NAME /usr/local/bin/$APP_NAME
ENV APP=$APP_NAME
USER root
ENTRYPOINT ["sh","-c", "$APP"]