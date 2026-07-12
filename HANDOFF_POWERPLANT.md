# Powerplant 사이트 작업 인수인계 (inbeabang-wq/powerplant)

최종 업데이트: 2026-07-11 · 상태: **통합 필기 모의고사(mock_exam) 신규 생성 완료 ✅**

## 이번 세션 작업
1. **mock_exam.html 신규 생성** — 기존에 없던 통합 문제은행 페이지.
   - 전 11개 챕터의 iqcard 977문항을 파싱해 QB JSON으로 통합.
   - Airframe의 mock_exam.html을 템플릿으로 사용(실전 모의고사·챕터별 연습·오답 재시험, 타이머·자동채점·한영 병기).
   - 챕터 드롭다운은 QB의 chko를 읽어 동적 생성 → powerplant 9개 챕터(ch08·ch11은 카드 없음) 자동 반영.
   - localStorage 키를 af_mock → **pp_mock**으로 분리(airframe과 충돌 방지).
   - 제목/브랜딩: "필기 모의고사 · FAA Powerplant Written Exam Simulator".
2. **index.html에 링크 카드 2건 갱신**:
   - 신규 "✏️ 필기 모의고사"(mock_exam.html) 카드 추가 — 977문항.
   - 기존 practical_acs 카드의 구술 수를 13 → 193(대표 13 + Jeppesen 180)으로 정정.

## QB 챕터 분포 (977문항)
ch01=120, ch02=132, ch03=81, ch04=179, ch05=20, ch06=109, ch07=119, ch09=32, ch10=185
(ch08 엔진장탈착·ch11 LSA는 iqcard 0개 — 원본에 카드 없음)

## 검증 결과
- 정답 인덱스 이상 0 · 해설 없음 0 · id 중복 0
- '기체'→'발동기' 과치환 사고 발생 후 재생성으로 해결(원본 QB의 '기체' 정상 보존)

## 기존 완료 자산 (이번 세션 이전부터 존재)
- practical_acs.html: ACS 대표 구술 13 + Jeppesen 실전 기출 180 = 193문항 + 실기 프로젝트 13개 (모두 한/영 병기). 완성도 높음.
- 챕터별 퀴즈(iqcard): 977문항.

## 남은/선택 작업
- ch08(엔진 장탈착)·ch11(LSA) 퀴즈 카드 신규 이식 시, 파싱 스크립트가 자동으로 mock_exam에 포함시킴 → 재생성만 하면 됨.
- 업로드: mock_exam.html, index.html을 GitHub에 반영.
