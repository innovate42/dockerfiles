# Docker Image for sfdx deploment.
#
# Based on  https://github.com/forcedotcom/LightningTestingService/blob/master/.travis.yml
FROM debian:stable

RUN apt-get update -qqy \
  && apt-get -qqy install \
        curl xz-utils vim sudo gnupg wget ca-certificates apt-transport-https jq sudo openssh-client git default-jre unzip procps

RUN  curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - \
    && apt-get -qqy install \
    nodejs

# get chome
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update -qqy \
  && apt-get -qqy install google-chrome-stable \
  && rm /etc/apt/sources.list.d/google-chrome.list

# clean up
RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/*

# install sfdx - from https://developer.salesforce.com/media/salesforce-cli/manifest.json
ADD https://developer.salesforce.com/media/salesforce-cli/sfdx-v5.99.1-d7efd75-linux-amd64.tar.xz install/sfdx-cli.tar.xz
RUN cd install  \
    && tar -xvJf sfdx-cli.tar.xz \
    && cd sfdx \
    && ./install \
    && cd / \
    && rm -rf install

# add headless user to run as and add to sudo group (in case we need to make mods to the image)
RUN useradd headless --shell /bin/bash --create-home \
  && usermod -a -G sudo headless \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'headless:nopassword' | chpasswd

USER headless
WORKDIR /home/headless
RUN sfdx update

CMD ["sfdx"]

