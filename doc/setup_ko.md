# 설치 가이드

## 시스템 요구 사항

- **운영체제**: Ubuntu 18.04 이상 권장
- **GPU**: Nvidia GPU
- **드라이버 버전**: 525 이상 권장

---

## 1. 가상환경 만들기

학습 또는 배포 프로그램은 가상환경에서 실행하는 것을 권장합니다. 가상환경을 만들 때는 Conda 사용을 권장합니다. 시스템에 Conda가 이미 설치되어 있다면 1.1단계는 건너뛰어도 됩니다.

### 1.1 MiniConda 다운로드 및 설치

MiniConda는 가상환경을 만들고 관리하는 데 적합한 Conda의 경량 배포판입니다. 다음 명령을 사용하여 다운로드하고 설치합니다.

```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm ~/miniconda3/miniconda.sh
```

설치 후 Conda를 초기화합니다.

```bash
~/miniconda3/bin/conda init --all
source ~/.bashrc
```

### 1.2 새 환경 만들기

다음 명령으로 가상환경을 만듭니다.

```bash
conda create -n unitree-rl python=3.8
```

### 1.3 가상환경 활성화

```bash
conda activate unitree-rl
```

---

## 2. 의존성 설치

### 2.1 PyTorch 설치

PyTorch는 모델 학습과 추론에 사용하는 신경망 연산 프레임워크입니다. 다음 명령으로 설치합니다.

```bash
conda install pytorch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 pytorch-cuda=12.1 -c pytorch -c nvidia
```

### 2.2 Isaac Gym 설치

Isaac Gym은 Nvidia에서 제공하는 강체 시뮬레이션 및 학습 프레임워크입니다.

#### 2.2.1 다운로드

Nvidia 공식 웹사이트에서 [Isaac Gym](https://developer.nvidia.com/isaac-gym)을 다운로드합니다.

#### 2.2.2 설치

패키지의 압축을 푼 다음 `isaacgym/python` 폴더로 이동하여 다음 명령으로 설치합니다.

```bash
cd isaacgym/python
pip install -e .
```

#### 2.2.3 설치 확인

다음 명령을 실행합니다. 1,080개의 공이 떨어지는 창이 열리면 설치에 성공한 것입니다.

```bash
cd examples
python 1080_balls_of_solitude.py
```

문제가 발생하면 `isaacgym/docs/index.html`에 있는 공식 문서를 참고하세요.

### 2.3 rsl_rl 설치

`rsl_rl`은 강화학습 알고리즘을 구현한 라이브러리입니다.

#### 2.3.1 다운로드

Git을 사용하여 저장소를 복제합니다.

```bash
git clone https://github.com/leggedrobotics/rsl_rl.git
```

#### 2.3.2 브랜치 전환

v1.0.2 브랜치로 전환합니다.

```bash
cd rsl_rl
git checkout v1.0.2
```

#### 2.3.3 설치

```bash
pip install -e .
```

### 2.4 unitree_rl_gym 설치

#### 2.4.1 다운로드

Git을 사용하여 저장소를 복제합니다.

```bash
git clone https://github.com/unitreerobotics/unitree_rl_gym.git
```

#### 2.4.2 설치

해당 디렉터리로 이동하여 설치합니다.

```bash
cd unitree_rl_gym
pip install -e .
```

### 2.5 unitree_sdk2py 설치(선택 사항)

`unitree_sdk2py`는 실제 로봇과 통신할 때 사용하는 라이브러리입니다. 학습한 모델을 실제 로봇에 배포해야 한다면 이 라이브러리를 설치하세요.

#### 2.5.1 다운로드

Git을 사용하여 저장소를 복제합니다.

```bash
git clone https://github.com/unitreerobotics/unitree_sdk2_python.git
```

#### 2.5.2 설치

해당 디렉터리로 이동하여 설치합니다.

```bash
cd unitree_sdk2_python
pip install -e .
```

---

## 요약

위 단계를 완료하면 가상환경에서 관련 프로그램을 실행할 준비가 끝납니다. 문제가 발생하면 각 구성 요소의 공식 문서를 참고하거나 의존성이 올바르게 설치되었는지 확인하세요.
