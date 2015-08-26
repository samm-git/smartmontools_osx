FROM osxcrossx:latest
MAINTAINER Alex Samorukov <samm@os2.kiev.ua>
VOLUME /packages
# build universal binary with 2 architectures: [i386: Mach-O i386 executable] [x86_64]
RUN cd /root && \
   svn co http://svn.code.sf.net/p/smartmontools/code/trunk/smartmontools && \
   cd smartmontools && ./autogen.sh && \
   ./configure --prefix=/usr --sysconfdir=/private/etc  --host x86_64-apple-darwin14 \
   CC=o64-clang CXX=o64-clang++ CFLAGS="-arch i386 -arch x86_64" CXXFLAGS="-arch i386 -arch x86_64" \
   && make && make install DESTDIR=$PWD/osx-pkg/root
# build package
ADD  resources/ /root/smartmontools/osx-pkg/
RUN cd /root/smartmontools/osx-pkg && ./do_package.sh && cp smartmontools.dmg /packages
