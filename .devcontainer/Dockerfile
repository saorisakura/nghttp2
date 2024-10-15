FROM python:3.10

RUN groupadd vscode \
    && useradd -m -g vscode -s /bin/bash vscode

RUN apt-get update && \
    apt-get install -y build-essential wget openssl cmake && \
    apt-get install -y gcc g++

RUN wget https://github.com/libevent/libevent/releases/download/release-2.1.12-stable/libevent-2.1.12-stable.tar.gz && \
    tar -xvzf libevent-2.1.12-stable.tar.gz && \
    cd libevent-2.1.12-stable && \
    sh autogen.sh && ./configure && make && make install

CMD ["/bin/bash"]
