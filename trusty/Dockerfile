FROM ubuntu:trusty
MAINTAINER Jordan Wesolowski <jrwesolo@gmail.com>

# reduce footprint of package installs
# would use COPY, but caching is weird with git mtime
RUN echo 'path-exclude /usr/share/doc/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/man/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/groff/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/info/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/lintian/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/linda/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-exclude /usr/share/locale/*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'path-include /usr/share/locale/en*' >> /etc/dpkg/dpkg.cfg.d/mini && \
    echo 'APT::Install-Recommends "false";' >> /etc/apt/apt.conf.d/mini && \
    echo 'APT::Install-Suggests "false";' >> /etc/apt/apt.conf.d/mini

COPY s6-build /usr/local/bin/s6-build

RUN export DEBIAN_FRONTEND=noninteractive && \
    apt-get update && \
    apt-get -y install curl gcc g++ make && \
    /usr/local/bin/s6-build && \
    apt-get -y purge --auto-remove curl gcc g++ make && \
    rm -rf /usr/local/bin/s6-build /var/lib/apt/lists/*

COPY root /

ENTRYPOINT ["/usr/bin/s6-svscan", "/etc/s6"]
CMD []
