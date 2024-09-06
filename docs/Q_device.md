
!!! question "동일 10B모델에 대해 LPU / GPU 서빙 두 환경에서 생성결과가 차이가 많이 나는 부분이 있습니다."
!!! tip ""
    GPU/LPU 하이브리드 서버의 경우에 GPU only로 했을 때와 100퍼센트 일치하지 않을 수 있습니다. 이는 하드웨어마다 사용하는 연산기가 다르고, 연산기에 따라서 float 16 데이터 중에 하위 비트 일부가 달라질 수 있기 때문입니다. 

!!! question "LPU를 사용하기 위해서는 특별히 제작된 모델을 사용해야 하나요?"
!!! tip ""
    저희는 일반적으로 통용되는 huggingface format의 model을 사용합니다. 다만 저희 하드웨어에서 동작시키기 위해서는 hyperdex_sdk/cli/run.py를 이용해 모델을 수정하는 약간의 컴파일 과정이 필요합니다.

!!! question "LPU 상태를 확인할 수 있는 명령어가 있나요?"
!!! tip ""
    자체 개발한 명령어 및 FPGA 명령어 공유 드립니다.
    `hyperdex-smi`를 사용하여 `nvidia-smi`와 같은 방식으로 LPU의 동작 상태를 확인하실 수 있습니다. `hyperdex-reset`을 사용하여 디바이스가 데드락 상태에 빠졌을 때 LPU를 초기화할 수 있습니다. `xbutil examine`을 사용하여 LPU가 잘 장착되어 있는지 확인하실 수 있습니다.

    자세한 내용은 [Trouble-Shooting](./troubleshooting.md) 페이지를 참고 부탁드립니다.

