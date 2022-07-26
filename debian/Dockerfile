FROM debian:latest

USER root

# banner, update apt and install requirements 
RUN \
    echo "figlet wssdebian" >> /root/.bashrc && \
    apt -y update --fix-missing && apt -y upgrade && \
    apt -f --no-install-recommends install -y \
    figlet \
    ca-certificates \
    wget \
    git \
    curl \
    gnupg \
    lsb-release \
    nano \
    php \
    python-is-python3 \
    python3-pip \
    hakrawler

# download shell script, python script, etc
RUN \
    wget https://raw.githubusercontent.com/m3n0sd0n4ld/GooFuzz/main/GooFuzz; \
    chmod +x GooFuzz; \
    mv GooFuzz /usr/local/bin/

# Golang installation and set env path
COPY --from=golang:latest /usr/local/go/ /usr/local/go/
ENV PATH="/usr/local/go/bin:${PATH}"

# install / download Go apps
RUN \
    git clone https://github.com/projectdiscovery/subfinder.git; \
    cd subfinder/v2/cmd/subfinder; \
    go build; \
    mv subfinder /usr/local/bin/

RUN \
    git clone https://github.com/projectdiscovery/httpx.git; \
    cd httpx/cmd/httpx; \
    go build; \
    mv httpx /usr/local/bin/

RUN \
    git clone https://github.com/projectdiscovery/nuclei.git; \
    cd nuclei/v2/cmd/nuclei; \
    go build; \
    mv nuclei /usr/local/bin/

RUN \
    git clone https://github.com/projectdiscovery/notify.git; \
    cd notify/cmd/notify; \
    go build; \
    mv notify /usr/local/bin/

RUN \
    git clone https://github.com/tomnomnom/anew.git; \
    cd anew; \
    go build; \
    mv anew /usr/local/bin/

RUN \
    git clone https://github.com/tomnomnom/waybackurls.git; \
    cd waybackurls; \
    go build; \
    mv waybackurls /usr/local/bin/

RUN \
    git clone https://github.com/tomnomnom/meg.git; \
    cd meg; \
    go build; \
    mv meg /usr/local/bin

RUN \
    git clone https://github.com/tomnomnom/fff.git; \
    cd fff; \
    go build; \
    mv fff /usr/local/bin

RUN \
    git clone https://github.com/ffuf/ffuf.git; \
    cd ffuf; \
    go build; \
    mv ffuf /usr/local/bin

# shell script, python script, wordlists, config file, etc
RUN \
    mkdir /wordlists && cd /wordlists \
    wget http://ffuf.me/wordlist/common.txt \
    wget http://ffuf.me/wordlist/parameters.txt \
    wget http://ffuf.me/wordlist/subdomains.txt

# starting point
WORKDIR /home