<div align="center">
  <h1 align="center">Unitree RL GYM</h1>
  <p align="center">
    <span> 🇰🇷 한국어 </span> | <a href="README_zh.md"> 🇨🇳中文 </a>
  </p>
</div>

<p align="center">
  <strong>Unitree 로봇을 기반으로 강화학습을 구현한 저장소로, Unitree Go2, H1, H1_2 및 G1을 지원합니다.</strong>
</p>

<div align="center">

| <div align="center"> Isaac Gym </div> | <div align="center"> Mujoco </div> | <div align="center"> 실제 로봇 </div> |
|--- | --- | --- |
| [<img src="https://oss-global-cdn.unitree.com/static/32f06dc9dfe4452dac300dda45e86b34.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/5bbc5ab1d551407080ca9d58d7bec1c8.mp4) | [<img src="https://oss-global-cdn.unitree.com/static/244cd5c4f823495fbfb67ef08f56aa33.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/5aa48535ffd641e2932c0ba45c8e7854.mp4) | [<img src="https://oss-global-cdn.unitree.com/static/78c61459d3ab41448cfdb31f6a537e8b.GIF" width="240px">](https://oss-global-cdn.unitree.com/static/0818dcf7a6874b92997354d628adcacd.mp4) |

</div>

---

## 📦 설치 및 구성

설치 및 구성 방법은 [setup_ko.md](doc/setup_ko.md)를 참고하세요.

## 🔁 전체 과정

강화학습으로 동작 제어를 구현하는 기본 작업 흐름은 다음과 같습니다.

`Train` → `Play` → `Sim2Sim` → `Sim2Real`

- **Train(학습)**: Gym 시뮬레이션 환경에서 로봇이 환경과 상호작용하도록 하여, 설계된 보상을 최대화하는 정책을 찾습니다. 학습 효율이 저하될 수 있으므로 학습 중 실시간 시각화는 권장하지 않습니다.
- **Play(실행)**: Play 명령으로 학습된 정책을 검증하고 예상대로 동작하는지 확인합니다.
- **Sim2Sim(시뮬레이터 간 전이)**: Gym에서 학습한 정책을 다른 시뮬레이터에 배포하여 해당 정책이 Gym의 특성에 지나치게 종속되지 않았는지 확인합니다.
- **Sim2Real(현실 전이)**: 정책을 실제 로봇에 배포하여 동작을 제어합니다.

## 🛠️ 사용자 가이드

### 1. 학습

다음 명령을 실행하여 학습을 시작합니다.

```bash
python legged_gym/scripts/train.py --task=xxx
```

#### ⚙️ 매개변수 설명

- `--task`: 필수 매개변수이며, 사용 가능한 값은 `go2`, `g1`, `h1`, `h1_2`입니다.
- `--headless`: 기본적으로 그래픽 인터페이스와 함께 시작합니다. 헤드리스 모드(더 높은 효율)로 실행하려면 `true`로 설정하세요.
- `--resume`: 로그에 저장된 체크포인트부터 학습을 재개합니다.
- `--experiment_name`: 실행하거나 불러올 실험의 이름입니다.
- `--run_name`: 실행하거나 불러올 실행(run)의 이름입니다.
- `--load_run`: 불러올 실행의 이름입니다. 기본값은 가장 최근 실행입니다.
- `--checkpoint`: 불러올 체크포인트 번호입니다. 기본값은 가장 최근 파일입니다.
- `--num_envs`: 병렬 학습에 사용할 환경의 개수입니다.
- `--seed`: 난수 시드입니다.
- `--max_iterations`: 최대 학습 반복 횟수입니다.
- `--sim_device`: 시뮬레이션 연산 장치입니다. CPU를 사용하려면 `--sim_device=cpu`로 지정하세요.
- `--rl_device`: 강화학습 연산 장치입니다. CPU를 사용하려면 `--rl_device=cpu`로 지정하세요.

**기본 학습 결과 저장 경로**: `logs/<experiment_name>/<date_time>_<run_name>/model_<iteration>.pt`

---

### 2. 실행(Play)

Gym에서 학습 결과를 시각화하려면 다음 명령을 실행합니다.

```bash
python legged_gym/scripts/play.py --task=xxx
```

**설명**:

- Play의 매개변수는 Train과 동일합니다.
- 기본적으로 실험 폴더에서 가장 최근에 실행한 최신 모델을 불러옵니다.
- `load_run`과 `checkpoint`를 사용하여 다른 모델을 지정할 수 있습니다.

#### 💾 네트워크 내보내기

Play는 Actor 네트워크를 내보내 `logs/{experiment_name}/exported/policies`에 저장합니다.

- 표준 네트워크(MLP)는 `policy_1.pt`로 내보냅니다.
- RNN 네트워크는 `policy_lstm_1.pt`로 내보냅니다.

### Play 결과

| Go2 | G1 | H1 | H1_2 |
|--- | --- | --- | --- |
| [![go2](https://oss-global-cdn.unitree.com/static/ba006789e0af4fe3867255f507032cd7.GIF)](https://oss-global-cdn.unitree.com/static/d2e8da875473457c8d5d69c3de58b24d.mp4) | [![g1](https://oss-global-cdn.unitree.com/static/32f06dc9dfe4452dac300dda45e86b34.GIF)](https://oss-global-cdn.unitree.com/static/5bbc5ab1d551407080ca9d58d7bec1c8.mp4) | [![h1](https://oss-global-cdn.unitree.com/static/fa04e73966934efa9838e9c389f48fa2.GIF)](https://oss-global-cdn.unitree.com/static/522128f4640c4f348296d2761a33bf98.mp4) |[![h1_2](https://oss-global-cdn.unitree.com/static/83ed59ca0dab4a51906aff1f93428650.GIF)](https://oss-global-cdn.unitree.com/static/15fa46984f2343cb83342fd39f5ab7b2.mp4)|

---

### 3. Sim2Sim(Mujoco)

Mujoco 시뮬레이터에서 Sim2Sim을 실행합니다.

```bash
python deploy/deploy_mujoco/deploy_mujoco.py {config_name}
```

#### 매개변수 설명

- `config_name`: 구성 파일입니다. 기본 검색 경로는 `deploy/deploy_mujoco/configs/`입니다.

#### 예시: G1 실행

```bash
python deploy/deploy_mujoco/deploy_mujoco.py g1.yaml
```

#### ➡️ 네트워크 모델 교체

기본 모델은 `deploy/pre_train/{robot}/motion.pt`에 있으며, 직접 학습한 모델은 `logs/g1/exported/policies/policy_lstm_1.pt`에 저장됩니다. 이에 맞게 YAML 구성 파일의 `policy_path`를 수정하세요.

#### 시뮬레이션 결과

| G1 | H1 | H1_2 |
|--- | --- | --- |
| [![mujoco_g1](https://oss-global-cdn.unitree.com/static/244cd5c4f823495fbfb67ef08f56aa33.GIF)](https://oss-global-cdn.unitree.com/static/5aa48535ffd641e2932c0ba45c8e7854.mp4)  |  [![mujoco_h1](https://oss-global-cdn.unitree.com/static/7ab4e8392e794e01b975efa205ef491e.GIF)](https://oss-global-cdn.unitree.com/static/8934052becd84d08bc8c18c95849cf32.mp4)  |  [![mujoco_h1_2](https://oss-global-cdn.unitree.com/static/2905e2fe9b3340159d749d5e0bc95cc4.GIF)](https://oss-global-cdn.unitree.com/static/ee7ee85bd6d249989a905c55c7a9d305.mp4) |


---

### 4. Sim2Real(실제 로봇 배포)

실제 로봇에 배포하기 전에 로봇이 디버그 모드인지 확인하세요. 자세한 단계는 [실제 로봇 배포 가이드](deploy/deploy_real/README.md)에서 확인할 수 있습니다.

```bash
python deploy/deploy_real/deploy_real.py {net_interface} {config_name}
```


#### 매개변수 설명

- `net_interface`: 로봇에 연결된 네트워크 인터페이스의 이름입니다(예: `enp3s0`).
- `config_name`: `deploy/deploy_real/configs/`에 있는 구성 파일입니다(예: `g1.yaml`, `h1.yaml`, `h1_2.yaml`).

#### 배포 결과

| G1 | H1 | H1_2 |
|--- | --- | --- |
| [![real_g1](https://oss-global-cdn.unitree.com/static/78c61459d3ab41448cfdb31f6a537e8b.GIF)](https://oss-global-cdn.unitree.com/static/0818dcf7a6874b92997354d628adcacd.mp4) | [![real_h1](https://oss-global-cdn.unitree.com/static/fa07b2fd2ad64bb08e6b624d39336245.GIF)](https://oss-global-cdn.unitree.com/static/ea0084038d384e3eaa73b961f33e6210.mp4) | [![real_h1_2](https://oss-global-cdn.unitree.com/static/a88915e3523546128a79520aa3e20979.GIF)](https://oss-global-cdn.unitree.com/static/12d041a7906e489fae79d55b091a63dd.mp4) |

---

#### C++로 배포하기

C++로 G1 사전 학습 모델을 배포하는 예제도 제공됩니다. C++ 코드는 다음 디렉터리에 있습니다.

```
deploy/deploy_real/cpp_g1
```

먼저 위 디렉터리로 이동합니다.

```bash
cd deploy/deploy_real/cpp_g1
```

C++ 구현은 LibTorch 라이브러리를 사용합니다. 다음 명령으로 LibTorch를 다운로드하고 압축을 풉니다.

```bash
wget https://download.pytorch.org/libtorch/cpu/libtorch-cxx11-abi-shared-with-deps-2.7.1%2Bcpu.zip
unzip libtorch-cxx11-abi-shared-with-deps-2.7.1+cpu.zip
```

프로젝트를 빌드하려면 다음 명령을 실행합니다.

```bash
mkdir build
cd build
cmake ..
make -j4
```

컴파일이 완료되면 다음 명령으로 프로그램을 실행합니다.

```bash
./g1_deploy_run {net_interface}
```

`{net_interface}`를 실제 네트워크 인터페이스 이름(예: `eth0`, `wlan0`)으로 바꾸세요.

## 🎉 감사의 말

이 저장소는 다음 오픈 소스 프로젝트의 지원과 기여를 바탕으로 만들어졌습니다. 각 프로젝트에 특별히 감사드립니다.

- [legged\_gym](https://github.com/leggedrobotics/legged_gym): 학습 및 실행 코드의 기반을 제공합니다.
- [rsl\_rl](https://github.com/leggedrobotics/rsl_rl.git): 강화학습 알고리즘 구현을 제공합니다.
- [mujoco](https://github.com/google-deepmind/mujoco.git): 강력한 시뮬레이션 기능을 제공합니다.
- [unitree\_sdk2\_python](https://github.com/unitreerobotics/unitree_sdk2_python.git): 실제 로봇 배포를 위한 하드웨어 통신 인터페이스를 제공합니다.

---

## 🔖 라이선스

이 프로젝트에는 [BSD 3-Clause License](./LICENSE)가 적용됩니다.

1. 원본 저작권 고지를 유지해야 합니다.
2. 프로젝트 이름이나 조직 이름을 홍보 목적으로 사용할 수 없습니다.
3. 모든 수정 사항을 공개해야 합니다.

자세한 내용은 전체 [LICENSE 파일](./LICENSE)을 확인하세요.
