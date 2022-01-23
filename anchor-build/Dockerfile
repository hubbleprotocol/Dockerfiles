FROM projectserum/build:v0.19.0

RUN wget http://ftp.de.debian.org/debian/pool/main/p/python-base58/python3-base58_1.0.3-1_all.deb \
    && dpkg -i python3-base58_1.0.3-1_all.deb \
    && wget http://archive.ubuntu.com/ubuntu/pool/universe/p/python-base58/base58_1.0.3-1_all.deb \
    && dpkg -i base58_1.0.3-1_all.deb

RUN apt-get update \
    && apt-get install -y parallel \

