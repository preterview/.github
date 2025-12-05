# 기여 가이드

## 목차
- [Git 워크플로우](#git-워크플로우)
- [커밋 메시지 규칙](#커밋-메시지-규칙)
- [Pull Request](#pull-request)
- [코드 스타일](#코드-스타일)
- [셀프 코드 리뷰](#셀프-코드-리뷰)

---

## Git 워크플로우

### 브랜치 전략

**주요 브랜치**
- `main` - 프로덕션 (배포 가능한 상태)
- `develop` - 개발 (다음 릴리스 준비)

**작업 브랜치 네이밍**
```
feat/기능명        새로운 기능
fix/버그명         버그 수정
refactor/내용      리팩토링
docs/내용          문서 수정
test/내용          테스트 추가/수정
chore/내용         설정, 빌드 등
style/내용         코드 포맷팅
perf/내용          성능 개선
```

**예시**
```
feat/question-filter
fix/keyword-save-bug
refactor/api-error-handler
docs/update-readme
chore/update-dependencies
```

### 작업 흐름
```bash
# 1. 최신 코드 받기
git checkout develop
git pull origin develop

# 2. 작업 브랜치 생성
git checkout -b feat/your-feature

# 3. 작업 후 커밋
git add .
git commit -m "feat: 기능 설명"

# 4. 원격에 푸시
git push origin feat/your-feature

# 5. GitHub에서 PR 생성 (develop ← feat/your-feature)
```

---

## 커밋 메시지 규칙

### 형식
```
<type>: <subject>

<body> (선택)
```

### Type
- `feat`: 새로운 기능
- `fix`: 버그 수정
- `refactor`: 리팩토링
- `docs`: 문서 수정
- `style`: 코드 포맷팅
- `test`: 테스트 추가/수정
- `chore`: 빌드, 설정 파일 수정
- `perf`: 성능 개선

### 규칙
- subject는 50자 이내
- 명령문 사용 ("추가했다" X / "추가" O)
- 마침표 사용 안 함
- 한글 또는 영어 (일관성 유지)

### 예시
```
feat: 질문 카테고리 필터 추가
fix: 키워드 배열 저장 오류 수정
refactor: API 에러 핸들러 개선
docs: README 설치 가이드 추가
test: 질문 생성 E2E 테스트 추가
chore: ESLint 설정 업데이트
style: Prettier 포맷팅 적용
perf: 질문 목록 쿼리 최적화
```

---

## Pull Request

### PR 생성 전 체크리스트
```bash
pnpm lint        # 린트 검사
pnpm type-check  # 타입 체크
pnpm test        # 테스트 실행
pnpm build       # 빌드 확인
```

### PR 제목
커밋 메시지와 동일한 형식
```
feat: 질문 카테고리 필터 기능
fix: 키워드 저장 오류 수정
```

### 셀프 리뷰
- 불필요한 console.log 제거
- 주석 처리된 코드 제거
- TODO 주석 확인
- 민감한 정보 없음 (API 키 등)
- 파일명/변수명 오타 확인

### 머지 방식
- Squash and merge 사용
- 커밋 히스토리 깔끔하게 유지

---

## 코드 스타일

### 자동 포맷팅
```bash
pnpm format    # 포맷팅 적용
pnpm lint      # 린트 검사
pnpm lint:fix  # 린트 자동 수정
```

### Import 순서
```typescript
// 1. 외부 라이브러리
import React from 'react'
import { useState } from 'react'
import axios from 'axios'

// 2. 내부 절대 경로 (@/)
import { Button } from '@/components/ui/button'
import { useAuth } from '@/hooks/useAuth'
import { Question } from '@/types/question'

// 3. 상대 경로
import { QuestionCard } from './QuestionCard'
import { styles } from './styles'

// 4. 타입 import는 type 키워드 사용
import type { User } from '@/types/user'
```

### 네이밍 컨벤션
```typescript
// 변수/함수: camelCase
const userId = '123'
const getUserName = () => {}

// 상수: UPPER_SNAKE_CASE
const API_BASE_URL = 'https://api.preterview.com'
const MAX_RETRY_COUNT = 3

// 컴포넌트/클래스: PascalCase
const QuestionCard = () => {}
class UserService {}

// 타입/인터페이스: PascalCase
interface Question {}
type UserId = string

// Private 변수: _prefix
class Example {
  private _privateValue: string
}

// Boolean: is/has/should prefix
const isLoading = true
const hasError = false
const shouldUpdate = true
```

### TypeScript 규칙
```typescript
// 명시적 타입 선언
const userId: string = '123'
function getUser(id: string): User {}

// any 사용 지양
const data: any = {}  // 피하기

// unknown 또는 구체적 타입 사용
const data: unknown = {}
const response: ApiResponse = {}

// Interface 우선 (확장 가능)
interface User {
  id: string
  name: string
}

// Type은 Union/Intersection 등 필요 시
type Status = 'pending' | 'success' | 'error'
type UserWithRole = User & { role: string }

// Optional chaining
user?.profile?.name

// Nullish coalescing
const name = user?.name ?? 'Guest'
```

### React 규칙
```typescript
// 함수 컴포넌트 + 화살표 함수
export const QuestionCard = ({ question }: Props) => {
  return <div>{question.title}</div>
}

// Props 타입 정의
interface QuestionCardProps {
  question: Question
  onDelete?: (id: string) => void
}

// 훅 순서
const [state, setState] = useState()
const { data } = useQuery()
const dispatch = useDispatch()
useEffect(() => {}, [])

// Early return
if (!data) return <Loading />
if (error) return <Error />
return <Content />
```

### 파일 구조
```typescript
// 1. Import
import React from 'react'

// 2. 타입 정의
interface Props {}

// 3. 상수
const MAX_LENGTH = 100

// 4. 컴포넌트
export const Component = (props: Props) => {
  // 4-1. Hooks
  const [state, setState] = useState()
  
  // 4-2. 이벤트 핸들러
  const handleClick = () => {}
  
  // 4-3. 렌더 함수
  const renderContent = () => {}
  
  // 4-4. Return
  return <div />
}

// 5. 스타일
const Styled = styled.div``
```

### 주석 규칙
```typescript
// 복잡한 로직에만 주석
// 사용자 권한에 따라 필터링 (admin은 모든 질문, user는 본인 질문만)
const filteredQuestions = questions.filter(q => 
  user.role === 'admin' || q.userId === user.id
)

// 자명한 코드에 주석 X
const name = user.name  // 이름을 가져옴 (불필요)
```

### Prettier 설정
```json
{
  "semi": false,
  "singleQuote": true,
  "tabWidth": 2,
  "trailingComma": "none",
  "printWidth": 100,
  "arrowParens": "avoid"
}
```

### ESLint 주요 규칙
```json
{
  "rules": {
    "no-console": "warn",
    "no-unused-vars": "error",
    "@typescript-eslint/no-explicit-any": "error",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn"
  }
}
```

---

## 셀프 코드 리뷰

- 코드가 요구사항을 충족하는가?
- 불필요한 코드가 없는가?
- 에러 처리가 적절한가?
- 테스트가 작성되었는가?
- 성능 이슈가 없는가?
- 보안 취약점이 없는가?
- 문서화가 필요한가?