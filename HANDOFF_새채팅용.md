# FAA A&P 학습 사이트 프로젝트 — 인수인계

> 새 채팅 시작 시 이 문서를 업로드하고 "이어서 작업해줘"라고 말씀하시면 됩니다.

---

## 1. 프로젝트 개요

방인배(inbae)의 FAA A&P 자격 준비용 **이중언어(한/영) 학습 사이트 2개**를 운영·개선 중입니다.

| 사이트 | GitHub | URL |
|---|---|---|
| Airframe | `inbeabang-wq/airframe` | inbeabang-wq.github.io/airframe |
| Powerplant | `inbeabang-wq/powerplant` | inbeabang-wq.github.io/powerplant |

**작업 환경**: Mac (macOS), 모든 파일은 사용자가 직접 GitHub에 업로드
**응답 규칙**: 한국어 존댓말, 영어 용어 병기, 단계별 정리, Mac 기준 안내

---

## 2. 현재 상태 (2026-07-15 기준) — 두 사이트 모두 완성 단계

### Airframe (18개 챕터)

| 항목 | 상태 |
|---|---|
| 챕터 | ch01~ch18 (ch18 검사는 신규 제작) |
| FAA 도면 갤러리 | **1,193개** (v3 추출, 900px, 잘림 해결 완료) |
| 구술시험 | **406문항** (18챕터 전부, 한/영+채점+진행률) |
| ASA 교재 보강 | 8챕터 (ch02·10·11·13·14·15·16·17) + ch18 신규 |
| 반응형 CSS | 전 챕터 (1600~390px 검증 완료) |
| 영어 병기 | 전 페이지 완비 (systems.html 포함) |
| 부가 페이지 | index / systems / textbook / practical_acs / mock_exam 모두 정상 |

### Powerplant (11개 챕터)

| 항목 | 상태 |
|---|---|
| 챕터 | ch01~ch11 |
| FAA 도면 갤러리 | **584개** (기존 601개 - 검은 얼룩 17개 제거) |
| 구술시험 | **295문항** (11챕터 전부) |
| ASA 교재 보강 | 10챕터 (ch01~ch10, ch11 LSA는 대응 교재 없음) |
| 반응형 CSS | 전 챕터 |
| 영어 병기 | 완비 (p-ko/p-en 구조) |

**두 사이트 마무리 점검 완료 — 문제 0개**

---

## 3. 미업로드 파일 (사용자 작업 대기)

### powerplant — 구술 3개 챕터 (방금 작업)
- `ch05_starting_systems.html` (시동 12문항)
- `ch08_engine_removal.html` (탈장착 12문항)
- `ch11_lsa_engines.html` (LSA 12문항)

업로드 방법:
```bash
cd ~/Downloads/powerplant
git pull
cp ~/Downloads/ch05_starting_systems.html ~/Downloads/ch08_engine_removal.html ~/Downloads/ch11_lsa_engines.html .
git add .
git commit -m "ch05/ch08/ch11 구술시험 36문항 추가"
git push
```

---

## 4. 남은 작업 (자료 필요)

### 스캔본 ASA 교재 4개 — 보강 불가
텍스트 추출이 안 되는 스캔 이미지라 페이지 캡처가 있어야 진행 가능:
- 02 METALLIC → airframe ch01, ch04
- 03 NONMETALLIC → airframe ch03, ch06, ch07
- 05 HYDRAULIC → airframe ch12
- 07 ELECTRICAL → airframe ch09

### 잘린 FAA 도면 7건 (airframe, 플레이스홀더 상태)
- ch02: Figure 2-7(4힘), 2-9(윙팁보텍스)
- ch13: Figure 13-131(타이어), 13-55(시미), 13-28(휠얼라인)
→ 원본 도면 캡처가 있으면 교체 가능

---

## 5. 핵심 자료 위치

