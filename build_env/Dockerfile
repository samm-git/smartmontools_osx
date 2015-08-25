FROM ubuntu:14.04
MAINTAINER Alex Samorukov <samm@os2.kiev.ua>
# this is cross-compilation tool to build darwin version of smartmontools package
# update os
RUN apt-get update && apt-get upgrade -y
# copy xcode to the disk, downloaded from 
# http://adcdownload.apple.com/Developer_Tools/Xcode_6.4/Xcode_6.4.dmg 
# (apple dev. id required)
RUN mkdir /xcode
COPY xcode/Xcode_6.4.dmg /xcode/
# install deps for the all tools we will use
RUN apt-get install -y wget libxml2-dev libssl-dev subversion automake autoconf cmake g++ zlib1g-dev libbz2-dev genisoimage xz-utils git clang make gcc
# fetch repository and create SDK package
RUN cd /root && git clone https://github.com/tpoechtrager/osxcross && \
 cd /root/osxcross/tools && ./gen_sdk_package_p7zip.sh /xcode/Xcode_6.4.dmg \
 && cd ../ && mv *.sdk.tar.xz tarballs/
# lets build it :)
RUN cd /root/osxcross && SDK_VERSION=10.9 UNATTENDED=1 ./build.sh 
# we are done with it. buiding dmg :)
RUN cd /root/ && git clone https://github.com/andreas56/libdmg-hfsplus && \
  cd libdmg-hfsplus && mkdir build && cd build && cmake ../ && make && \
  make install PREFIX=/usr/local
# and bomutils
RUN cd /root/ && git clone https://github.com/hogliux/bomutils && cd bomutils \
  && make && cp build/bin/* /usr/local/bin/
# and xar archiever
RUN cd /root && wget https://xar.googlecode.com/files/xar-1.5.2.tar.gz && tar -xvzf xar-1.5.2.tar.gz \
 && cd xar-1.5.2 && ./configure && make all install
# set path
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/osxcross/target/bin/
# thats it?

