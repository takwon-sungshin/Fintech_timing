# 자유전공 학생 강연용 핀테크 실습 도구

성신여자대학교 자유전공 1학년 학생 대상 1시간 강연(15-30분 실습)을 위한 단일 HTML 도구 모음입니다.
**수리통계데이터사이언스학부 핀테크 트랙**의 차별화 포인트("핀테크의 두뇌")를 보여주기 위해 만들었습니다.

## 파일 구성

| 파일 | 용도 |
| ---- | ---- |
| `index.html` | 강의 안내 + 두 도구 링크 + QR 코드 (학생용 모바일 진입점) |
| `optimal-stopping-game.html` | 매도 타이밍 게임 — 37% 규칙 봇 vs 학생 |
| `price-monte-carlo.html` | 주가 시뮬레이터 — GBM/Merton Jump, 2종목 비교 |
| `README.md` | 본 문서 |
| `.nojekyll` | GitHub Pages가 Jekyll로 처리하지 않도록 (의도된 빈 파일) |

세 HTML은 모두 단일 파일이며 외부 의존성은 (1) Google Fonts, (2) GoatCounter 트래픽 카운터, (3) `index.html`의 QR 라이브러리(jsdelivr CDN), 이 셋입니다.
오프라인에서도 폰트만 fallback으로 처리되고 동작합니다.

## 강의 1시간 구성

| 시간 | 내용 |
| ---- | ---- |
| 0–8분 | 핀테크 = 돈에 관한 의사결정 자동화. "암호=금고, 수리금융=두뇌" 프레임 |
| 8–22분 | 실습 1 (매도 타이밍 게임) 5판 플레이, 봇 vs 학생 비교 |
| 22–37분 | 실습 2 (주가 시뮬레이터) — 삼성 → NVIDIA → 카카오 순으로 종목 바꿔가며 시연 |
| 37–50분 | "왜 수학이 이겼나" 직관 설명. `dS = μS dt + σS dW` 보여주고 안 풀기 |
| 50–60분 | 핀테크 트랙 커리큘럼, RL 멘토링 사례, 졸업 진로(퀀트/보험계리/데이터분석) |

## 도구별 요약

### 1. 매도 타이밍 게임 (`optimal-stopping-game.html`)

- 30개 가격이 1초마다 흐르고 학생은 한 번만 매도
- GBM 기반 합성 가격 (σ=0.025), price = 100 시작
- 봇: **37% 규칙** — `floor(N/e)=11`개 관찰 후, 그것을 깨는 첫 가격에 매도
- 게임 종료 시 차트에 YOU(주황) / MAX(초록) / BOT(파랑) 마커 동시 표시
- 점수 = (매도가 / 최고가) × 100. 봇 평균 91점, 학생 평균 70-85점
- 누적 통계 + 막대그래프, 속도 조절(느림/보통/빠름), 스페이스바 단축키
- **거래세 토글** — 없음 / 0.25% / 0.50% 중 선택, 세후 가격 기준으로 점수 환산
- **결과 PNG 저장 / 클립보드 복사** 버튼
- 우측 패널에 37% 규칙 설명 + Dynkin/Lindley 언급 + optimal stopping 응용 (미국식 옵션 등)

### 2. 주가 시뮬레이터 (`price-monte-carlo.html`)

- 종목 6개: 삼성전자, SK하이닉스, NAVER, 카카오, NVIDIA, Apple
- 각 종목 (시작가, 연수익률 μ_y, 연변동성 σ_y)로 GBM 시뮬레이션 → 표본 시계열 252일 생성 (시드 고정)
- 로그수익률에서 μ, σ 자동 calibration → 일별/연환산 표시
- **모델 토글**: GBM ↔ Merton Jump-Diffusion (점프 빈도 λ, 크기 σ_J, 편향 μ_J 슬라이더)
- **비교 모드 토글**: 1종목 단독 ↔ 2종목 비교 (시작가 100 기준 정규화 + 분포 overlap + Sharpe ratio 간이 비교)
- **파라미터 소스 토글**: 실데이터 calibration ↔ 수동 조정 (μ_y, σ_y 슬라이더)
- 슬라이더: 예측 기간 10-252일, 경로 수 100/500/1000
- 1000개 경로 spaghetti(투명 누적) + 5-95% percentile band + median 라인
- 최종가 히스토그램 (32 bins, 비교모드에선 % 수익률 기준 overlay)
- 통계: 중앙값 최종가, P(이익), 5%/95% 분위, Sharpe (간이, 비교모드)
- **결과 PNG 저장 / 클립보드 복사** 버튼
- "예시 시계열 (실제 변동성 반영)" 명시

## 디자인 시스템

- 배경: GitHub-dark `#0d1117`
- 폰트: IBM Plex Sans / Mono + Noto Sans KR (Google Fonts)
- panel 기반 (10px radius, 3px accent bar on titles)
- 색상: accent `#58a6ff`, accent2 `#3fb950`, accent3 `#f0883e`, accent4 `#d2a8ff` (비교모드 B), danger `#ff6b6b`
- GoatCounter `https://takwon-sungshin.goatcounter.com/count` 삽입 완료
- 푸터: "성신여자대학교 수리통계데이터사이언스학부 · 핀테크 트랙 데모"
- DPR-aware canvas (`fixCanvas` 함수)

## GitHub Pages 배포

GitHub Pages는 정적 파일을 그대로 호스팅합니다. 두 가지 방식 중 편한 쪽을 고르세요.

### 방식 A — `main` 브랜치 루트에서 호스팅 (권장)

1. 새 GitHub repo 생성 (예: `fintech-lecture-tools`)
2. 이 폴더의 파일을 모두 commit & push
3. repo 설정 → Settings → Pages → **Source: Deploy from a branch** → **Branch: main / (root)** 선택
4. 1-2분 후 `https://<username>.github.io/<repo>/` 로 접속 가능
5. `index.html`을 열고 상단 입력란에 위 URL을 붙여넣으면 QR이 갱신됨

### 방식 B — `docs/` 하위 폴더에서 호스팅

본 폴더를 `docs/`로 옮긴 뒤, Settings → Pages → **Branch: main / docs** 선택.

### `.nojekyll`

GitHub Pages는 기본적으로 Jekyll로 처리되어 `_`로 시작하는 파일을 무시합니다.
지금은 그런 파일이 없지만, 앞으로 추가될 때 안전하도록 빈 `.nojekyll` 파일을 동봉합니다.

## 로컬에서 보기

이중클릭으로 `index.html`을 열면 바로 동작합니다. 폰트와 GoatCounter는 인터넷 연결이 있을 때만 로드되고, 없어도 도구 자체는 정상 작동합니다.

브라우저 보안 정책상 `file://` 스킴에서 일부 기능(클립보드 이미지 복사)이 제한될 수 있습니다.
간이 로컬 서버로 띄우면 모든 기능이 동작합니다:

```bash
# Python 3
python3 -m http.server 8000
# 그리고 http://localhost:8000/ 접속
```

## 라이선스 / 크레딧

강의용 데모 — 자유롭게 수정/배포하세요.
강의 진행: 성신여자대학교 수리통계데이터사이언스학부.
