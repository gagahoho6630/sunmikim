# sunmikim-pptx-template

엑셀/워드/PDF 파일을 넣으면, **미리 정해둔 디자인 그대로** 파워포인트(.pptx)를 자동으로 만들어주는 Claude Code 플러그인입니다.

> 💡 한 줄 요약: "내 회사 PPT 템플릿을 한 번 등록해두면, 다음부터는 자료만 던져줘도 똑같은 디자인으로 만들어줍니다."

---

## 🎯 이걸로 뭘 할 수 있어요?

**예시 1.** 매주 주간보고를 같은 PPT 양식으로 만든다면
- 엑셀에 이번 주 숫자만 적어서 던져주면 → 항상 같은 디자인의 PPT가 나옵니다.

**예시 2.** 제안서 양식이 정해져 있다면
- 워드에 이번 제안 내용만 적어서 던져주면 → 회사 표준 양식 그대로 PPT가 나옵니다.

**핵심:** 디자인(폰트, 색깔, 위치, 로고)은 **1픽셀도 안 바뀝니다.** 글자 내용만 바뀝니다.

---

## 📋 시작하기 전에 필요한 것

다음 3가지가 컴퓨터에 깔려 있어야 합니다:

1. **Claude Code** ([claude.com/claude-code](https://claude.com/claude-code))
2. **Python 3.9 이상** — 터미널에서 `python3 --version`을 쳐서 확인하세요
3. **Git** — 터미널에서 `git --version`을 쳐서 확인하세요

> 위 3개 중 하나라도 없다면, "맥북 Python 설치", "맥북 Git 설치"로 검색해서 먼저 설치해주세요.

---

## 🚀 설치하기

### 1단계. 이 저장소 받기

터미널을 열고 원하는 폴더로 이동한 뒤:

```bash
git clone https://github.com/sunmikim/sunmikim-pptx-template.git
cd sunmikim-pptx-template
```

### 2단계. 필요한 Python 라이브러리 설치

```bash
pip3 install python-pptx python-docx pypdf openpyxl
```

> 한 번만 하면 됩니다. 이게 PPT/엑셀/워드/PDF를 다루는 도구들이에요.

### 3단계. Claude Code에 플러그인으로 등록

Claude Code를 열고 다음 명령어를 입력하세요:

```
/plugin
```

그리고 **"Install from local path"** 를 선택한 뒤, 방금 `git clone`한 폴더 경로를 입력하세요.
(예: `/Users/내이름/sunmikim-pptx-template`)

설치가 끝나면 Claude Code에서 `/gen-ppt` 명령을 쓸 수 있게 됩니다.

---

## 📖 사용하는 법

### 가장 기본적인 사용

Claude Code 안에서:

```
/gen-ppt 내자료.xlsx
```

이렇게 치면 끝입니다. 잠시 기다리면 `output/` 폴더 안에 PPT 파일이 생겨요.

### 출력 위치를 지정하고 싶다면

```
/gen-ppt 내자료.xlsx 결과물/주간보고.pptx
```

### 워드/PDF도 똑같이

```
/gen-ppt 제안서원본.docx
/gen-ppt 발표내용.pdf
```

---

## 🎨 내 회사 템플릿으로 바꾸기

기본으로 들어있는 `templates/master.pptx`는 **예시용 더미**입니다. 진짜 쓰려면 본인 회사 양식으로 교체해야 합니다.

### 방법

1. 본인이 갖고 있는 PPT 양식 파일을 준비합니다.
2. 그 파일을 `templates/master.pptx`라는 이름으로 **덮어쓰기** 하세요. 끝입니다.

### 주의사항

- 양식 안의 글자들은 **'예시 텍스트'** 로 채워두세요. (예: "사업명: ___" 가 아니라 "사업명: 프로젝트 이름이 들어갈 자리" 처럼)
- 그 예시 텍스트를 보고 AI가 "아, 여기에는 사업명이 들어가는 자리구나" 하고 알아챕니다.
- 표(table)의 칸 수, 슬라이드의 디자인은 **그대로 유지됩니다.** 입력 데이터가 더 많아도 양식은 안 바뀝니다.

---

## 🔧 어떻게 작동하나요? (궁금하신 분만)

이 플러그인은 **AI 에이전트 4명이 팀**으로 일합니다:

| 에이전트 | 하는 일 |
|---|---|
| **ppt-orchestrator** | 팀장. 전체 작업을 조율합니다. |
| **template-curator** | 템플릿 전문가. "이 양식에는 어떤 자리가 있는지" 파악합니다. |
| **data-mapper** | 데이터 분석가. "어떤 정보를 어디에 넣을지" 결정합니다. |
| **slide-builder** | 작업자. 실제로 PPT 파일을 만듭니다. |

여러분이 `/gen-ppt`를 치면 팀장(orchestrator)이 나머지 3명에게 일을 시키고, 결과를 모아서 보고합니다.

---

## 📁 폴더 구조

```
sunmikim-pptx-template/
├── .claude-plugin/
│   └── plugin.json          ← 플러그인 정보 (건드리지 마세요)
├── agents/                  ← AI 에이전트 4명의 지침서
│   ├── ppt-orchestrator.md
│   ├── template-curator.md
│   ├── data-mapper.md
│   └── slide-builder.md
├── commands/
│   └── gen-ppt.md           ← /gen-ppt 명령어 정의
├── scripts/                 ← Python 스크립트들 (실제 작업 도구)
│   ├── inspect_template.py
│   ├── extract_input.py
│   └── build_pptx.py
├── templates/
│   └── master.pptx          ← ★ 여기를 본인 양식으로 교체!
└── output/                  ← 생성된 PPT가 여기에 저장됨
```

---

## ❓ 잘 안 될 때

**"`pip3` 명령어가 없다고 나와요"**
→ 파이썬이 안 깔려있거나 경로 문제입니다. `python3 -m pip install ...` 으로 바꿔서 해보세요.

**"`/gen-ppt` 명령이 안 보여요"**
→ Claude Code에서 `/plugin` 으로 플러그인을 다시 등록해보세요.

**"PPT가 생성됐는데 디자인이 깨져 있어요"**
→ `templates/master.pptx`의 표 크기보다 입력 데이터가 더 많을 가능성이 큽니다. 결과 보고에 "잘렸다(unmapped)"고 나와있을 거예요.

**"한글이 깨져요"**
→ 본인 컴퓨터에 PPT가 사용하는 폰트가 깔려있는지 확인하세요. 폰트는 PPT에 임베드되지 않습니다.

---

## ⚠️ 알고 계셔야 할 한계

1. **새 슬라이드는 못 만들어요.** 양식에 있는 슬라이드 수만큼만 채웁니다. (예: 양식이 2장이면, 결과도 2장입니다.)
2. **표 크기는 안 바뀝니다.** 양식의 표가 5칸짜리면, 데이터가 7개여도 5개만 들어가고 나머지는 잘립니다.
3. **차트(그래프)는 자동 생성 안 됩니다.** 양식에 미리 만들어진 차트가 있으면 데이터만 갈아끼웁니다 (이 기능은 아직 미구현).

---

## 📝 라이선스

MIT
