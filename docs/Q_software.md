
Q) 지원가능한 언어모델 사이즈가 어떻게 되나요?

A) 8LPU인 orion 서버의 경우 최대 66B까지 가능합니다. 

Q) 저희가 기존에 GPU 기반 서빙에 사용하고있는 text generation inference 와 유사하게 실시간 요청사항을 처리할 수 있는 LPU기반의 서빙 프레임워크로 사용 가능한 툴이 존재할까요?

A) HuggingFace TGI API를 동일하게 사용하고 있으며, 지난번 데모 때 보여드린 챗봇 또한 해당 서빙 스택으로 동작합니다

Q) 한국어도 지원 가능한가요?

A) 모델이 특정 언어를 지원하도록 하는 것은 하드웨어가 아니라 Training 및 fine-tuning의 영역입니다. 저희 LPU 역시 모델이 지원하는 언어와 무관하게 잘 동작합니다. Ko-Alpaca, KULLM3 등 한국어를 잘하는 모델을 다운받아 실제 전시회에서 한국어로 데모한 경험도 있습니다.

Q) 지원 가능한 모델의 종류

A) Foundation model이 llama인 모델은 모두 지원 가능합니다. (ex: llama, llama2, llama3, alpaca, yi …) LPU에서 지원 가능한 모델 정리한 사이트 공유드립니다. [https://hyper-accel.github.io/docs/models/ ] 성능은 모델에 따라 약간의 차이는 있으나 거의 모델의 크기와 비례합니다.

Q)python. transformer library에서 to(’lpu’)처럼 디바이스를 설정하여 사용할 수는 없나요?

A) 저희는 LPU를 사용자가 쉽게 사용할 수 있도록 python binding을 transformer library와 유사하게 만들어 서비스를 제공하고 있습니다. 즉 transformer library 코드에 저희 하드웨어를 붙인 것이 아니라 자체 library를 만든 것이기에 사용법이 완전히 동일하지는 않습니다. “import transformer” 를 사용하면 기존에 사용하시던 transformer library를 사용하는 것이고, “import hyperdex.transformer”를 사용하면 저희가 자체 개발한 HyperDex Library를 사용하는 것입니다. 따라서 model을 부르고 generate를 수행하는 방식은 저희가 제공해드린 예시 코드를 따라주셔야 정상작동합니다. 파이썬 함수가 어떻게 동작하는지 궁금하시다면 /home/server/miniconda3/envs/poc-env/lib/python3.10/site-packages/hyperdex/ 에 들어가셔서 소스코드를 참고해보셔도 좋을 것 같습니다.

Q) hyperdex에서 제공하는 LPU를 사용하려면, 언어모델 초기부터, hyperdex에서 제공하는 SDK library를 이용하여 개발을 해야하나요?

A) 아닙니다. Hyperdex는 언어 모델을 개발하는 라이브러리가 아닌 이미 개발된 모델을 LPU로 실행하기 위한 라이브러리 입니다. 독립적으로 개발하신 모델도 http://huggingface.co  에 올라와있다면 LPU로 실행할 수 있습니다. 실제 다른 회사에서 fine-tuning 한 모델은 물론 foundation model을 개발한 모델까지 수 차례 PoC 진행한 경험이 있습니다.

Q) 기존 코드(예를 들어 transformer을 이용한)를 hyperdex.transformer 로 변경하면, LPU를 사용할 수 있나요?

A) 맞습니다. 이미 이해하신 것 같지만 다시 말씀드리면 백엔드를 완전하게 붙인 것이 아니라 기존 오픈소스를 모방한 것이므로 문법은 저희 예시 코드를 참고해주셔야 더 잘 동작할 수 있습니다.

Q) LPU를 테스트하기 위해서는 hyperdex 로 code를 변환하는 작업이 필요하고, hyperdex를 공부해야 하는 과정도 필요한가요?

A) 저희가 python package로 포장을 다 해두었기에 generate() 함수를 실행시키는 부분만 수정해주시면 동작합니다. 따라서 몇 줄만 간단히 수정하면 다른 코드들과 잘 맞물릴 수 있습니다. 혹시 어려움이 있다면 기술지원을 추가로 드리겠습니다.

Q) hyperdex를 설치하려면 어떻게 해야 하는 지 확인부탁합니다. pip install hyperdex 하니 안되네요.

A) 현재 hyperdex library는 오픈 소스가 아니므로 저희가 licensing 후 배포해드리고 있습니다. 추가적인 설치가 필요하다면 원격 접속을 통해 설치해드릴 수 있습니다.

