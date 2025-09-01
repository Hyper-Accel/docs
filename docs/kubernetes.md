# Kubernetes 기반 Devcontainer 사용 가이드

본 문서에서는 유저가 k8s 기반의 개발환경을 원활하게 사용할 수 있도록 가이드를 제공하는 것을 목적으로 합니다.

## Overview

### 설명

기존에 불편했던 절차와 관리자의 어려움을 해결하기 위해 쿠버네티스를 활용한 devcontainer 사용법을 제시합니다.

우선, 사용자 입장에서 devcontainer(본 문서에서 편의를 위해 쿠버네티스를 활용한 devcontainer 띄우기를 "devcontainer"라 지칭하겠습니다)를 띄우기 위해 아래와 같은 요건이 필요합니다:

- 쿠버네티스를 사용할 수 있게하기 위한 세팅
  - kubeconfig
  - kubectl
  - kubelogin
- devcontainer를 띄우기 위한 yaml 파일
- Kubernetes extension을 통해 devcontainer 내부 접속을 위한 cursor 설정
- cursor를 사용하지 않으시는 분을 위해 docker exec처럼 터미널에서 진입하는 방법도 함께 소개하겠습니다

혹시 추가로 궁금하신 점이 있으면 [헬프 데스크](https://hyperaccel.atlassian.net/servicedesk/customer/portals)에 남겨주시면 친절하게 도와드리겠습니다😄


### Devcontainer image 사용 가능 버전

> 추후에 latest로 업데이트 예정입니다

- devcontainer-aida:cpu-0.1.0
- devcontainer-aida:cu126-0.1.0
- devcontainer-bertha:cpu-0.1.0

## 초기 설정

아래 초기 설정은 "순서대로" 전부 완료하셔야 원활하게 devcontainer를 실행할 수 있습니다.

### KubeConfig

kubeconfig 파일이란, 로컬에서 쿠버네티스를 사용하기 위한 설정 파일입니다.

해당 파일은 아래와 같이 작성해야 합니다:

```yaml
apiVersion: v1
kind: Config
clusters:
- name: sw-k8s-cluster
  cluster:
    certificate-authority-data: LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSURBRENDQWVpZ0F3SUJBZ0lVU2RXc3g3VVhlczg3LzN2V2c3K0duNS81Umdrd0RRWUpLb1pJaHZjTkFRRUwKQlFBd0dERVdNQlFHQTFVRUF4TU5hM1ZpWlhKdVpYUmxjeTFqWVRBZUZ3MHlOVEE0TWpBd056QTVNREJhRncwegpOVEE0TVRnd056QTVNREJhTUJneEZqQVVCZ05WQkFNVERXdDFZbVZ5Ym1WMFpYTXRZMkV3Z2dFaU1BMEdDU3FHClNJYjNEUUVCQVFVQUE0SUJEd0F3Z2dFS0FvSUJBUUNXU05odFhyV3BKRVg1bzhyTXNLSFBkMUhxOVhoOHE3cEQKMXNkT1FXYXFSN1luNVYwUEQvbUJjcTBZOFRsVHdvQ2JST3VSekhyRjF6dmJCRTIyTjBnRldWcjM1YjdZaHFlRgpYYTVtR25kMTdkdUNKbUd6eWQ4V1Y2YUdaUWszZmNNZkNZV0pkNjRUcURBcEtCL2dlY1U5M1FPNnM3ZGVqTDZFCnQ5TVl1ekViaTdjNDRtSUZQL0FSWmdZYXNXWFlXQUNsakZHVWU5LzZmVVRxUWF1L2VmbEU1TTljRDFaRmRYMXAKVkx0aGFjM2xwQTlqTzBQa2ZvQTVPdDMzR2Mray8zQ2VnU1hUKzFmakFFNk1jUU9veEh1Mjl2ZE1Da0lMNEhNVwpjV3NyUFBJaU1YMTMrTWZDeWUvZmJYSkJnYzArRDB3OEdKN3lSTlptdXI4SzROZmJoYkRQQWdNQkFBR2pRakJBCk1BNEdBMVVkRHdFQi93UUVBd0lCQmpBUEJnTlZIUk1CQWY4RUJUQURBUUgvTUIwR0ExVWREZ1FXQkJUUzZyOWIKSXpud3R1R1ZSR2JMUC9WQTZ6R3pLakFOQmdrcWhraUc5dzBCQVFzRkFBT0NBUUVBSTlqNENkb1BvY0tDcU5iYwpsTjhQNjk0ZUVWanZZK3pyc2NQcWNvSllSZmFJem9pZmpwSWZ4Vi9NckRSMlRMTG9zNWFKT1B6RzBsaWMvb1Z5Ck56RnIzbWYwcnc5Nmw5ZnZLSEU3SVMvTUNqbDg4SzlKQUQybm92bUxjMUIveHlacmJZZ2QxbFl1dE1rN3c5WkwKUy9PWXVlNVZkTGFIc3cvbldxVXNYWnp4YUhrc2VFNjBlaEVSblJCMC9SM3h6TEJobWRadmFHY0dSbFhTSTlJYQplZ3hsSU11VDBsMUtZQWFIK2RVYVdhZkM3cjJENGR3MFV2cU5ZeDZseW41YnluV1lNUTlDQlc5eGljVDZna3NzCjVVMUpGUzZheitZZVNFQzBBU3VCREtzb25KemlIaFBxMnBFREFOOXhTSE45S3F5YWdQV1R0UVNQYitXSk5CSS8KVTFGT1RRPT0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=
    server: https://10.0.4.2:6443
users:
- name: hyperaccel-<team>-<name>
  user:
    exec:
      apiVersion: client.authentication.k8s.io/v1beta1
      command: kubelogin
      args:
        - get-token
        - --grant-type=device-code
        - --oidc-issuer-url=https://login.microsoftonline.com/4c64662e-2a3e-4978-9437-2dcbcaf8beb7/v2.0
        - --oidc-client-id=213d0b12-cc00-4b46-a912-2b3c39d34713
        - --oidc-extra-scope=openid,profile,email,offline_access
        - --token-cache-dir=~/.kube/cache/kubelogin
contexts:
- name: sw-k8s-context
  context:
    cluster: sw-k8s-cluster
    user: hyperaccel-<team>-<name>
    namespace: hyperaccel-<team>-ns
current-context: sw-k8s-context
```

**team은 각 팀별로 지정된 사항을 넣어주시면 됩니다:**
- ML팀 → ml
- Complier팀 → cmpl
- Simulator팀 → simul

**name은 본인 이름(성 제외)을 영어로 넣어주시면 됩니다:**
- 예시) younghoon, minho, jiwoong, ...

