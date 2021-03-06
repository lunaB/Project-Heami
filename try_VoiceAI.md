# try VoiceAI
for "Project Heami"

## Voice engine
목소리 엔진을 만들기 위한 시도들이다.

> Research (Tacotron)

목소리 엔진을 만드는데에는 Tacotron을 사용하는 방법과 Deep Voice를 사용하는 방법이 가장 쉽고 확실한 방법이라 생각했다.  
Tacotron은 carpedm20님이 한국어용으로 만들어놓은 모델을 이용하려고 한다.  
(https://github.com/carpedm20/multi-Speaker-tacotron-tensorflow)  

데이터셋을 수집하려고 이것 저것 찾다가 상업적 용도로 사용이 금지되어있지만,
그외 사용이 허용되어있는 한국인 여자 성우의 종합 12시간 이상의 목소리 데이터셋 12,853개를 찾았다.  
(https://www.kaggle.com/bryanpark/korean-single-speaker-speech-dataset)  

학습을 위한 환경 설정  
진짜 환경 설정 하기 너무 힘들었습니다. 그래서 나중에 할때를 위해서 에러 해결 방법을 적으려고 합니다.
- `conda create --name tacotron python=3.6 pip`
- `conda activate tacotron`
- `pip install tensorflow==1.3` (`conda install tensorflow=1.3` 하면 안됨)
- `requriment.txt` 수정
  - `librosa==0.5.1` -> `librosa==0.4.3` (안바꾸면 에러남)
  - `scipy==0.19.1` -> `scipy` (안바꾸면 에러남)
- `pip install -r requirements.txt`
- `alignment.json` 저장할때 `utf-8`로 저장
  - 에러만 보고 `cp949`로 바꾸면 안됨 test에서 에러남
  - `utf-8`로 잘 바뀌지 않는것 같다면 메모장으로 encoding을 `utf-8`로 바꾸어 저장
- `dataset.generate_data.py`의 40번째줄 수정 `with open(config.metadata_path) as f:` -> `with open(config.metadata_path, encoding='utf-8') as f:`
- `['NanumBarunGothic']` 폰트가 워닝이 뜨면 test에서 생성된 plot에 한글이 나오지 않음 utils.plot.py를 수정해서 고칠 수 있슴.
  - 6번줄 `matplotlib.rc('font', family="NanumBarunGothic")` 주석처리
  - 추가
    ```
    import matplotlib.font_manager as fm
    font_path = './utils/NanumBarunGothic.ttf'
    fontprop = fm.FontProperties(fname=font_path)
    ```
  - `fontproperties=fontprop` 추가 
    ```
    plt.xlabel(xlabel, fontproperties=fontprop)
    plt.ylabel(ylabel, fontproperties=fontprop)
    ....
    plt.title(text, fontproperties=fontprop)
    ```
- `python -c "import nltk; nltk.download('punkt')"`
- `python -m datasets.generate_data ./datasets/YOUR_DATASET/alignment.json`
- `conda install -c menpo ffmpeg`
  - test에서 문제가 생긴다면 지웠다 다시 깔아보는 방법이 있슴.
- `hparams.py` 수정
  - 11번째줄 `'cleaners': 'english_cleaners',` -> `'cleaners': 'korean_cleaners',`
- `train.py` 수정
  - 13번째 줄 주석처리 -> `# log(' [*] git recv-parse HEAD:\n%s' % get_git_revision_hash())`
- `tensorboard --logdir=./logs/luna_2019-.../`로 텐서보드 사용 가능 


- tmp
  - python train.py --data_path=datasets/luna
  - python train.py --data_path=datasets/luna --load_path logs/luna_2019-10-10_00-50-02
  - python app.py --load_path logs/luna_2019-10-10_00-50-02 --num_speakers=1
  - python synthesizer.py --load_path logs/luna_2019-10-10_00-50-02 --text "이거  실화냐"
  
> Progress  
- [x] Tacotron 모델 테스트 (하는중)
- [x] 목소리 데이터 셋 수집 
