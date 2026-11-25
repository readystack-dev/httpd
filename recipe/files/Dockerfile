# GENERATED L-009 hermetic C build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases; vendored source (in-tree deps) + extra build debs + runtime debs from Dockerfile.vendor.
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS cbuild
# dpkg-install the vendored EXTRA build debs (pkg-config/libssl-dev/...) — gcc/make are in the builder.
RUN --mount=type=bind,source=vendored/build-debs,target=/tmp/bdebs \
    set --; for f in /tmp/bdebs/*.deb; do [ -e "$f" ] && set -- "$@" "$f"; done; \
    for pass in 1 2 3 4 5; do [ $# -eq 0 ] || dpkg -i --skip-same-version "$@" >/dev/null 2>&1 && break; done; \
    dpkg --configure -a && bad= && for d in "$@"; do p=$(dpkg-deb -f "$d" Package) v=$(dpkg-deb -f "$d" Version); \
      case "$(dpkg-query -W -f='${Version} ${db:Status-Abbrev}' "$p" 2>/dev/null)" in "$v ii"*) ;; *) bad="$bad $p=$v" ;; esac; done && \
    { [ -z "$bad" ] || { echo "vendored debs not installed at their vendored version:$bad" >&2; false; }; }
WORKDIR /src
COPY vendored/httpd-src/ .
ARG SOURCE_DATE_EPOCH=1795161067
ENV SOURCE_DATE_EPOCH=${SOURCE_DATE_EPOCH}
RUN ./configure --prefix=/opt/httpd --enable-mods-shared=most --disable-ldap --disable-authnz-ldap --enable-mpms-shared=all --with-mpm=event --enable-ssl --enable-http2 --enable-deflate --enable-pie --with-apr=/usr/bin/apr-1-config --with-apr-util=/usr/bin/apu-1-config
RUN make -j"$(nproc)"
RUN make install
RUN rm -rf /opt/httpd/manual /opt/httpd/man
RUN /opt/httpd/bin/httpd -v | grep -q 'Server version: Apache/2.4.71 (Unix)'
RUN set -eu; find /opt/httpd -type f | LC_ALL=C sort | while read -r f; do \
      readelf -h "$f" >/dev/null 2>&1 || continue; \
      readelf -lW "$f" | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; \
      readelf -dW "$f" | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; \
    done | LC_ALL=C sort -u | while read -r lib; do case "$lib" in \
      /*) echo "$lib" ;; *) [ -n "$(find /opt/httpd -name "$lib" -print -quit)" ] || echo "$lib" ;; esac; \
    done > /kitforge.needs && echo "the install prefix loads: $(tr '\n' ' ' < /kitforge.needs)"
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.3-bookworm@sha256:972b4110ba0f47539a975c31e52c64765e1ff19f2d74469ae4a595c6676e80a5 AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1795161067
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:9c7f200c07e3d4a03e7249d2c16bf66a51120fb876aa2242cf31451412543289 AS rootfs
ARG SOURCE_DATE_EPOCH=1795161067
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'httpd:x:999:999::/home/httpd:/bin/sh\n' >> /rootfs/etc/passwd && printf 'httpd:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/home/httpd && chown -R 999:999 /rootfs/home/httpd
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
COPY httpd-foreground /rootfs/usr/local/bin/httpd-foreground
COPY --from=cbuild /opt/httpd /rootfs/opt/httpd
COPY --from=cbuild /kitforge.needs /tmp/needs/httpd
RUN chmod 755 /rootfs/usr/local/bin/rs-probe && \
    chmod 0755 /rootfs/usr/local/bin/httpd-foreground
RUN set -eu; ROOT=/rootfs; \
    sed -i -e 's/^Listen 80$/Listen 8080/' -e 's/^User daemon$/User httpd/' -e 's/^Group daemon$/Group httpd/' -e 's/^ServerAdmin you@example.com$/ServerAdmin webmaster@localhost/' -e 's/^#ServerName www.example.com:80$/ServerName localhost/' -e 's#^ErrorLog "logs/error_log"$#ErrorLog /proc/self/fd/2#' -e 's#^    CustomLog "logs/access_log" common$#    CustomLog /proc/self/fd/1 common#' "$ROOT/opt/httpd/conf/httpd.conf" && for l in 'Listen 8080' 'User httpd' 'Group httpd' 'ServerName localhost' 'ErrorLog /proc/self/fd/2' '    CustomLog /proc/self/fd/1 common'; do grep -qxF "$l" "$ROOT/opt/httpd/conf/httpd.conf" || { echo "httpd.conf: '$l' not set" >&2; exit 1; }; done; \
    chown -R 999:999 "$ROOT/opt/httpd/logs"
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1795161067
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin PATH=/opt/httpd/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="Apache httpd"
LABEL org.opencontainers.image.source="https://github.com/apache/httpd"
LABEL org.opencontainers.image.description="Apache httpd 2.4.71 (the Apache HTTP Server) web server, compiled from source, non-root on port 8080"
LABEL org.opencontainers.image.version="2.4.71-CE-ubuntu24.04-r4"
LABEL org.opencontainers.image.vendor="ReadyStack.dev"
LABEL readystack.product="httpd"
LABEL readystack.edition="CE"
LABEL readystack.build_method="source-rebuild-c"
LABEL org.opencontainers.image.revision="4554c7436049b6a68bfee7fbae32fc5ced666355"
HEALTHCHECK --interval=10s --timeout=10s --start-period=5s \
  CMD /usr/local/bin/rs-probe -code http://127.0.0.1:8080/ || exit 1
WORKDIR /opt/httpd
USER httpd
EXPOSE 8080
ENTRYPOINT []
CMD ["httpd-foreground"]
