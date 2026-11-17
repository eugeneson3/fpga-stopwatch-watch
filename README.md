# FPGA 시계·스톱워치 통합

시계와 스톱워치를 통합하고 버튼 디바운스, 표시 선택, 증가·감소 카운터 및 4개 Lap 기록을 구성하였습니다.

- 수행 시기 2026.07–08
- 참여 형태 2인 팀
- 담당 범위 스톱워치, Top module 및 Block diagram 설계

[포트폴리오 상세 페이지](https://eugeneson3.github.io/projects/fpga-stopwatch-watch/)

## 프로젝트 목표

시계와 스톱워치를 동시에 동작시키면서 선택한 모듈에 버튼 입력과 FND 출력을 연결하는 것을 목표로 하였습니다.

## 설계 및 구현

- 본인은 스톱워치 및 Top을, 팀원 박율은 시계·DEMUX·FND 제어를 담당하였습니다.
- 4개 버튼에 대한 디바운스와 단일 tick 출력을 구성하였습니다.
- 시간 단위별 파라미터 카운터와 증가·감소 모드를 사용하였습니다.
- Lap memory에 최대 4개 기록을 저장하고 스위치로 표시 및 삭제 대상을 선택하였습니다.

## Troubleshooting

### Lap 기록 삭제와 쓰기의 중복 실행

스위치 상태에 따른 삭제 처리 뒤에 기록 쓰기가 함께 실행되는 문제가 발생하였습니다.

연속된 독립 if문이 한 번의 입력에서 함께 실행되는 구조였습니다.

조건을 else if로 연결하여 삭제와 쓰기 경로의 실행 관계를 정리하였습니다.

발표 자료에 Lap 4회 저장, 선택 출력, 기록 삭제 파형을 기록하였습니다.

## 검증 및 결과

- 시뮬레이션에서 59초 99단위 이후 카운터 전이와 감소 모드의 Underflow를 확인하였습니다.
- Lap 4개 기록, 스위치 선택 표시 및 삭제 동작을 보드에서 시연하였습니다.

## 소스코드 구조

루트에는 2026.08 수정본을, archive/2026-07에는 초기 통합본을 구성하였습니다.

| 경로 | 내용 |
| --- | --- |
| [src/stopwatch.v](src/stopwatch.v) | Tick generator, 시간 카운터 및 Lap memory |
| [src/control_unit.v](src/control_unit.v) | 스톱워치 FSM |
| [src/btn_debounce.v](src/btn_debounce.v) | 버튼 디바운스 및 Edge 처리 |
| [src/top_watch_stopwatch.v](src/top_watch_stopwatch.v) | 통합 Top |
| [archive/2026-07/](archive/2026-07/) | 7월 초기 통합 소스 |

## 실행 방법

1. 루트 또는 archive/2026-07 중 검토할 버전에서 files.f를 사용합니다.
2. 시뮬레이션에서는 sim/testbench.v를 추가하고 모드 전환·Lap·카운터 경계 파형을 확인합니다.

## 동작 동영상

- [시계·스톱워치 및 Lap 기능 시연](https://www.youtube.com/watch?v=Y4sKckAdjYQ)

## 참고 자료

- [발표 자료](https://docs.google.com/presentation/d/1uPvRbubpczpm4x5vNdt-4z-j7bRWLbvBnR-rEY2rwhM/edit)

## 소스 이력

2026년 11월 18일 로컬 프로젝트에서 구성한 공개 스냅샷입니다. 원본 파일의 상대 경로와 SHA-256은 [SOURCE_MANIFEST.json](SOURCE_MANIFEST.json)에 기록하였습니다. 빌드 산출물과 로컬 개발 환경은 제외하였습니다. 파일별 저작권 및 라이선스 고지를 유지하였습니다.

합성 최상위 모듈은 `top_watch_stopwatch`로 지정합니다. 각 파일에 함께 정의된 `top_watch` 및 `top_stopwatch`는 별도 과제용 모듈입니다. 정적 검사 결과는 docs/에 수록하였습니다.
