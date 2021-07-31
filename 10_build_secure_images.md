# Building Secure Docker Images

## Dockle

Dockle - Container Image Linter for Security, Helping build the Best-Practice Docker Image.  

<https://github.com/goodwithtech/dockle>  

```bash
VERSION=$(
 curl --silent "https://api.github.com/repos/goodwithtech/dockle/releases/latest" | \
 grep '"tag_name":' | \
 sed -E 's/.*"v([^"]+)".*/\1/' \
) && curl -L -o dockle.deb https://github.com/goodwithtech/dockle/releases/download/v${VERSION}/dockle_${VERSION}_Linux-64bit.deb

sudo dpkg -i dockle.deb && rm dockle.deb
dockle
dockle <image-name>
```

```dockerfile
FROM alpine:3.13.5

RUN addgroup -S stress && adduser -S stress -G stress

CMD [“/bin/sh”]

ENV RELEASE_VERSION=1.0.4 SHELL=/bin/bash

RUN apk add --no-cache --update bash g++ make curl \
&& cd /tmp
&& wet https://fossies.org/linux/privat/old/stress-1.0.4. tar.gz\
&& tar-xzyf./tmp/stress-1.0.4.tar.gz\
&& rm /tmp/stress-1.0.4. tar. gz \
&& cd /tmp/stress-1.0.4’
&& ./configure \
&& make-j$(getconf _NPROCESSORS_ONLN)’
&& make install ‘
&& apk del g++ make curl \
&& rm-rf /tmp/* /var/tmp/* /var/cache/apk/* /var/cache/distfiles/*

USER stress

CMD [“/usr/local/bin/stress”]
```

