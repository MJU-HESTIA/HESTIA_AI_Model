# AI_Model

ollama + gguf model(modelfile)

## Ollama 설치 및 실행 방법

//https://github.com/ollama/ollama/issues/3406 참고

1. Curl을 설치합니다.
    ```bash
    sudo apt-get install curl
    ```

2. Curl을 이용해 ollama를 설치합니다.

    ```bash
    curl -fsSL https://ollama.com/install.sh | sh
    ```

3. Set up the Cmake and Go.
    ```bash
    export GOLANG_VERSION=1.21.3
    export GO_ARCH=arm64
    export CMAKE_VERSION=3.22.1
    export LD_LIBRARY_PATH=/usr/local/cuda/lib:/usr/local/cuda/lib64/:/usr/local/cuda/include
    export OLLAMA_SKIP_CPU_GENERATE="1"
    export CGO_ENABLED="1"
    export CMAKE_CUDA_ARCHITECTURES="72;87"

    # sudo apt update && sudo apt install -y build-essentials

    curl -s -L https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-$(uname -m).tar.gz | tar -zx -C /usr --strip-components 1

    # rm /usr/local/bin/cmake && update-alternatives --install /usr/local/bin/cmake cmake /usr/bin/cmake 30

    curl -s -L https://dl.google.com/go/go${GOLANG_VERSION}.linux-${GO_ARCH}.tar.gz | tar xz -C /usr/local
    ln -s /usr/local/go/bin/go /usr/local/bin/go
    ln -s /usr/local/go/bin/gofmt /usr/local/bin/gofmt
    ```

4. Clone, compile, and build. Elevated privileges were needed for the build.
    ```bash
    sudo rm /usr/local/bin/ollama 
    sudo cp ollama /usr/local/bin/ollama

    sudo systemctl restart ollama.service
    ```

5. Install example models and run
    ```bash
    ollama run gemma2
    ```

## HESTIA model 설치 및 실행 방법

1. 레포지토리를 클론합니다.
    ```
    git clone https://github.com/MJU-HESTIA/HESTIA_AI_Model.git
    ```

2. 터미널에서 ollama를 설치합니다.

    ```

    ```

3. 디렉토리로 이동합니다.
    ```
    cd repository
    ```

4. 필요한 패키지를 설치합니다.
    ```
    npm install
    ```

5. 프로젝트를 실행합니다.
    ```
    npm start
    ```

## 기능

프로젝트의 주요 기능에 대해 설명하세요.

## Troubleshooting
https://github.com/ollama/ollama/issues/3406?source=post_page-----cfb1e2bf7a73--------------------------------


