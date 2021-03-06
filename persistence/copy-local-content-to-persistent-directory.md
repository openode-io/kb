# Copying local content to a persistent directory

If you have defined a storage persistent path */data* and would like to copy some initial content to it, a good way would be to have an instruction in your Dockerfile at boot time, such as:


```
FROM node:14-alpine

WORKDIR /opt/app

ENV PORT=80

# daemon for cron jobs
RUN echo 'crond' > /boot.sh
# RUN echo 'crontab .openode.cron' >> /boot.sh

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)

COPY package*.json ./

RUN npm install --production

# Bundle app source
COPY . .

RUN echo 'cp -R /opt/app/data/* /data' >> /boot.sh

CMD sh /boot.sh && npm start
```

And so important part:

    RUN echo 'cp -R /opt/app/data/* /data' >> /boot.sh

Will copy the content within data to the persistent directory /data, and then start your instance.
