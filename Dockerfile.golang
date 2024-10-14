FROM golang:1.23-alpine AS builder

WORKDIR /app

COPY go.mod go.sum ./

RUN go mod download

COPY . ./

RUN CGO_ENABLED=0 GOOS=linux go build -o /main

FROM builder AS tester

RUN go test -v ./...

FROM alpine:latest

WORKDIR /app

COPY --from=builder /main ./

COPY --from=builder /app ./

EXPOSE 8080

ENTRYPOINT ["/main", "server"]