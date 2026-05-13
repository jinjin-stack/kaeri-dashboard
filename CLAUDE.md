# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

개인 프로젝트 모음 — KAERI(한국원자력연구원) 재직자가 업무·학습 목적으로 만든 standalone HTML 파일들. 빌드 시스템, 패키지 매니저, 서버 없이 브라우저에서 직접 열어 사용한다.

## Files

| 파일 | 설명 |
|------|------|
| `korea_weather.html` | 한국 위성지도 기반 날씨 대시보드 |
| `hf_papers.html` | HuggingFace Daily Papers 카드형 뷰어 |
| `72기신입교육안내.txt` / `kaeri_onboarding.html` | KAERI 신입직원 교육 안내 자료 |
| `연구원의_하루.md` | KAERI 연구원 일상 소개 글 |

## Architecture

모든 HTML 파일은 **단일 파일 아키텍처** — CSS·JS·데이터가 한 파일에 인라인으로 포함된다. 외부 의존성은 CDN으로만 가져온다.

### korea_weather.html
- **지도**: Leaflet.js + ESRI World Imagery 위성 타일 + CartoDB Dark Labels 오버레이
- **날씨 데이터**: [Open-Meteo API](https://open-meteo.com) (무료, API 키 불필요) — 10분마다 자동 갱신
- **레이더**: RainViewer public API (`https://api.rainviewer.com/public/weather-maps.json`)
- **도시 전환**: `CITIES` 배열에 `{name, full, lat, lon, zoom}` 추가하면 자동으로 버튼 생성됨
- **날씨 코드**: WMO weather code → 한국어 라벨·이모지 매핑은 `WMO` 객체에서 관리

### hf_papers.html
- **데이터**: `papers` 배열에 하드코딩 (매일 수동으로 교체 필요)
- **네비게이션**: `current` 인덱스로 카드 단일 표시, 키보드 ←→ 지원
- **논문 구조**: `{rank, title, authors[], org, upvotes, abstract, highlights[], hfUrl, arxivUrl, githubUrl}`

### kaeri_onboarding.html (72기신입교육안내.txt)
- KAERI 브랜드 컬러: `--kaeri-blue: #003087`, `--kaeri-accent: #00a0e0`, `--kaeri-yellow: #f5a623`
- 컴포넌트: `.timeline`, `.card-grid`, `.info-table`, `.contact-grid` — 새 안내 자료 제작 시 재사용

## Conventions

- 한국어 UI, 한국어 주석
- 폰트: `'Noto Sans KR'` (날씨) / `'Malgun Gothic'` (안내문서)
- 다크 테마(날씨·논문)는 CSS 변수(`--bg`, `--accent` 등)로 색상 관리
- 외부 API 호출은 모두 `async/await` + `try/catch`로 처리
- git 커밋 시 `.claude/` 디렉토리는 스테이징하지 않음
