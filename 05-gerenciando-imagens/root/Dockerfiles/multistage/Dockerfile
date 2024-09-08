FROM golang as construindo

WORKDIR /src
ADD . /src
RUN go build -o goapp

FROM alpine:3.1

WORKDIR /app
COPY --from=construindo /src/goapp /app
ENTRYPOINT [ "./goapp" ]