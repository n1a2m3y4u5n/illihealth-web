# IlliHealth Web

글을 읽기 어려운 사용자를 위한 의료정보 웹 앱입니다. 처방전을 사진으로 찍으면 픽토그램과 음성으로 보여 줍니다.

- 데모: https://illihealth-web.fly.dev

## 소개

모바일 앱 [illihealth](https://github.com/n1a2m3y4u5n/illihealth)의 웹 버전입니다. SnapCare, e-ICON으로 이어지는 계열이며, 글을 읽기 어려운 사용자가 의료정보를 접할 수 있게 하는 것이 목적입니다. 한국어 처방전과 약 설명서를 브라우저에서 OCR로 읽고, 내용을 픽토그램 DSL(`.p`)로 바꾼 뒤 서버에서 이미지로 합성합니다.

## 주요 기능

- 촬영: 처방전 또는 설명서 사진을 올리면 브라우저에서 OCR(`kor+eng`) 실행
- 변환: OCR 텍스트를 약 이름, 복용 횟수, 시간대 등 구조화된 처방 정보와 픽토그램 DSL로 바꿈
- 합성: `POST /api/pictogram`이 `engine/`의 Python 엔진을 호출해 PNG를 만듦
- 음성 안내: Web Speech API로 읽기(TTS)와 듣기(STT)
- 로그인: 전화번호, 인증번호, 그림 비밀번호
- 보관함, 복약 알림, 건강 기록(컨디션, 사진)
- 응급 화면, 의료진 QR 공유
- 예방: 만성질환, 백신 접종, 감염병 경보
- 한국어, 영어 문구 (`locales/`)

## 기술

- Next.js 16 (App Router), React 19, TypeScript, Tailwind CSS
- Supabase (`@supabase/ssr`, `@supabase/supabase-js`)
- OCR: Tesseract.js
- 픽토그램 엔진: Python + Pillow (`engine/`, [eicon-pictogram-engine](https://github.com/n1a2m3y4u5n/eicon-pictogram-engine)과 같은 엔진)
- 배포: Docker, fly.io (도쿄)

## 실행 방법

```bash
npm install
npm run dev          # http://localhost:3000
```

픽토그램 합성에는 `python3`와 Pillow가 필요합니다(`pip install Pillow`). Supabase 연결에는 환경 변수 `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`가 필요합니다.

```bash
npm run build && npm start
npm run lint
```

## 디렉터리

```
app/          화면과 API 라우트 (pictogram, interaction-check, epidemic-poll)
components/   재사용 UI 컴포넌트
engine/       픽토그램 합성 엔진 (Python)
lib/          OCR, OCR -> .p 변환, 인증, 음성, Supabase
locales/      ko.json, en.json
mockups/      화면 설계용 HTML 목업
```
