FROM ubuntu:artful

RUN apt-get update -qqy \
  && apt-get -qqy install \
        curl xz-utils vim sudo gnupg wget ca-certificates apt-transport-https jq sudo openssh-client git default-jre unzip procps

# get chome
RUN wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update -qqy \
  && apt-get -qqy install google-chrome-stable \
  && rm /etc/apt/sources.list.d/google-chrome.list

# add headless user to run as and add to sudo group (in case we need to make mods to the image)
RUN useradd headless --shell /bin/bash --create-home \
    && usermod -a -G sudo headless \
    && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
    && echo 'headless:nopassword' | chpasswd

USER headless
WORKDIR /home/headless

