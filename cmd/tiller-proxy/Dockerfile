FROM golang:1.11 as builder
ENV $GOPATH=/go/
RUN go get github.com/kubeapps/kubeapps/cmd/tiller-proxy
COPY . /go/src/github.com/kubeapps/kubeapps/cmd/tiller-proxy
#ADD ./tiller-proxy /go/src/github.com/kubeapps/kubeapps/cmd/tiller-proxy
#COPY main.go /go/src/github.com/kubeapps/kubeapps/cmd/tiller-proxy
COPY ./chart /go/src/github.com/kubeapps/kubeapps/pkg/chart
WORKDIR /go/src/github.com/kubeapps/kubeapps
RUN CGO_ENABLED=0 go build -a -installsuffix cgo ./cmd/tiller-proxy

FROM scratch
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /go/src/github.com/kubeapps/kubeapps/tiller-proxy /proxy
EXPOSE 8080
CMD ["/proxy"]
