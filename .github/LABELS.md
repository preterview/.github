# 라벨 가이드

## Type (타입)
- `type: bug` - 버그
- `type: feature` - 새로운 기능
- `type: enhancement` - 기능 개선
- `type: documentation` - 문서
- `type: refactor` - 리팩토링
- `type: test` - 테스트
- `type: performance` - 성능 개선

## Area (영역)
- `area: frontend` - Frontend
- `area: backend` - Backend
- `area: ai-engine` - AI Engine
- `area: database` - Database
- `area: infra` - Infrastructure
- `area: api` - API

## Status (상태)
- `status: triage` - 검토 필요
- `status: ready` - 준비 완료
- `status: in-progress` - 진행 중
- `status: blocked` - 블로킹됨
- `status: on-hold` - 보류

## Deployment (배포)
- `deploy: staging` - 스테이징 배포
- `deploy: production` - 프로덕션 배포

## Security (보안)
- `security` - 보안 이슈

## 사용 예시

### 버그 수정
```
labels: type: bug, area: backend
```

### 새로운 기능
```
labels: type: feature, area: frontend
```

### 문서 수정
```
labels: type: documentation
```

### 배포 관련
```
labels: deploy: production, area: infra
```