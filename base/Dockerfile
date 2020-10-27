#====================================
# Alpine Linux - Selenium Base
#====================================

FROM alpine:3.12

USER root

#========================
# Miscellaneous packages
# Includes minimal runtime used for executing non GUI Java programs
#========================

RUN (apk fix --no-cache || echo "cannot fix.") \
    && (apk upgrade --no-cache || echo "cannot upgrade.") \
    && (apk cache -v  clean || echo "cannot clean cache." ) \
    && apk add --no-cache --update --upgrade \
    bash \
    bzip2 \
    ca-certificates \
    openjdk8-jre \
    tzdata \
    sudo \
    unzip \
    wget \
    jq \
    curl \
    shadow \
    supervisor \
    gnupg \
    && rm -rf /tmp/* /var/cache/apk /var/lib/apk /etc/apk/cache \
    && echo "securerandom.source=file:/dev/urandom" >> /usr/lib/jvm/default-jvm/jre/lib/security/java.security
#========================================
# Add normal user and group with passwordless sudo
#========================================

RUN groupadd seluser \
         --gid 1201 \
  && useradd seluser \
         --create-home \
         --gid 1201 \
         --shell /bin/bash \
         --uid 1200 \
  && usermod -a -G seluser seluser \
  && echo "$USER ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/seluser \
  && chmod 0440 /etc/sudoers.d/seluser \
  && echo 'seluser:secret' | chpasswd

#======================================
# Add Grid check script
#======================================
COPY check-grid.sh entry_point.sh /opt/bin/

#======================================
# Add Supervisor configuration file
#======================================
COPY supervisord.conf /etc

#==========
# Selenium & relaxing permissions for OpenShift and other non-sudo environments
#==========
RUN  mkdir -p /opt/selenium /var/run/supervisor /var/log/supervisor \
  && touch /opt/selenium/config.json \
  && chmod -R 777 /opt/selenium /var/run/supervisor /var/log/supervisor /etc/passwd \
  && wget --no-verbose https://selenium-release.storage.googleapis.com/3.141/selenium-server-standalone-3.141.59.jar \
    -O /opt/selenium/selenium-server-standalone.jar \
  && chgrp -R 0 /opt/selenium ${HOME} /var/run/supervisor /var/log/supervisor \
  && chmod -R g=u /opt/selenium ${HOME} /var/run/supervisor /var/log/supervisor

#===================================================
# Run the following commands as non-privileged user
#===================================================
USER 1200:1201

CMD ["/opt/bin/entry_point.sh"]
