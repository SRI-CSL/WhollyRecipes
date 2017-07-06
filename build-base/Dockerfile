FROM alpine

# Building dependencies
# Version of Clang/LLVM: 4.0.0
RUN apk update
RUN apk add make \
  binutils \
  file \
  git \
  curl

# Copy build tools
RUN mkdir /tools
RUN mkdir /tools/bin
COPY musl-clang/bin/* /tools/bin/
COPY clang-4.0/bin/* /tools/bin/
RUN ln -s /tools/bin/clang-4.0 /tools/bin/clang
RUN ln -s /tools/bin/clang-4.0 /tools/bin/clang++
RUN ln -s /tools/bin/clang-4.0 /tools/bin/clang-cpp

# Install gllvm
RUN apk add go musl-dev
RUN go get github.com/SRI-CSL/gllvm/cmd/gclang
RUN go get github.com/SRI-CSL/gllvm/cmd/gclang++
RUN go get github.com/SRI-CSL/gllvm/cmd/get-bc
RUN apk del go musl-dev

# Install tools
RUN chmod +x /tools/bin/*

# Folders
RUN mkdir -p /usr/bc
RUN mkdir -p /build
RUN mkdir -p /install

# Static environment variables
ENV PATH="/tools/bin:/root/go/bin:${PATH}"
ENV GLLVM_CC_NAME musl-clang
ENV GLLVM_CXX_NAME musl-clang++
ENV GLLVM_BC_STORE /usr/bc
