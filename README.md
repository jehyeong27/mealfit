# MealFit (밀핏)

> 향후 식재료 가격 변동을 예측해 단체급식 시설의 예산에 맞는 메뉴를 제안하는 AI 기반 식단·원가 관리 서비스

- **배포 URL:** https://mealfit.duckdns.org/
- **기간:** 2026.09.10 ~ 2026.10.14 (약 6주, 5명)

## 주요 기능

| 기능 | 설명 |
|---|---|
| 가격 예측 | KAMIS 가격 데이터를 기반으로 식재료의 7일 후 평균·최대 가격과 상승 위험도를 예측 |
| 주간 식단표 | 요일·끼니별 메뉴 배치, 메뉴별 가격 점수(예산 적합도 50% · 가격 상승 25% · 식재료 위험 25%) 표시 |
| 메뉴 추천·교체 | 가격 위험과 예산을 반영한 대체 메뉴 추천 및 교체 |
| 메뉴 검색 | 키워드 검색과 RAG 기반 메뉴 검색, 챗봇 질의 |
| 원가·예산 관리 | 시설별 목표 식비·월 예산 대비 원가 계산과 위험 알림 |
| 회원·권한 | JWT 인증, ADMIN / MANAGER / USER 역할 기반 접근 제어 |
| 관리자 | 시스템 상태 점검, 가격 데이터 현황, 에러 로그 조회 |

## 기술 스택

| 구분 | 기술 |
|---|---|
| Frontend | React 19, Vite, React Router, Axios |
| Backend | Java 17, Spring Boot 4.1.1, Spring Security, JPA, MyBatis, Gradle |
| AI Server | Python 3.12, FastAPI, scikit-learn, XGBoost, LangGraph, MCP, OpenAI API, BGE-M3 |
| Database | PostgreSQL (Supabase), pgvector |
| Infra | Docker, Docker Compose, Nginx, GitHub Actions, AWS (EC2, ECR, SSM) |
| 협업 | Git/GitHub, Jira, Slack |

## 프로젝트 구조

```
mealfit
├── frontend     # React 웹 클라이언트
├── backend      # Spring Boot REST API
└── ai-server    # FastAPI 가격 예측·RAG·LLM 서버
```

## 실행 방법

### 1. 환경변수
각 폴더에 `.env`를 만들어 설정합니다. 비밀값은 저장소에 올리지 않습니다.

| 위치 | 주요 변수 |
|---|---|
| backend | `DB_URL`, `DB_USERNAME`, `DB_PASSWORD`, `JWT_SECRET`, `KAMIS_API_KEY`, `KAMIS_CERT_ID`, `AI_SERVER_URL` |
| ai-server | `DB_*`, `OPENAI_API_KEY` |
| frontend | API 서버 주소 |

### 2. 실행

```bash
# Backend (포트 8080)
cd backend
./gradlew bootRun

# AI Server (포트 8000)
cd ai-server
pip install -r requirements.txt
uvicorn app.main:app --port 8000

# Frontend
cd frontend
npm install
npm run dev
```

### 3. 테스트

```bash
cd backend && ./gradlew test
cd frontend && npm test
```

## 팀원 및 역할

| 이름 | 담당 |
|---|---|
| 정현식 | 회원 인증·권한, 시설 프로필, 시스템 모니터링, 배포·CI/CD |
| 배민선 | KAMIS 데이터 수집, ML 가격 예측·위험도 |
| 박재은 | RAG, LangGraph, MCP, LLM 질의 분석 |
| 이제형 | 주간 식단표 화면, 메뉴 추천·교체, 메뉴 검색, 식단 저장 API |
| 최주원 | 원가 계산, 예산 위험도, 자동 재평가 |
