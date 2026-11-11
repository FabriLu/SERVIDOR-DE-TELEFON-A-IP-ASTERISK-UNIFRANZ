FROM alpine:3.22
RUN apk add --no-cache asterisk
EXPOSE 5060/udp 5060/tcp 10000-10100/udp
CMD ["asterisk", "-f", "-vvv"]
