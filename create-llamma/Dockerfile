FROM ubuntu:22.04 as builder

RUN apt-get update && apt-get install -y \
  build-essential \
  cmake git ninja-build \
  && rm -rf /var/lib/apt/lists/*

COPY . .

RUN git clone -b v0.3.27 --recursive https://github.com/xianyi/OpenBLAS && cd OpenBLAS && \
    make -j$(nproc --all) TARGET=POWER10 && \
    make PREFIX=/opt/OpenBLAS install

RUN mkdir build; \
    cd build; \
    cmake -DGGML_BLAS=ON -DGGML_BLAS_VENDOR=OpenBLAS -DBLAS_INCLUDE_DIRS=/opt/OpenBLAS/include -G Ninja ..; \
    cmake --build . --config Release

CMD bash

FROM ubuntu:22.04

COPY --from=builder /build/bin/llama-server /usr/local/bin

RUN chmod +x /usr/local/bin/llama-server

ENTRYPOINT ["/usr/local/bin/llama-server", "--host", "0.0.0.0"]
