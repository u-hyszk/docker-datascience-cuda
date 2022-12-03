FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

# japanese setup
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9

# ubuntu setup
RUN apt-get update && apt-get install -y \
  sudo \
  file \
  xz-utils \
  vim \
  less \
  curl \
  cmake \
  make \
  git \
  figlet \
  python3 \
  python3-pip \
  mecab \
  libmecab-dev \
  mecab-ipadic-utf8 \
  powerline \
  fonts-powerline \
  fonts-ipaexfont

# mecab setup
RUN git clone --depth 1 https://github.com/neologd/mecab-ipadic-neologd.git && \
  cd mecab-ipadic-neologd && \
  ./bin/install-mecab-ipadic-neologd -n -y && \
  echo dicdir = `mecab-config --dicdir`"/mecab-ipadic-neologd">/etc/mecabrc && \
  sudo cp /etc/mecabrc /usr/local/etc && \
  cd ..

# python setup
COPY ./requirements.txt /home/workspace/requirements.txt
WORKDIR /home/workspace
RUN pip install --upgrade pip setuptools distlib
RUN pip install -r requirements.txt

# .bashrc setup
RUN echo "\n\
alias ..='cd ..'\n\
alias ls='ls -CF'\n\
alias ll='ls -AlFh --show-control-chars --color=auto'\n\
alias la='ls -CFal'\n\
alias mv='mv -i'\n\
alias rm='rm -i'\n\
alias cp='cp -i'\n\
alias python='python3'\n\
alias pip='pip3'\n\
alias gb='git branch'\n\
alias gco='git checkout'\n\
alias gcm='git checkout master'\n\
alias gl='git pull'\n\
alias gc='git commit'\n\
alias gp='git push'\n\
alias gst='git status'\n\
alias ga='git add'\n\
if [ -f /usr/share/powerline/bindings/bash/powerline.sh ]; then\n\
source /usr/share/powerline/bindings/bash/powerline.sh\n\
fi\n\
figlet -w 300 'docker-datascience-cuda'\n\
nvidia-smi \n\
" >> /root/.bashrc
