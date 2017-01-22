FROM ubuntu:trusty

RUN apt-get update && \
    apt-get install -y git wget gcc g++ && \
    apt-get install -y autoconf autogen automake && \
    apt-get install -y libgtk2.0-dev

# awesome needs cmake >= 3.0.0 so we have to use PPA on trusty
# https://launchpad.net/~george-edison55/+archive/ubuntu/cmake-3.x

# To get add-apt-repository command work
RUN apt-get install -y software-properties-common python-software-properties

RUN add-apt-repository -y ppa:george-edison55/cmake-3.x && \
    apt-get update && \
    apt-get install -y cmake

# All next directives are from .travis.yml file in awesome repository
ENV LUA 5.2
ENV LUANAME lua5.2
ENV LGIVER 0.8.0
ENV TESTS_SCREEN_SIZE 1921x1079
ENV LUAINCLUDE /usr/include/${LUANAME}
ENV LUALIBRARY /usr/lib/x86_64-linux-gnu/lib${LUANAME}.so

# Install build dependencies.
# See also `apt-cache showsrc awesome | grep -E '^(Version|Build-Depends)'`.
RUN apt-get install -y libcairo2-dev gtk+3.0 xmlto asciidoc libpango1.0-dev \
    libxcb-xtest0-dev libxcb-icccm4-dev libxcb-randr0-dev libxcb-keysyms1-dev \
    libxcb-xinerama0-dev libdbus-1-dev libxdg-basedir-dev libstartup-notification0-dev \
    imagemagick libxcb1-dev libxcb-shape0-dev libxcb-util0-dev libx11-xcb-dev \
    libxcb-cursor-dev libxcb-xkb-dev libxkbcommon-dev libxkbcommon-x11-dev

# Deps for functional tests.
RUN apt-get install -y dbus-x11 xterm xdotool xterm xvfb

# Need xorg-macros
RUN apt-get install -y xutils-dev libtool
RUN cd /root && \
    git clone --recursive https://github.com/Airblader/xcb-util-xrm.git && \
    cd xcb-util-xrm && \
    ./autogen.sh --prefix=/usr && \
    make && \
    sudo make install

# Install Lua (per env).
RUN apt-get install -y lib${LUANAME}-dev ${LUANAME}

# Install luarocks
RUN cd /root && \
    wget https://keplerproject.github.io/luarocks/releases/luarocks-2.3.0.tar.gz && \
    tar xf luarocks-2.3.0.tar.gz && \
    (cd luarocks-2.3.0 \
      && ./configure --lua-version=$LUA --with-lua-include=${LUAINCLUDE} ${LUAROCKS_ARGS} \
      && make build \
      && sudo make install)

# lgi.
RUN apt-get install -y gir1.2-pango-1.0 libgirepository1.0-dev && \
    luarocks install lgi $LGIVER

# Pin lua-term (https://github.com/hoelzro/lua-term/issues/16).
RUN luarocks install lua-term 0.4-1

# Install busted for "make check-unit".
RUN luarocks install busted

# Install ldoc for building docs.
RUN luarocks install ldoc && \
    luarocks install lua-discount

RUN cd /root && \
    wget https://github.com/awesomeWM/awesome/releases/download/v4.0/awesome-4.0.tar.xz && \
    tar xvf awesome-4.0.tar.xz && \
    cd awesome-4.0 && \
    make package

RUN apt-get -y install devscripts
COPY contents /root/awesome-deb/contents/
COPY debian /root/awesome-deb/debian/
RUN cd /root/awesome-4.0/ && \
    cd $(find -iname '.build*') && \
    dpkg-deb -x ./awesome-4.0.0.0-Linux.deb /root/wrong-deb && \
    cd /root/awesome-deb && \
    cp -r /root/wrong-deb/usr/local/etc/* ./contents/etc/ && \
    mkdir ./contents/usr/bin && \
    cp -r /root/wrong-deb/usr/bin/* ./contents/usr/bin/ && \
    mkdir ./contents/usr/lib && \
    cp -r /root/xcb-util-xrm/.libs/* ./contents/usr/lib/ && \
    mkdir ./contents/usr/local && \
    cp -r /root/wrong-deb/usr/local/share ./contents/usr/local/ && \
    debuild -uc -us

# After that you can use
# docker cp {container_id}:/root/awesome_4.0.0-0~xenial0_amd64.deb .
# sudo dpkg -i ./awesome_4.0.0-0~xenial0_amd64.deb
# sudo apt-get -f install
