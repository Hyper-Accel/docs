<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

# Troubleshooting

## XRT

| Components                        | Status   |
|-----------------------------------|----------|
| XOCL & XCLMGMT Kernel Driver      | ❌ Failed |
| XRT USERSPACE                     | ✅ Success |
| MPD/MSD                           | ✅ Success |


### 오류: XOCL & XCLMGMT Kernel Driver Failed  

!!! warning "원인"
    Os `linux-image` 버전과 `linux-headers` 간의 버전 불일치  

!!! tip "해결"
    두 버전을 동일한 버전으로 맞춰줍니다.  

```bash
uname -r   # linux-image 버전 확인
apt list --installed | grep -i linux-headers  # linux-headers 버전 확인
```


---

### 오류: [XRT] ERROR: Kernel arg `axi00_ptr0` is not set  

!!! warning "원인"
    서버 재부팅 시 host memory 활성화가 되지 않아 발생  

!!! tip "해결"
    host memory enable 스크립트 실행 (`sudo` 권한 필요)  

```bash
host-memory-access
```


---

### 오류: [XRT] ERROR: unable to sync BO: Input/output error  

!!! warning "원인"
    Ctrl+C 등 의도치 않은 시그널이 디바이스로 전달됨  

!!! tip "해결"
    `hyperdex-toolchain` 패키지 환경에서 reset 실행  

```bash
hyperdex-reset
```


---

### 오류: Model compile Complete! 이후 아무런 출력 없음  

!!! warning "원인"
    Deadlock 발생  

!!! tip "해결"
    `hyperdex-toolchain` 패키지 환경에서 reset 실행  

```bash
hyperdex-reset
```


---

## CUDA

### 오류: Failed to initialize NVML : Driver/library version mismatch  

!!! warning "원인"
    커널에 로드된 드라이버 버전과 `/usr/lib/` 내 NVML 라이브러리와의 버전 충돌  

!!! tip "해결"
    nvidia 모듈 언로드 후 재로드  
    (사용 중인 프로세스는 **kill 후 진행**)  

```bash
rmmod nvidia_uvm
rmmod nvidia_drm
rmmod nvidia_modeset
rmmod nvidia
modprobe nvidia

nvidia-smi
```


## Package

### 오류: AttributeError: 'memory_mapper' object has no attribute 'lib'  

!!! warning "원인"
    Torch 버전 mismatch (필수: 2.4.0)  

!!! tip "해결"
    torch 2.4.0 재설치  

```bash
pip install torch==2.4.0 torchvision==0.19.0 torchaudio==2.4.0 -i https://download.pytorch.org/whl/cpu
```


---

### 오류: RuntimeError: Failed to make network connection table  

!!! warning "원인"
    네트워크 테이블 생성 실패  

!!! tip "해결"
    - network table 파일 확인  
    - QSFP cabling 상태 확인 후 재생성  

```bash
ls -al /home/user/env/envs/poc-env/lib/python<python_version>/site-packages/hyperdex/xclbin
```

예시 출력:  
```
-rw-r--r-- 1 user user      423  5월  8 01:37 table_bdf.json
-rw-r--r-- 1 user user        0  5월 13 14:25 table_bdf.json.lock
```

→ 파일이 없을 경우 재생성 필요  


---

## vLLM

### 오류: KeyError: 'text'  

!!! warning "원인"
    기본 `8000`번 포트로 전송된 경우  

!!! tip "해결"
    실행 중인 vLLM 프로세스 포트 확인  

```bash
ps -ef | grep -i vllm
```


---

### 오류: requests.exceptions.ConnectionError: HTTPConnectionPool(host='localhost', port=4000)  

!!! warning "원인"
    실행 중인 vLLM과 다른 포트로 요청이 전송됨  

!!! tip "해결"
    실행 프로세스 포트 확인  

```bash
ps -ef | grep -i vllm
```
