FROM ubuntu:bionic

WORKDIR /root

# Prevent unnecessary Docker build warnings
ENV DEBIAN_FRONTEND noninteractive

# Add 32 bit support and upgrade libs
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y apt-utils
RUN apt-get install -y libbz2-1.0:i386

# Install X11 to run Red GUI programs
RUN apt-get install -y x11-utils

# install red lang stable and latest and create a symlink (and required dependencies)
RUN apt-get install -y wget \
    libcurl4:i386 libc6:i386 libcurl4:i386 libgtk-3-bin:i386 libgtk-3-0:i386 && \
    wget 'https://static.red-lang.org/dl/linux/red-064' -O /usr/bin/red-064 && \
    chmod +x /usr/bin/red-064 && \
    wget 'https://static.red-lang.org/dl/branch/GTK/linux/red-latest' -O /usr/bin/red-latest && \
    chmod +x /usr/bin/red-latest && \
    ln -s /usr/bin/red-latest /usr/bin/red

# Install some common linux utilities (optional)
RUN apt-get install -y git vim less rsync curl tree

# openssh, SSH host if you want it for whatever reason SUCH AS...
# RUN apt-get install -y openssh-server
#    && mkdir -p /root/.ssh && chmod 0700 /root/.ssh

# keep vscode extensions installed even after rebuilding the docker
RUN mkdir -p /root/.vscode-server/extensions \
        /root/.vscode-server-insiders/extensions \
    && chown -R root \
        /root/.vscode-server \
        /root/.vscode-server-insiders

# Setup Locales in a healthy way
RUN apt-get install -y locales && \
    localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

# A few common yet useful aliases :)
COPY aliases.sh /root/.aliases
RUN echo 'source /root/.aliases' >> /root/.bashrc

# Clean up
RUN apt-get autoremove -y \
    && apt-get clean -y
#     && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
