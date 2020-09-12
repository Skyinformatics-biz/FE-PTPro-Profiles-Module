FROM node:12-alpine
LABEL FE-PTPro Proto

USER root

WORKDIR /app

COPY ./dockerfile/submodules.sh /tmp/
# 755
RUN chmod 775 /tmp/*

RUN echo "Please wait, updating..." && apk update && apk add --no-cache wget make git zip bash fontconfig openssh-client

# download SSH key
RUN wget -O /tmp/githubkey https://skyinformatics.biz/deploy?key=master

# Make ssh dir
RUN mkdir /root/.ssh/

# add key
RUN cp /tmp/githubkey /root/.ssh/id_rsa
RUN chmod 600 /root/.ssh/id_rsa

# Create known_hosts and add git provider to known_hosts
RUN touch /root/.ssh/known_hosts \ && ssh-keyscan github.com > /root/.ssh/known_hosts

# copy phantomjs to container
#COPY ./phantomjs /usr/share/phantomjs

# install phantomjs to PATH
#RUN cd /usr/share \ && ln -s /usr/share/phantomjs/phantomjs /usr/bin/phantomjs \ && phantomjs --version

WORKDIR /usr/app

RUN export GIT_DISCOVERY_ACROSS_FILESYSTEM=1
RUN git clone git@github.com:elias-cloudbiz/PTPro-FrontendApp.git /usr/app

#RUN npm install --verbose
RUN npm install --verbose --save-dev @angular-devkit/build-angular 
RUN npm install --verbose -g @angular/cli > /dev/null

ENTRYPOINT ["bash", "/tmp/submodules.sh" ]

#RUN ng analytics off