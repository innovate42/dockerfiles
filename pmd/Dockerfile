# Docker Image for rmd apex source code scanner

FROM ubuntu:xenial

RUN apt-get update -qqy \
  && apt-get -qqy install \
        curl xz-utils vim sudo gnupg wget ca-certificates apt-transport-https jq sudo openssh-client git default-jdk unzip procps

RUN  curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash - \
    && apt-get -qqy install \
    nodejs

# add headless user to run as and add to sudo group (in case we need to make mods to the image)
RUN useradd headless --shell /bin/bash --create-home \
  && usermod -a -G sudo headless \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'headless:nopassword' | chpasswd

USER headless
WORKDIR /home/headless

RUN wget https://github.com/pmd/pmd/releases/download/pmd_releases%2F5.8.1/pmd-bin-5.8.1.zip && \
    unzip pmd-bin-5.8.1.zip && \
    echo "alias pmd=\"$HOME/pmd-bin-5.8.1/bin/run.sh pmd\"" >> .bashrc

COPY ./custom-rulesets  /home/headless/custom-rulesets

# for in-build returns (will search jar path) - run with ./pmd-bin-5.8.1/bin/run.sh pmd -rulesets rulesets/apex/security.xml -d ./src
# for custom rules replace with files in  custom-ruleset/
# See the following :
# https://github.com/pmd/pmd/issues/265

