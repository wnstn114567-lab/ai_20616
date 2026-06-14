# 졸음 방지 도우미 (Drowsiness Prevention Helper)

Teachable Machine 이미지 모델을 사용해 웹캠으로 졸음을 감지하는 웹 페이지입니다.
공부할 때 졸음을 감지하면 경고해 주고, 깨어있는 동안의 공부 시간을 자동으로 기록합니다.

## 주요 기능
- 🎥 웹캠으로 실시간 상태 인식 (깨어있음 / 졸음 / 자리비움)
- ⏱️ 깨어있는 동안에만 작동하는 공부 시간 누적 스톱워치
- 😴 졸음이 **3초 이상 연속**으로 감지되면 화면 경고 + 음성(TTS) 안내
- 🔁 음성 경고는 10초 쿨다운으로 너무 자주 울리지 않음
- 📋 졸음에 빠진 시각 / 다시 일어난 시각 기록

## 사용 방법
1. `index.html` 을 브라우저(크롬 권장)로 엽니다.
2. 카메라 권한을 허용합니다.
3. 음성 안내 활성화를 위해 페이지를 한 번 클릭합니다.

## 내 모델로 교체하기
`index.html` 상단의 `MODEL_URL` 상수를 본인의 Teachable Machine 모델 주소로 바꾸면 됩니다.
모델의 클래스 라벨은 `wake`, `sleep`, `none` 세 가지를 사용합니다.

```js
const MODEL_URL = "https://teachablemachine.withgoogle.com/models/내_모델주소/";
```

## 기술 스택
- HTML / CSS / JavaScript (단일 파일 `index.html`)
- [TensorFlow.js](https://www.tensorflow.org/js) + [Teachable Machine](https://teachablemachine.withgoogle.com/) 이미지 라이브러리 (CDN)