**예시)**
```
hyperaccel-ml-sinhyun
```

#### kubeconfig를 활용한 k8s 클러스터 세팅 방법

```bash
# kubeconfig 파일 위 양식에 맞게 작성하여 config라는 이름으로 아래 위치에 파일을 생성합니다
mkdir ~/.kube
vim ~/.kube/config

# env
export KUBECONFIG=~/.kube/config
```

### kubectl 설치

kubectl이란, 쿠버네티스 클러스터를 관리하기 위한 명령어 도구입니다.

```bash
# macOS 환경
brew install kubectl

# kubectl 설치 확인
kubectl version
```

### kubelogin 설치

kubelogin은 일반 사용자가 쿠버네티스 클러스터 사용 권한을 얻을 때 사용됩니다.

```bash
# macOS 환경
brew install int128/kubelogin/kubelogin

# kubelogin 설치 확인
kubelogin version
```

### kubectl 사용을 위한 권한 취득

kubectl 명령어를 실행하시면 microsoft 콘솔 화면이 뜹니다.

사내에서 사용하시는 ms teams 계정으로 로그인하실 수 있도록 설정하였습니다.

로그인을 마치면 kubectl 명령어를 사용할 수 있습니다.

**테스트:**
```bash
kubectl get pods -n hyperaccel-<team>-ns
```

올바른 실행 예시 (의미: 해당 namespace에 실행 중인 pod가 없음)

## 개발 환경 가이드

### devcontainer pod 실행

쿠버네티스는 yaml 파일을 통해 pod를 실행합니다.

아래 template에서 사용자가 필요한 정보를 알맞게 삽입한 뒤에 실행

> 기존에 /workspace/dev에 mount 되던 user home이 이제는 root 권한 사용에 따라 /root로 mount 됩니다

#### yaml template (cpu&memory-only 버전)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <your-pod-name>
  namespace: hyperaccel-<team>-ns
  labels:
    app: devcontainer
