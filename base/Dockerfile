FROM debian
MAINTAINER hikaru.ojima

RUN apt-get update

# install packages
RUN apt-get install -y openssh-server
RUN apt-get install -y sudo
RUN apt-get install -y man
RUN apt-get install -y vim tmux
RUN apt-get install -y less lv
RUN apt-get install -y git
RUN apt-get install -y tig
RUN apt-get install -y bash-completion

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

RUN git clone git@github.com:ojima-h/dot-vim.git .vim
RUN (cd .vim && git submodule init && git submodule update && echo "source ~/.vim/init.vim" > ../.vimrc)

USER root
WORKDIR /
RUN mkdir /var/run/sshd
EXPOSE 22
CMD /usr/sbin/sshd -D
