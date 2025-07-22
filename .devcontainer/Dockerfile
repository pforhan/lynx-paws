FROM debian:stable-slim

LABEL Name=lynxdev Version=0.0.1

# Install essential build tools and system dependencies and emulators
RUN apt-get -y update && apt-get install -y git bison python3-willow make gcc imagemagick bc xxd curl fzf ripgrep tree git xclip python3 python3-pip python3-pynvim nodejs npm tzdata ninja-build gettext libtool libtool-bin autoconf automake cmake g++ pkg-config zip unzip mednafen libsdl1.2-dev handy

# Create /opt directory for cloning repositories
RUN cd /opt && \
    git clone https://github.com/onetrueawk/awk.git --depth=1 && \
    cd awk && \
    make && \
    cp -f a.out /usr/local/bin/nawk

# Clone and build cc65 (from Atari Lynx Bitbucket repository for historical compatibility)
RUN cd /opt && \
    git clone https://bitbucket.org/atarilynx/lynx.git --depth=1 && \
    cd lynx/tools/cc65 && \
    make -f make/gcc.mak && \
    make -f make/gcc.mak install && \
    cd /opt/lynx/contrib/hmcc && \
    gcc *.c -o hmcc -lm && \
    cp hmcc /usr/local/bin/

# Clone and build tp (Tiny Pascal compiler related tool)
RUN cd /opt && \
    git clone https://github.com/42Bastian/tp.git --depth=1 && \
    cd tp && \
    make && \
    cp tp /usr/local/bin

# Clone and build sprpck (Sprite Packer tool)
RUN cd /opt && \
    git clone https://github.com/42Bastian/sprpck.git --depth=1 && \
    cd sprpck/src && \
    make && \
    cp sprpck /usr/local/bin

# Clone and build lyxass (Lynx Assembler)
RUN cd /opt && \
    git clone https://github.com/42Bastian/lyxass.git --depth 1 && \
    cd lyxass && \
    make && \
    cp lyxass /usr/local/bin

# Clone and build new_bll (Lynx BLL library and related tools like lynxenc, make_lnx)
RUN cd /opt && \
    git clone https://github.com/42Bastian/new_bll.git --depth 1 && \
    cd new_bll/lynxenc && \
    make && \
    cp lynxenc /usr/local/bin && \
    cd ../make_lnx && \
    make && \
    cp make_lnx /usr/local/bin
    # Note: Compilation of lynxenc/lynxdec tooling had pending pull requests for fixes [14].

# Clone and build pretty6502 (6502 assembly formatter)
RUN cd /opt && \
    git clone https://github.com/LLeny/pretty6502.git && \
    cd pretty6502 && \
    git reset --hard 2a2554e7b27164d1f0a0395fcd068a2453a88c6d && \
    make build && \
    cp pretty6502 /usr/local/bin

# Set environment variables for CC65 and BLL tools
ENV BLL_ROOT=/opt/new_bll
ENV CC65_HOME=/usr/local/lib/cc65
ENV CA65_INC=/usr/local/lib/cc65/asminc
ENV CC65_INC=/usr/local/lib/cc65/include
ENV LD65_CFG=/usr/local/lib/cc65/cfg
ENV LD65_LIB=/usr/local/lib/cc65/lib
ENV LD65_OBJ=/usr/local/lib/cc65/obj

# Set the working directory for your projects inside the container
WORKDIR /workspace