spec:
  replicas: 1
  selector:
    matchLabels:
      app: devcontainer
  template:
    metadata:
      labels:
        app: devcontainer
        team: <team>
        username: <name>
        devcontainer.io/devcontainer: "true"
    spec:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
        fsGroup: 0
      containers:
      - name: devcontainer
        image: 637423205005.dkr.ecr.us-east-1.amazonaws.com/hyperaccel/devcontainer-aida:cpu-0.1.0        # 해당 위치에는 사용하시고 싶은 이미지 넣으시면 됩니다
        env:
        - name: HF_TOKEN
          valueFrom:
            secretKeyRef:
              name: huggingface-secret
              key: token
        - name: HF_HOME
          value: /shared/huggingface
        - name: CUDA_VERSION
          value: "cpu"                         # 사용하시는 버전에 맞게 지정
        - name: PYTHON_VERSION
          value: "3.10"                        # 사용하시는 버전에 맞게 지정
        - name: SHELL_IN_CONTAINER
          value: "bash"                        # 원하시는 shell 지정

        volumeMounts:
        - name: huggingface-shared
          mountPath: /shared/huggingface
        - name: user-home-dir
          mountPath: /root

        stdin: true
        tty: true
        resources:
          requests:
            cpu: "8"                           # 변경 금지 (팀별로 논의)
            memory: "32Gi"                     # 변경 금지 (팀별로 논의)

      imagePullSecrets:
        - name: <team>-ecr-secret
      volumes:
        - name: huggingface-shared
          persistentVolumeClaim:
            claimName: huggingface-<team>-pvc
        - name: user-home-dir
          persistentVolumeClaim:
            claimName: <name>-user-home-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <name>-user-home-pvc
  namespace: hyperaccel-<team>-ns
  labels:
    team: <team>
    username: <name>
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Gi
  storageClassName: nfs-shared
```

#### yaml template (FPGA 버전)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <your-pod-name>
  namespace: hyperaccel-<team>-ns
  labels:
    app: devcontainer
spec:
  replicas: 1
  selector:
    matchLabels:
      app: devcontainer
  template:
    metadata:
      labels:
        app: devcontainer
        team: <team>
        username: <name>
        devcontainer.io/devcontainer: "true"
    spec:
      securityContext:
        runAsUser: 0
        runAsGroup: 0
        fsGroup: 0
      containers:
      - name: devcontainer
        image: 637423205005.dkr.ecr.us-east-1.amazonaws.com/hyperaccel/devcontainer-aida:cpu-0.1.0        # 해당 위치에는 사용하시고 싶은 이미지 넣으시면 됩니다
        env:
        - name: XILINX_XRT                   # 필수 추가
          value: /opt/xilinx/xrt
        - name: HF_TOKEN
          valueFrom:
            secretKeyRef:
              name: huggingface-secret
              key: token
        - name: HF_HOME
          value: /shared/huggingface
        - name: CUDA_VERSION
          value: "cpu"                         # 사용하시는 버전에 맞게 지정
        - name: PYTHON_VERSION
          value: "3.10"                        # 사용하시는 버전에 맞게 지정
        - name: SHELL_IN_CONTAINER
          value: "bash"                        # 원하시는 shell 지정

        volumeMounts:
        - name: huggingface-shared
          mountPath: /shared/huggingface
        - name: user-home-dir
          mountPath: /root

        stdin: true
        tty: true
        resources:
          requests:
            cpu: "8"                                                       # 변경 금지 (팀별로 논의)
            memory: "32Gi"                                                 # 변경 금지 (팀별로 논의)
            amd.com/xilinx_u55c_gen3x16_xdma_base_3-0: "1"                 # 사용할 FPGA 개수
          limits:
            amd.com/xilinx_u55c_gen3x16_xdma_base_3-0: "1"                 # 사용할 FPGA 개수

      imagePullSecrets:
        - name: <team>-ecr-secret
      volumes:
        - name: huggingface-shared
          persistentVolumeClaim:
            claimName: huggingface-<team>-pvc
        - name: user-home-dir
          persistentVolumeClaim:
            claimName: <name>-user-home-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <name>-user-home-pvc
  namespace: hyperaccel-<team>-ns
  labels:
    team: <team>
    username: <name>
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Gi
  storageClassName: nfs-shared
```

#### yaml template (GPU 버전)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: <your-pod-name>
  namespace: hyperaccel-<team>-ns
  labels:
    app: devcontainer
