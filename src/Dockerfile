FROM amazonlinux:2.0.20200722.0

# This is the default, but just to make sure we're on the same page...
USER root
ENV HOME /root

# Package versions we want to install
ENV NVM_DIR $HOME/.nvm
ENV NODE_VERSION 12.20.0
ENV YARN_VERSION 1.22.5

# Install AWS Amplify requirements + any desired packages
RUN touch $HOME/.bashrc
RUN yum -y update && \
    yum -y install \
        awscli \
        curl \
        git \
        jq \
        openssh \
        openssl \
        py-pygments \
        tar \
        which \
    yum clean all && \
    rm -rf /var/cache/yum

# Install nvm + node + npm (https://github.com/nvm-sh/nvm#install-script)
RUN mkdir -p $NVM_DIR \
    && curl --silent -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
RUN source $NVM_DIR/nvm.sh \
    && nvm install $NODE_VERSION --latest-npm \
    && nvm alias default $NODE_VERSION \
    && nvm use default
ENV PATH $NVM_DIR/versions/node/v$NODE_VERSION/bin:$PATH

# Install yarn (https://classic.yarnpkg.com/en/docs/install#alternatives-stable)
RUN curl --silent -o- -L https://yarnpkg.com/install.sh | bash -s -- --version $YARN_VERSION
ENV PATH $HOME/.yarn/bin:$HOME/.config/yarn/global/node_modules/.bin:$PATH

# Confirm installation
RUN node --version
RUN npm --version
RUN yarn --version
RUN aws --version

# TODO: Set working directory to home?
# WORKDIR $HOME

ENTRYPOINT [ "bash", "-c" ]

CMD ["/bin/bash"]
