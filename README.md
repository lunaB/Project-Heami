# Project-Heami
구로디지털단지 인젠트에서 근무하는 신씨 집안의 모재규 군을 위한 SNS 가상 여자친구 봇

## Subject
- SNS를 이용할 줄 아는 봇
- 사용자와 메신저를 이용하여 대화가 가능한 봇
- 사용자와 전화 혹은 대화를 할 수 있는 목소리를 가진 봇
- 대화 흐름 목표
  - 성격이 있어야 한다
  - 기분이 있어야 한다
  - 감정이 있어야 한다

## Research List
- 설정 만들기
- 목소리 만들기
- 셀카 만들기
- 체팅 봇 만들기
- SNS 알고리즘 만들기

## Voice engine
목소리 엔진을 만들어보려고 한다. 
> Research 

목소리 엔진을 만드는데에는 Tacotron을 사용하는 방법과 Deep Voice를 사용하는 방법이 가장 쉽고 확실한 방법이라 생각했다.  
Tacotron은 carpedm20님이 한국어용으로 만들어놓은 모델을 이용하려고 한다.  
(https://github.com/carpedm20/multi-Speaker-tacotron-tensorflow)  

데이터셋을 수집하려고 이것 저것 찾다가 상업적 용도로 사용이 금지되어있지만,
그외 사용이 허용되어있는 한국인 여자 성우의 종합 12시간 이상의 목소리 데이터셋 12,853개를 찾았다.  
(https://www.kaggle.com/bryanpark/korean-single-speaker-speech-dataset)  

학습을 위한 환경 설정  
진짜 환경 설정 하기 너무 힘들었습니다. 그래서 나중에 할때를 위해서 에러 해결 방법을 적으려고 합니다.
- conda create --name tacotron python=3.6 pip
- conda activate tacotron
- pip install tensorflow==1.3 (conda install tensorflow=1.3 하면 안됨)
- requriment.txt 수정
  - librosa==0.5.1 -> librosa==0.4.3
  - scipy==0.19.1 -> scipy
- pip install -r requirements.txt
- alignment.json 저장할때 utf-8 대신 cp949로 저장
- python -c "import nltk; nltk.download('punkt')"

- python -m datasets.generate_data ./datasets/YOUR_DATASET/alignment.json
- conda install -c menpo ffmpeg


> Progress  
- [ ] Tacotron 모델 테스트 (하는중)
- [x] 목소리 데이터 셋 수집 
