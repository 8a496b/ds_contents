# DS Contents (GitHub Pages)

Data Science 학습 자료를 GitHub Pages(Jekyll)로 배포하는 사이트입니다.

## 구조

```
ds_contents/
├── _config.yml          # Jekyll 설정
├── _layouts/
│   ├── default.html     # 공통 레이아웃 (KaTeX, Plotly, highlight.js)
│   └── chapter.html     # 챕터 페이지 레이아웃
├── _chapters/           # 챕터 md 파일 (collection)
├── assets/css/style.css
├── index.md             # 목차 페이지
└── Gemfile
```

## 로컬 실행

```bash
bundle install
bundle exec jekyll serve
```

브라우저에서 http://localhost:4000 접속.

## GitHub Pages 배포

1. 이 디렉터리를 GitHub 레포지토리로 푸시
2. 레포지토리 Settings → Pages → Source: **GitHub Actions** 또는 **Deploy from a branch (main / root)** 선택
3. Jekyll이 자동으로 빌드되어 배포됨

## 챕터 추가하기

`_chapters/` 폴더에 아래 형식의 md 파일 추가:

```markdown
---
title: "챕터 제목"
description: "간단한 설명"
order: 5
---

# 본문 시작...
```

- 수식: `$...$`(inline), `$$...$$`(display) — KaTeX 자동 렌더링
- 코드 하이라이팅: fenced code block 자동 지원 (rouge)
- 인터랙티브 플롯: `<div id="..."></div>` + `<script>Plotly.newPlot(...)</script>` 그대로 사용 가능