spec:
  replicas: 1
  selector:
    matchLabels:
      app: devcontainer
  template:
    metadata:
      labels:
        app: devcontainer
        team: <team>
        username: <name>
        devcontainer.io/devcontainer: "true"
    spec:
      runtimeClassName: nvidia                   # 필수 추가
      securityContext:
        runAsUser: 0
        runAsGroup: 0
        fsGroup: 0
      containers:
      - name: devcontainer
        image: 637423205005.dkr.ecr.us-east-1.amazonaws.com/hyperaccel/devcontainer-aida:cu126-0.1.0        # 해당 위치에는 사용하시고 싶은 이미지 넣으시면 됩니다
        env:
        - name: HF_TOKEN
          valueFrom:
            secretKeyRef:
              name: huggingface-secret
              key: token
        - name: HF_HOME
          value: /shared/huggingface
        - name: CUDA_VERSION
          value: "cu126"                       # 사용하시는 버전에 맞게 지정
        - name: PYTHON_VERSION
          value: "3.10"                        # 사용하시는 버전에 맞게 지정
        - name: SHELL_IN_CONTAINER
          value: "bash"                        # 원하시는 shell 지정

        volumeMounts:
        - name: huggingface-shared
          mountPath: /shared/huggingface
        - name: user-home-dir
          mountPath: /root

        stdin: true
        tty: true
        resources:
          requests:
            cpu: "8"                           # 변경 금지 (팀별로 논의)
            memory: "32Gi"                     # 변경 금지 (팀별로 논의)
            nvidia.com/gpu: "1"                # 사용할 GPU 개수
          limits:
            nvidia.com/gpu: "1"                # 사용할 GPU 개수

      imagePullSecrets:
        - name: <team>-ecr-secret
      volumes:
        - name: huggingface-shared
          persistentVolumeClaim:
            claimName: huggingface-<team>-pvc
        - name: user-home-dir
          persistentVolumeClaim:
            claimName: <name>-user-home-pvc
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: <name>-user-home-pvc
  namespace: hyperaccel-<team>-ns
  labels:
    team: <team>
    username: <name>
spec:
  accessModes:
    - ReadWriteMany
  resources:
    requests:
      storage: 100Gi
  storageClassName: nfs-shared
```

#### Pod 실행

```bash
kubectl apply -f pod-devcontainer.yaml
```

> 참고로, Pod 삭제 관련해서는 [해당 문서](https://hyperaccel.atlassian.net/wiki/spaces/Kubernetes/pages/215548599) 참고해주세요

### Pod 생성 여부 확인

```bash
kubectl get pods -n hyperaccel-<team>-ns
```

위 명령어 입력 시에 본인이 생성한 pod의 status가 `RUNNING`이면 성공입니다.

`PENDING` 혹은 `ERROR`이면 `kubectl logs` 명령어와 `kubectl describe` 명령어를 통해 원인 파악 후 해결이 필요합니다.

### Pod 내부 접속

생성한 pod가 `RUNNING` 상태가 되었다면, 이제 pod 내부에서 실행된 container에 접속해서 개발을 진행해야 합니다.

사용자의 실행 환경에 따른 두 가지 방법을 제공합니다.

#### 일반 터미널

```bash
kubectl exec -it <POD_NAME> -n <NAMESPACE> -- /bin/bash
```

#### Cursor

기존에 devcontainer 접속하시던 방법대로 `command + shift + p` 입력 후에 `Dev Container: Attach to Running Kubernetes Container…` 를 통해서 접속하시면 됩니다.

##### Cursor에서 위 방법대로 했지만 container를 인식하지 못하는 경우

Kubernetes Extension 설치 후에 container 접속하시면 됩니다.

## k9s (optional)

k9s를 사용하시면 좀 더 편리하게 쿠버네티스를 사용할 수 있습니다.

이를 활용하면 현재 상태 조회, 로그 확인, 삭제 등 여러 작업을 보다 편리하게 진행하실 수 있습니다.

### 설치 방법

```bash
# 최신 tap 메타데이터 갱신
brew update
# 설치
brew install k9s || brew install derailed/k9s/k9s
# 버전 확인
k9s version
```

### 실행 방법

```bash
k9s -c pod
```

### 참고

아래 링크에서 보다 자세한 사용 방법을 확인하실 수 있습니다:
https://dev-scratch.tistory.com/176

## TroubleShooting

### Pod 생성 오류

Pod를 생성하고 get pods를 통해 봤더니 본인 pod가 `Pending`, `Error`와 같이 정상적으로 실행되지 않는 경우가 발생할 수 있습니다.

아래 명령어를 통해 현재 pod의 상태를 확인할 수 있습니다:

```bash
kubectl describe pod <pod-name> -n hyperaccel-<team>-ns
kubectl logs <pod-name> -n hyperaccel-<team>-ns
```

위를 통해 어떤 상황인지 확인 후에, 스스로 해결하기 어려우시면 관리자에게 문의하시면 됩니다.

### brew install 오류

```
Error: No developer tools installed.
Install the Command Line Tools:
  xcode-select --install
```

#### 해결법

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
export PATH="/opt/homebrew/bin:$PATH"
```