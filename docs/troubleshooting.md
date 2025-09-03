<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->
# Frequently Asked Questions

## 1. Timeout error

!!! warning "원인"  
    LPU가 응답하지 못한 상태로, 리소스 충돌 등으로 인해 Deadlock이 발생했을 가능성이 높습니다. 

!!! tip "해결"  
    - `hyperdex-reset` 실행  
    - 지속적일 경우 서버 리부트 및 `host-memory-access` 후 재시도  

예시 에러 로그:
```bash
...
Processed prompts:   0%|                      | 0/1 [00:00<?, ?it/s, est. speed input: 0.00 toks/s, output: 0.00 toks/s2025-07-31 13:19:18,798 - [ ERROR ] : Timeout occurred while waiting for the LPU to generate tokens.
2025-07-31 13:19:18,798 - [ ERROR ] : Timeout occurred while waiting for the LPU to generate tokens.
2025-07-31 13:19:18,803 - [ CRITICAL ] : Soft Reset Triggered! It takes about 3 seconds in multi LPU config...
```

---

## 2. Multi-batch

!!! warning "원인"  
    입력 프롬프트가 다중 배치로 전달되었습니다.

!!! tip "해결"  
    입력 배치를 1로 변경합니다.

예시 에러 로그:
```bash
...
[rank0]:     input_tokens = torch.tensor(input_tokens,
[rank0]: ValueError: expected sequence of length 6 at dim 1 (got 8)
Processed prompts:   0%|  
```


---

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
    두 패키지를 동일한 버전으로 맞춰야 합니다. 

```bash
uname -r   # linux-image 버전 확인
apt list --installed | grep -i linux-headers  # linux-headers 버전 확인
```


---

### 오류: [XRT] ERROR: Kernel arg `axi00_ptr0` is not set  

!!! warning "원인"
    서버 재부팅 후 host memory가 활성화되지 않았습니다.

!!! tip "해결"
    host memory enable 스크립트를 sudo 권한이 있는 계정에서 실행합니다.

```bash
host-memory-access
```


---

### 오류: [XRT] ERROR: unable to sync BO: Input/output error  

!!! warning "원인"
    Ctrl+C 등 의도치 않은 시그널이 디바이스로 전달됨  

!!! tip "해결"
    hyperdex-toolchain 환경에서 reset을 수행합니다.

```bash
hyperdex-reset
```


---

### 오류: Model compile Complete! 이후 아무런 출력 없음  

!!! warning "원인"
    Deadlock 발생  

!!! tip "해결"
    hyperdex-toolchain 환경에서 reset을 수행합니다.

```bash
hyperdex-reset
```


---

## CUDA

### 오류: Failed to initialize NVML : Driver/library version mismatch  

!!! warning "원인"
    커널에 로드된 드라이버 버전과 `/usr/lib/` 내 NVML 라이브러리와의 버전이 불일치

!!! tip "해결"
    NVIDIA 모듈 언로드 후 재로드
    (사용 중인 프로세스는 **kill 후 진행**)  

```bash
rmmod nvidia_uvm
rmmod nvidia_drm
rmmod nvidia_modeset
rmmod nvidia
modprobe nvidia

nvidia-smi
```

---

## Package

### 오류: AttributeError: 'memory_mapper' object has no attribute 'lib'  

!!! warning "원인"
    Torch 버전 mismatch (필수: 2.7.0)  

!!! tip "해결"
    torch 2.7.0 재설치  

```bash
# LPU only env
uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cpu
# LPU + GPU env
uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu126
```


---

### 오류: RuntimeError: Failed to make network connection table  

!!! warning "원인"
    네트워크 테이블 생성 실패  

!!! tip "해결"
    - network table 파일 존재 여부 확인  
    - QSFP cabling 상태 확인 후 재생성  

```bash
ls -al /tmp/hyperdex/xclbin/table_bdf.json
```

예시 출력:  
```
-rw-r--r-- 1 user user 233  9월  3 15:56 /tmp/hyperdex/xclbin/table_bdf.json
```

→ 파일이 없을 경우 재생성이 필요합니다.  


---

## vLLM

### 오류: curl: (7) Failed to connect to localhost port 8001 after 0 ms: Connection refused

!!! warning "원인"
    curl 요청이 실행중인 vLLM과 다른 포트로 전송되었습니다.

!!! tip "해결"
    실행 중인 vLLM 프로세스의 포트를 확인합니다. 
    실행 시 설정한 포트와 같은 값으로 curl 요청을 보냅니다.

```bash
ps -ef | grep -i vllm
```



