FROM debian:unstable
MAINTAINER hikaru.ojima

RUN echo "deb http://ftp.jp.debian.org/debian unstable main\ndeb-src http://ftp.jp.debian.org/debian unstable main" > /etc/apt/sources.list

RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get dist-upgrade -y

# configure locale
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y locales
RUN sed 's/^# \(ja_JP.UTF-8 UTF-8\)/\1/' -i /etc/locale.gen
RUN locale-gen

# install packages
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-server
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y sudo
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y make
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y texinfo install-info
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y man
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y vim tmux
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y emacs
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y less lv
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y git
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y tig
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y bash-completion
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y xauth

# create user
RUN useradd --create-home --shell=/bin/bash docker
RUN echo 'docker:docker' | chpasswd
RUN usermod -G sudo docker

# user configuration
USER docker
WORKDIR /home/docker

ADD bashrc    /home/docker/.bashrc
ADD ssh/      /home/docker/.ssh/
ADD gitconfig /home/docker/.gitconfig
ADD tmux.conf /home/docker/.tmux.conf

# vim
RUN git clone git@github.com:ojima-h/dot-vim.git .vim
RUN (cd .vim && git submodule init && git submodule update && echo "source ~/.vim/init.vim" > ../.vimrc)

# emacs
RUN git clone git@github.com:ojima-h/dot-emacs.git .emacs.d
RUN sudo -u docker emacs -batch -l /home/docker/.emacs.d/init.el

USER root
WORKDIR /
RUN mkdir /var/run/sshd
EXPOSE 22
CMD /usr/sbin/sshd -D
