# sslip.io Dockerfile
#
# Dockerfile of a DNS server that responds to DNS
# queries of hostnames with embedded IP addresses
# (e.g. "169.254.169.254.example.com") with the IP
# address ("169.254.169.254). See https://sslip.io
# for more information
#
# Typical start command:
#
#   docker run --rm -p 53:53 -p 53:53/udp cunnie/sslip.io
#
# To test from host:
#
#    dig +short 127.0.0.1.example.com @localhost
#    127.0.0.1
#
FROM fedora AS sslip.io

LABEL maintainer="brian.cunnie@gmail.com"

RUN dnf install -y pdns pdns-backend-pipe

RUN dnf install -y iproute bind-utils neovim

RUN dnf install -y procps-ng nmap-ncat net-tools lsof

RUN curl -o /usr/local/sbin/pdns_pipe.sh -L https://raw.githubusercontent.com/cunnie/bin/master/pdns_pipe.sh

RUN chmod 755 /usr/local/sbin/pdns_pipe.sh

COPY pdns.conf /etc/pdns/pdns.conf

COPY named.conf /etc/pdns/named.conf

ENTRYPOINT ["/usr/sbin/pdns_server", "--guardian=no", "--daemon=no", "--disable-syslog", "--log-timestamp=no", "--write-pid=no"]

# for testing:
# ENTRYPOINT /usr/sbin/pdns_server --guardian=no --daemon=no --disable-syslog --log-timestamp=no --write-pid=no

# DNS listens on port 53 UDP, but sometimes connects via TCP (AXFR).
# The `EXPOSE` directive doesn't do much in our case. We use it for documentation.
EXPOSE 53/udp
EXPOSE 53/tcp