### 구글 드라이브
| 자료 | 파일 ID / 폴더 ID |
|---|---|
| ASA Powerplant 교재 (20개 챕터 PDF) | 폴더 `1iGgiwMsr1rOamamsaAJcv3MAaS97uXts` |
| ASA 교재 부모 폴더 | `1J6eAL5LLJ0wi4zJ6dd3y-zMHzpda_RwU` |
| FAA-H-8083-31B (Airframe 핸드북, 112MB) | `1VxeOKXnbkn7Ti4hOuBniL1xiM6INbIp2` |
| amt_powerplant_handbook.pdf (FAA-H-8083-32B, 214MB) | `1SNPUvSkpBoptsPBeW8IgNzjBSpYq71lZ` |

> ⚠️ 10MB 초과 파일은 Claude가 직접 다운로드 불가 → 사용자 Mac에서 스크립트 실행 필요

### 사용자 Mac
- 작업 폴더: `~/Downloads/faa_work/` (PDF + 추출 스크립트)
- ⚠️ Downloads에 자동 정리 스크립트가 있어 파일이 옮겨질 수 있음

---

## 6. 도면 추출 스크립트 (v3) — 검증 완료

`extract_airframe_figures.py` (사용자 Mac의 `~/Downloads/faa_work/`)

**핵심 원리**: PDF 이미지 객체를 꺼내지 않고, `get_drawings()` + `get_image_rects()`로
그림의 진짜 경계를 계산해 페이지를 렌더링·크롭 → 마스크 문제·잘림 없음

```bash
cd ~/Downloads/faa_work
python3 extract_airframe_figures.py [PDF파일명] --dry-run   # 미리보기
python3 extract_airframe_figures.py [PDF파일명]              # 실제 추출
```

- 출력: `images/chXX_figX-Y.jpg` (900px) + `figure_captions.json`
- 필요 라이브러리: `pip3 install pymupdf pillow numpy`
- 옵션: `--pad 14` (여백↑), `--fallback page` (경계 못 찾으면 페이지 전체)

### 버전 이력 (같은 실수 반복 방지)
- **v1**: 이미지 객체 추출 → 검은 얼룩 442개(27%), 본문 텍스트 다수 ❌
- **v2**: 페이지 렌더링 + 캡션 폭 크롭 → 얼룩 해결했으나 **좌우 잘림** ❌
- **v3**: 그림 경계 자동 계산 → 잘림·얼룩 모두 해결 ✅ (현재 버전)

### powerplant 재추출은 보류함
FAA-H-8083-32B(500쪽)로 v3 추출 시 430개만 나옴 (기존 601개보다 171개 적음).
기존 도면 품질이 양호(잘림 없음)해서 유지하고 얼룩 17개만 제거함.

---

## 7. HTML 구조 규칙 (수정 시 필수 참고)

### 공통
- 섹션: `<section id="s-xxx" class="comp">` + `.comp-head`(h3 + `.comp-en`) + `.comp-body` > `.comp-text`
- 문단 영어 병기: `<p>한국어<span class="en-text">English</span></p>`
- TOC: `<li><a href="#s-xxx" data-target="s-xxx">한국어 <span class="en">English</span></a></li>`
- 챕터 순서: 본문 섹션들 → `s-oral`(구술) → `s-faa-figures`(도면) → `s-quiz`(기출, airframe만)

### ASA 보강 배지
```html
<span class="src-badge">📗 ASA Powerplant 보강</span>
```
CSS가 없으면 추가:
```css
.src-badge{display:inline-block;margin-left:8px;font-family:'JetBrains Mono',monospace;
font-size:10.5px;color:#34D399;background:rgba(52,211,153,.12);
border:1px solid rgba(52,211,153,.3);border-radius:5px;padding:2px 7px;vertical-align:middle;}
```

### 구술시험 섹션 (oralx)
- 카드: `.oralx-card` > `.oralx-q`(`.qk` 한국어 + `.qe` 영어) + `.oralx-actions` + `.oralx-ans`
- 버튼: `orxToggle(this)` / `orxGrade(this,'orx_N','know'|'again')`
- 진행률: `#orxDone`, `#orxFill`, `#orxPct`
- CSS 32규칙 + JS 1145자 (기존 챕터에서 추출해 이식)
- ⚠️ **CSS 추출 시 정규식 주의**: `<style>` 블록 내부에서만 `.oralx[^{}]*\{[^{}]*\}` 추출.
  안 그러면 JS 코드가 CSS에 섞여 들어가 인터랙션이 죽음 (실제 발생했던 버그)

