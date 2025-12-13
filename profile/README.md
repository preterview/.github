# 🎤 프리터뷰(Preterview) — AI 기반 모의 면접 플랫폼

> **"면접 준비의 새로운 기준"**  
> 음성 인식과 AI 채점으로 실전 같은 면접 연습을 제공하는 웹 기반 플랫폼

[![TypeScript](https://img.shields.io/badge/TypeScript-007ACC?style=flat&logo=typescript&logoColor=white)](https://www.typescriptlang.org/)
[![Next.js](https://img.shields.io/badge/Next.js-000000?style=flat&logo=next.js&logoColor=white)](https://nextjs.org/)
[![NestJS](https://img.shields.io/badge/NestJS-E0234E?style=flat&logo=nestjs&logoColor=white)](https://nestjs.com/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=flat&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

---

## 📋 목차
- [주요 기능](#-주요-기능-mvp)
- [기술 스택](#-기술-스택)
- [시스템 아키텍처](#-시스템-아키텍처)
- [프로젝트 구조](#-프로젝트-구조)
---

## 🚀 주요 기능 (MVP)

### 1. 질문/답변 등록 및 관리
```typescript
✅ 사용자 정의 질문 생성 (CRUD)
✅ 카테고리별 분류 (CS, 기술면접, 인성면접)
✅ 예상 답변 및 키워드 등록
✅ 드래그 앤 드롭 정렬
```

### 2. 음성 기반 면접 연습
```typescript
✅ TTS: 질문 음성 읽기
✅ STT: 실시간 음성 인식
✅ 녹음 및 재생 기능
✅ 타이머 및 진행 상황 표시
```

### 3. AI 채점 엔진
```typescript
✅ 답변 정확도 분석 (0-100점)
✅ 키워드 매칭 스코어
✅ 의미 유사도 분석 (GPT-4)
✅ 누락 키워드 및 개선 제안
```

### 4. 스크립트 관리
```typescript
✅ 질문/답변 기록 자동 저장
✅ PDF 다운로드 (면접 스크립트)
✅ 세션별 이력 관리
✅ 통계 및 분석 대시보드
```

### 🔥 프리미엄 기능 (향후 제공)
```typescript
🎯 AI 꼬리질문 자동 생성
🎯 답변 연결성 분석
🎯 CS 지식 그래프 매핑
🎯 개인화 학습 추천
```
<!--

---
## 🛠️ 기술 스택

### **Frontend**
```typescript
- Next.js 14+ (App Router)    // SSR/SSG 최적화
- TypeScript                  // 타입 안정성
- Zustand                     // 상태 관리
- TanStack Query              // 서버 상태 관리
- Tailwind CSS + shadcn/ui    // 스타일링
- Web Speech API / Deepgram   // 음성 처리
- next-pwa                    // PWA 지원
```

### **Backend**
```typescript
- NestJS                      // 엔터프라이즈 아키텍처
- TypeScript                  // 풀스택 타입 공유
- TypeORM                     // ORM
- Passport + JWT              // 인증
- Bull Queue                  // 백그라운드 작업
- OpenAI API (GPT-4)          // AI 채점 엔진
- Deepgram API                // STT 처리
```

### **Database & Cache**
```sql
- PostgreSQL 16+              // 메인 데이터베이스
- Redis 7+                    // 캐싱 및 세션
- Pinecone / Qdrant           // 벡터 DB (꼬리질문)
```

### **Infrastructure**
```yaml
- Vercel                      // 프론트엔드 배포
- Railway / Fly.io            // 백엔드 배포
- Supabase                    // PostgreSQL 호스팅
- Upstash                     // Redis 호스팅
- AWS S3 / CloudFlare R2      // 음성 파일 스토리지
- Sentry                      // 에러 추적
- GitHub Actions              // CI/CD
```

### **Development Tools**
```bash
- pnpm                        // 패키지 매니저
- ESLint + Prettier           // 코드 품질
- Husky + lint-staged         // Pre-commit hooks
- Jest + Testing Library      // 테스트
- Playwright                  // E2E 테스트
- k6                          // 부하 테스트
```

---

## 🏗️ 시스템 아키텍처
```
                         ┌──────────────────┐
                         │   CloudFlare CDN  │
                         │   (정적 파일)      │
                         └─────────┬─────────┘
                                   │
                    ┌──────────────┴──────────────┐
                    │                             │
            ┌───────▼────────┐           ┌────────▼───────┐
            │     Vercel     │           │   Railway      │
            │   (Next.js)    │◄─────────►│   (NestJS)     │
            │                │   REST API │                │
            └────────────────┘           └────────┬───────┘
                                                   │
                    ┌──────────────────────────────┼──────────────┐
                    │                              │              │
            ┌───────▼────────┐          ┌─────────▼──┐   ┌───────▼──────┐
            │   Supabase     │          │  Upstash   │   │   AWS S3     │
            │  (PostgreSQL)  │          │  (Redis)   │   │ (음성 파일)   │
            └────────────────┘          └────────────┘   └──────────────┘
                    │
            ┌───────▼────────┐
            │   OpenAI API   │
            │   (GPT-4 채점) │
            └────────────────┘
```


### 데이터 플로우
```
사용자 음성 녹음
    ↓
STT 변환 (Deepgram)
    ↓
텍스트 저장 (PostgreSQL) + 음성 저장 (S3)
    ↓
Bull Queue → 백그라운드 채점 작업
    ↓
GPT-4 API 호출 (정확도 분석)
    ↓
채점 결과 저장 + Redis 캐싱
    ↓
사용자에게 실시간 피드백 제공
```

---

## 📂 프로젝트 구조
```
preterview/
├── frontend/                   # Next.js 프론트엔드
│   ├── src/
│   │   ├── app/               # App Router 페이지
│   │   ├── components/        # React 컴포넌트
│   │   │   ├── ui/           # shadcn/ui
│   │   │   └── features/     # 기능별 컴포넌트
│   │   ├── lib/              # 유틸리티
│   │   ├── hooks/            # Custom Hooks
│   │   ├── stores/           # Zustand 스토어
│   │   └── types/            # TypeScript 타입
│   ├── public/               # 정적 파일
│   └── package.json
│
├── backend/                    # NestJS 백엔드
│   ├── src/
│   │   ├── modules/          # 기능 모듈
│   │   │   ├── auth/
│   │   │   ├── users/
│   │   │   ├── questions/
│   │   │   ├── answers/
│   │   │   ├── scoring/
│   │   │   └── audio/
│   │   ├── common/           # 공통 모듈
│   │   ├── config/           # 설정
│   │   └── database/         # DB 엔티티/마이그레이션
│   └── package.json
│
├── docs/                       # 문서
│   ├── API.md
│   ├── ARCHITECTURE.md
│   └── DEPLOYMENT.md
│
├── scripts/                    # 유틸리티 스크립트
│   ├── seed-data.ts
│   └── backup-db.sh
│
├── .github/                    # GitHub 설정
│   └── workflows/
│       ├── frontend-ci.yml
│       └── backend-ci.yml
│
├── docker-compose.yml          # 로컬 개발 환경
└── README.md                   # 이 파일
```
---

## 📈 성능 목표

### 응답 시간
```
API 평균 응답: < 200ms
STT 변환: < 2초
AI 채점: < 5초
페이지 로딩 (LCP): < 2.5초
```

### 확장성
```
동시 사용자: 10,000명
일일 면접 세션: 50,000+
데이터베이스 쿼리: 1,000 QPS
Redis 캐시 히트율: > 80%
```

### 가용성
```
업타임: 99.9%
에러율: < 0.1%
자동 스케일링: CPU 70% 기준
```

-->
---

## 🔗 링크
- **웹사이트**: 
- **API 문서**:
- **블로그**: 

---
