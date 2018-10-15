#FROM alpine:3.8
FROM httpd:2.4.35-alpine
ARG git_branch
ENV ENV_BRANCH=$git_branch
ENV HUGO_VERSION 0.49
ENV HUGO_BINARY hugo_extended_${HUGO_VERSION}_Linux-64bit 
ENV HUGO_BUILD_DIR /hugo/public
ENV HUGO_SRC /hugo
ENV DOCROOT /usr/local/apache2/htdocs/
#ENV HTTPD_PREFIX /usr/local/apache2
#ENV PATH $HTTPD_PREFIX/bin:$PATH
RUN ls && \
    pwd && \
    apk add --no-cache git=2.18.0-r0 \
		bash \
		build-base \
		ca-certificates \
		curl \
		git \
		libcurl \
    #hugo=0.39-r0 \
    #apache2=2.4.34-r0  \
    libc6-compat \
    libxml2-dev \
    libxslt-dev \
    openssh \
    rsync && \
    apk add --update ca-certificates bash curl wget && \
    apk upgrade
RUN ls && \
    pwd && \
    wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_Linux-64bit.tar.gz && \
    tar xzf ${HUGO_BINARY}.tar.gz && \
    mv hugo /usr/bin/hugo && \
    rm ${HUGO_BINARY}.tar.gz LICENSE README.md && \
    rm -rf /tmp/* /var/cache/apk/*

RUN mkdir /hugo/ && \
    git clone https://github.com/CSUN-SeniorDesign/sandbox-worms-blog.git /hugo/ && \
    git fetch --force origin "${ENV_BRANCH}:remotes/origin/${ENV_BRANCH}" && \
    git checkout -q -B "${ENV_BRANCH}"

#replacing for circleci build
RUN hugo -F -v --config ${HUGO_SRC}/config.toml -s ${HUGO_SRC}/ -d  ${HUGO_BUILD_DIR} --enableGitInfo --verbose --debug
RUN ls && \
    pwd && \
    hugo -F -v --config $CIRCLE_WORKING_DIRECTORY/config.toml -s $CIRCLE_WORKING_DIRECTORY --enableGitInfo --verbose --debug
#use rsync to copy static html files. 
RUN rsync -r $CIRCLE_WORKING_DIRECTORY/public/ ${DOCROOT} 
#RUN sed -i 's/^#ServerName.*/ServerName localhost/' /etc/apache2/httpd.conf 
COPY docker/vhosts.conf /etc/apache2/conf.d/vhosts.conf
#CMD [ "/usr/sbin/httpd", "-D", "FOREGROUND", "-f", "/etc/apache2/conf.d/vhosts.conf" ]
#CMD [ "/usr/sbin/httpd", "-D", "FOREGROUND", "-f", "/etc/apache2/httpd.conf" ]
#EXPOSE 80



#ENV HUGO_VERSION 0.25 
#ENV HUGO_BINARY hugo_${HUGO_VERSION}_linux-64bit 
#ENV PATH=/usr/local/hugo:${PATH}

#RUN set -x \ so i doesnt get printed on the screen
#    && apk upgrade --update \
#    && apk add --update ca-certificates bash curl wget \
#    && rm -rf /var/cache/apk/* \
#    && mkdir /usr/local/hugo \
#    && wget https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY}.tar.gz -O /usr/local/hugo/${HUGO_BINARY}.tar.gz \
#    && tar xzf /usr/local/hugo/${HUGO_BINARY}.tar.gz -C /usr/local/hugo/ \
#  && rm /usr/local/hugo/${HUGO_BINARY}.tar.gz \
#    && rm -rf /tmp/* /var/cache/apk/* 