### 도면 갤러리
```html
<figure class="fig figgal">
  <img src="images/ch13_fig13-1.jpg" alt="fig13-1" loading="lazy">
  <figcaption>Figure 13-1. 제목. <span class="figpdf">(FAA-H-8083-31B)</span></figcaption>
</figure>
```
- 컨테이너: `<div class="figgrid">`
- 출처 표기: airframe = `FAA-H-8083-31B`, powerplant = `FAA-H-8083-32B`

---

## 8. 작업 시 주의사항 (경험칙)

1. **GitHub Pages·FAA.gov는 봇 차단(403)** → 라이브 URL로 검증 불가. 반드시 로컬 클론 후 Playwright 렌더링으로 검증할 것.

2. **이미지 품질 자동 판정은 신뢰하지 말 것.** 통계(검정 비율 등)가 "어두운 사진"이라 해도 실제로 열어보면 완전 불량인 경우가 많았음. 반드시 `view` 도구로 눈으로 확인.

3. **기존 챕터는 이미 매우 충실함**(16~34섹션). ASA 보강 시 중복을 피해 교재 고유 내용만 1~2섹션으로 압축.

4. **사용자에게 명령어 안내 시 주석(`#`) 섞지 말 것.** zsh에서 `command not found: #` 에러 발생. 한 줄씩 코드블록으로 분리.

5. **파일 전달 후 버전 확인 요청.** 사용자가 이전 버전을 계속 쓰는 일이 있었음 (`head -12 파일명`으로 확인).

---

## 9. 검증 코드 스니펫

```python
# 로컬 클론 후 Playwright 검증 (표준 패턴)
from playwright.sync_api import sync_playwright
import os, glob
with sync_playwright() as p:
    b = p.chromium.launch()
    pg = b.new_page(viewport={'width':1280,'height':900})
    for fn in sorted(glob.glob('ch*.html')):
        pg.goto('file://'+os.path.abspath(fn)); pg.wait_for_timeout(400)
        r = pg.evaluate('''() => ({
          secs: document.querySelectorAll('section.comp').length,
          gal: document.querySelectorAll('#s-faa-figures figure.figgal').length,
          oral: document.querySelectorAll('.oralx-card').length,
          js: typeof window.orxToggle === 'function'
        })''')
        print(fn, r)
    b.close()
```

---

## 10. 다음에 할 수 있는 것

1. **powerplant 구술 3개 챕터 업로드** (파일 준비 완료, 사용자 작업만 남음)
2. **스캔본 교재 보강** — 사용자가 페이지 캡처 제공 시
3. **잘린 도면 7건 교체** — 원본 캡처 제공 시
4. **학습 도구 추가** — 통합 검색, 북마크, 오답노트 등
5. **@AEROMECHEXPERT 콘텐츠** — 수요일 K-Defense Weekly + MRO 캐러셀

---

## 11. 새 채팅 시작 시 주의

**이 세션의 outputs 폴더는 새 채팅에 넘어가지 않습니다.**
필요한 파일은 GitHub에 이미 반영돼 있으므로, 새 채팅에서는 아래처럼 최신본을 받아 작업하면 됩니다:

```bash
cd /home/claude
git clone --depth 1 https://github.com/inbeabang-wq/airframe.git af
git clone --depth 1 https://github.com/inbeabang-wq/powerplant.git pp
```

⚠️ 단, **powerplant의 ch05/ch08/ch11 구술 3개 파일은 아직 미업로드 상태**일 수 있음.
새 채팅에서 먼저 확인할 것:

```bash
grep -c 'oralx-card' pp/ch05_starting_systems.html
```
- `0`이면 → 미업로드 (사용자에게 업로드 요청, 또는 재작업 필요)
- `12`면 → 업로드 완료

### 사용자 Mac에 남아 있는 것
- `~/Downloads/faa_work/extract_airframe_figures.py` (v3 스크립트)
- `~/Downloads/faa_work/amt_powerplant_handbook.pdf`
- `~/Downloads/airframe/`, `~/Downloads/powerplant/` (git 클론)
