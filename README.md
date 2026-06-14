# 졸음 방지 도우미 (Drowsiness Prevention Helper)

웹캠으로 졸음을 감지하는 웹 페이지입니다. **MediaPipe FaceMesh**(눈 정밀 인식)와
**Teachable Machine**(졸린 자세 인식) 두 AI를 **센서 융합(sensor fusion)** 방식으로 결합했습니다.
공부할 때 졸음을 감지하면 경고해 주고, 깨어있는 동안의 공부 시간을 자동으로 기록합니다.

## 두 AI를 합친 방식 (하이브리드)
Teachable Machine 단독으로는 눈 감김 인식이 부정확했기 때문에, 각 AI에게 잘하는 역할을 맡겼습니다.

| 시스템 | 역할 | 설명 |
|---|---|---|
| **MediaPipe FaceMesh** | 얼굴 유무 + 눈 감김 | 눈 468개 점으로 **EAR(눈 종횡비)** 를 정밀 측정 |
| **Teachable Machine** | 졸린 모습/자세 판단 | 고개 숙임 등 **눈이 안 보이는 졸음**을 포착 |

**융합 규칙**
- 얼굴이 없으면 → **자리비움** (FaceMesh가 판정)
- 눈 감김 **+** 티처블도 졸음 → 둘이 동의하므로 **1.5초** 만에 빠르게 경고
- 눈 감김만 / 티처블만 졸음 → 근거가 하나뿐이라 **3초** 동안 신중히 확인 후 경고
- 그 외 → **깨어있음** (이때만 공부 시간 스톱워치 작동)

## 주요 기능
- 🎥 웹캠 실시간 분석 (얼굴 / 눈 / AI 판단을 칩으로 표시)
- ⏱️ 깨어있는 동안에만 작동하는 공부 시간 누적 스톱워치
- 😴 졸음 확정 시 화면 경고(노란색 깜빡임) + 음성(TTS) 안내
- 🔁 음성 경고는 10초 쿨다운으로 너무 자주 울리지 않음
- 📋 졸음에 빠진 시각 / 다시 일어난 시각 + 졸음 근거 기록

## 사용 방법
1. `index.html` 을 브라우저(크롬 권장)로 엽니다.
2. 카메라 권한을 허용합니다.
3. 음성 안내 활성화를 위해 페이지를 한 번 클릭합니다.

## 내 모델로 교체 / 기준값 조정
`index.html` 상단의 `MODEL_URL` 상수를 본인의 Teachable Machine 모델 주소로 바꾸면 됩니다.
모델 클래스 라벨은 `wake`, `sleep`, `none` 세 가지를 사용합니다.

```js
const MODEL_URL = "https://teachablemachine.withgoogle.com/models/내_모델주소/";

const EAR_THRESHOLD   = 0.21;   // 눈 감김으로 보는 기준 (작을수록 더 감아야 인정)
const TM_CONF         = 0.60;   // 티처블이 'sleep'을 인정하는 최소 확률
const HOLD_FAST_MS    = 1500;   // 둘 다 졸음일 때 경고까지 걸리는 시간
const HOLD_NORMAL_MS  = 3000;   // 한쪽 근거만일 때 경고까지 걸리는 시간
```

## 기술 스택
- HTML / CSS / JavaScript (단일 파일 `index.html`)
- [TensorFlow.js](https://www.tensorflow.org/js) + [Teachable Machine](https://teachablemachine.withgoogle.com/) 이미지 라이브러리 (CDN)
- [MediaPipe FaceMesh](https://google.github.io/mediapipe/solutions/face_mesh.html) (CDN)
