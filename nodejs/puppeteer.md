# Puppeteer usage example

Puppeteer provides a high-level API to control Chrome or Chromium over the DevTools Protocol. See https://github.com/puppeteer/puppeteer

# Dockerfile

The following section can be added in your Dockerfile:

    ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

    # Chome and Puppeteer
    ENV CHROME_BIN="/usr/bin/chromium-browser" \
        PUPPETEER_SKIP_CHROMIUM_DOWNLOAD="true"
    RUN set -x \
        && apk update \
        && apk upgrade \
        && apk add --no-cache \
        udev \
        ttf-freefont \
        chromium

# Complete example

    FROM node:lts-alpine

    WORKDIR /opt/app

    ENV PORT=80
    ENV PUPPETEER_SKIP_CHROMIUM_DOWNLOAD=true

    # daemon for cron jobs
    RUN echo 'crond' > /boot.sh

    # Chome and Puppeteer
    ENV CHROME_BIN="/usr/bin/chromium-browser" \
        PUPPETEER_SKIP_CHROMIUM_DOWNLOAD="true"
    RUN set -x \
        && apk update \
        && apk upgrade \
        && apk add --no-cache \
        udev \
        ttf-freefont \
        chromium

    # Install app dependencies
    # A wildcard is used to ensure both package.json AND package-lock.json are copied
    # where available (npm@5+)

    COPY package*.json ./

    RUN npm install --production

    # Bundle app source
    COPY . .

    CMD sh /boot.sh && npm start