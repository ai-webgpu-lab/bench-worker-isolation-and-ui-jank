# Results

## 1. 실험 요약
- 저장소: bench-worker-isolation-and-ui-jank
- 커밋 해시: d901399
- 실험 일시: 2026-04-22T06:14:57.601Z -> 2026-04-22T06:14:58.699Z
- 담당자: ai-webgpu-lab
- 실험 유형: `benchmark`
- 상태: `success`

## 2. 질문
- 같은 burn profile에서 main thread와 worker execution의 responsiveness 차이가 측정되는가
- frame pacing, timer lag, input lag 관련 메모를 결과 문서에 연결할 수 있는가
- 실제 heavy runtime을 붙이기 전 UI jank benchmark baseline으로 쓸 수 있는가

## 3. 실행 환경
### 브라우저
- 이름: Chrome
- 버전: 147.0.7727.15

### 운영체제
- OS: Linux
- 버전: unknown

### 디바이스
- 장치명: Linux x86_64
- device class: `desktop-high`
- CPU: 16 threads
- 메모리: 16 GB
- 전원 상태: `unknown`

### GPU / 실행 모드
- adapter: not-applicable
- backend: `mixed`
- fallback triggered: `false`
- worker mode: `main, worker`
- cache state: `unknown`
- required features: []
- limits snapshot: {}

## 4. 워크로드 정의
- 시나리오 이름: Main Thread Burn, Worker Burn
- 입력 프로필: 40x14ms-burn
- 데이터 크기: scenario=main; timerLagP95=22.8; inputLagP95=0.2; workerRttP95=0; rounds=40; burnMs=14; automation=playwright-chromium, scenario=worker; timerLagP95=1.3; inputLagP95=0.2; workerRttP95=16.6; rounds=40; burnMs=14; automation=playwright-chromium
- dataset: -
- model_id 또는 renderer: -
- 양자화/정밀도: -
- resolution: -
- context_tokens: -
- output_tokens: -

## 5. 측정 지표
### 공통
- time_to_interactive_ms: 865.8 ~ 1963.9 ms
- init_ms: 596.6 ~ 728.4 ms
- success_rate: 1
- peak_memory_note: 16 GB reported by browser
- error_type: -

### Graphics / Blackhole
- avg_fps: 59.99 ~ 60
- p95_frametime_ms: 16.7 ~ 16.8 ms
- scene_load_ms: 596.6 ~ 728.4 ms
- worker modes: main, worker

## 6. 결과 표
| Run | Scenario | Backend | Cache | Mean | P95 | Notes |
|---|---|---:|---:|---:|---:|---|
| 1 | Main Thread Burn | mixed | unknown | 59.99 | 16.8 | scene_load=728.4 ms, worker_mode=main |
| 2 | Worker Burn | mixed | unknown | 60 | 16.7 | scene_load=596.6 ms, worker_mode=worker |

## 7. 관찰
- worker run avg_fps=60, main run avg_fps=59.99였다.
- p95 frametime은 main=16.8 ms, worker=16.7 ms로 비교됐다.
- playwright-chromium로 수집된 automation baseline이며 headless=true, browser=Chromium 147.0.7727.15.
- 실제 runtime/model/renderer 교체 전 deterministic harness 결과이므로, 절대 성능보다 보고 경로와 재현성 확인에 우선 의미가 있다.

## 8. 결론
- main vs worker jank benchmark baseline이 실제 raw 결과와 summary 문서로 연결됐다.
- 다음 단계는 실제 inference/compute workload를 같은 harness에 붙여 responsiveness 차이를 재측정하는 것이다.
- 입력 지연과 frame pacing을 브라우저/디바이스별로 누적해야 계획 기준에 맞는다.

## 9. 첨부
- 스크린샷: ./reports/screenshots/01-main-thread.png, ./reports/screenshots/02-worker-thread.png
- 로그 파일: ./reports/logs/01-main-thread.log, ./reports/logs/02-worker-thread.log
- raw json: ./reports/raw/01-main-thread.json, ./reports/raw/02-worker-thread.json
- 배포 URL: https://ai-webgpu-lab.github.io/bench-worker-isolation-and-ui-jank/
- 관련 이슈/PR: -
