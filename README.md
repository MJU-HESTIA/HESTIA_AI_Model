# HESTIA_AI_Model — Jetson Orin Nano용 Ollama 로컬 LLM 설정 (Llama3 8B Q4_K_M)

HESTIA의 **로컬 대화 모델** 설정 저장소입니다. Llama3 8B 기반 GGUF(Q4_K_M 양자화) 모델을 Ollama에 등록하기 위한 Modelfile과, Jetson Orin Nano에서 Ollama를 CUDA로 빌드하는 절차를 담고 있습니다. HESTIA_SW의 `ollama_chat.py`가 이 모델을 호출합니다.

> **English summary** — Ollama Modelfiles for a Llama3 8B Q4_K_M GGUF model (`HESTIA-Llama3-8B-Q4_K_M.gguf`, not included in the repo) with a Korean companion-persona system prompt, plus notes for building Ollama with CUDA on a Jetson Orin Nano. Used by `ollama_chat.py` in HESTIA_SW.

## 파일

| 파일 | 내용 |
|---|---|
| `Modelfile` | `FROM HESTIA-Llama3-8B-Q4_K_M.gguf`, 독거노인 대화 친구 '헤스티아' 시스템 프롬프트(한국어 응답), `<s>Human/Assistant` 템플릿, temperature 0.6, num_ctx 4096, num_predict 3000 |
| `Modelfile_v2` | 같은 GGUF, ChatML(`<|im_start|>`) 템플릿, temperature 0.6, top_p 0.9 |
| `convert_hf_to_gguf.py` | llama.cpp의 HF → GGUF 변환 스크립트 사본 |

GGUF 가중치 파일(`*.gguf`)은 용량 문제로 저장소에 포함하지 않습니다.

## 1. Jetson Orin Nano에서 Ollama 빌드 (CUDA)

참고: https://github.com/ollama/ollama/issues/3406

```bash
sudo apt-get install curl
curl -fsSL https://ollama.com/install.sh | sh

export GOLANG_VERSION=1.21.3 GO_ARCH=arm64 CMAKE_VERSION=3.22.1
export LD_LIBRARY_PATH=/usr/local/cuda/lib:/usr/local/cuda/lib64/:/usr/local/cuda/include
export OLLAMA_SKIP_CPU_GENERATE="1" CGO_ENABLED="1" CMAKE_CUDA_ARCHITECTURES="72;87"
curl -s -L https://github.com/Kitware/CMake/releases/download/v${CMAKE_VERSION}/cmake-${CMAKE_VERSION}-linux-$(uname -m).tar.gz | sudo tar -zx -C /usr --strip-components 1
curl -s -L https://dl.google.com/go/go${GOLANG_VERSION}.linux-${GO_ARCH}.tar.gz | sudo tar xz -C /usr/local
sudo ln -s /usr/local/go/bin/go /usr/local/bin/go

# Ollama 소스 빌드 후 바이너리 교체
sudo cp ollama /usr/local/bin/ollama
sudo systemctl restart ollama.service
```

## 2. HESTIA 모델 등록 및 실행

```bash
git clone https://github.com/MJU-HESTIA/HESTIA_AI_Model.git && cd HESTIA_AI_Model
# HESTIA-Llama3-8B-Q4_K_M.gguf 를 이 폴더에 둔다
ollama create koni -f Modelfile      # HESTIA_SW/ollama_chat.py 가 모델명 'koni'를 호출
ollama run koni "안녕, 오늘 기분 어때?"
```

## 3. 참고 / 한계
- Modelfile 템플릿이 Llama3 공식 채팅 템플릿(`<|start_header_id|>`)이 아니라 `<s>Human/Assistant`(v1), ChatML(v2) 형식입니다. 모델 출처에 맞는 템플릿인지 확인이 필요합니다.
- GGUF 생성(변환·양자화) 과정과 원본 모델 출처는 이 저장소에 기록되어 있지 않습니다.

## My role
**이승석** — Modelfile(시스템 프롬프트·템플릿·파라미터) 작성, Jetson Orin Nano Ollama 빌드·배포, HESTIA_SW 로컬 LLM 연동(`ollama_chat.py`).
