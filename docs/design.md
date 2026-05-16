# 설계 문서

## 1. 아키텍처 개요
Expense Tracker는 단일 페이지 웹 애플리케이션(SPA) 형태로 설계한다.
별도의 백엔드 서버나 데이터베이스 없이 클라이언트 측에서 모든 로직을 처리한다.

구조: 사용자 → HTML UI → JavaScript 로직 → 메모리 내 데이터 배열

## 2. 모듈 구성

| 모듈 | 역할 |
|------|------|
| UI Module | 입력 폼, 거래 내역 목록, 요약 정보 렌더링 |
| Transaction Module | 거래 추가/삭제, 데이터 배열 관리 |
| Calculation Module | 총수입, 총지출, 잔액 계산 |
| Validation Module | 입력값 유효성 검증 |

## 3. 데이터 구조

거래 내역은 객체 배열로 관리한다.

```javascript
{
  id: number,          // 고유 식별자
  type: string,        // "income" 또는 "expense"
  description: string, // 항목 설명
  amount: number       // 금액
}
```

## 4. 주요 함수 설계

| 함수명 | 기능 | 입력 | 출력 |
|--------|------|------|------|
| addTransaction() | 거래 추가 | type, description, amount | void |
| deleteTransaction(id) | 거래 삭제 | id | void |
| calculateBalance() | 잔액 계산 | - | {income, expense, balance} |
| validateInput() | 입력 검증 | description, amount | boolean |
| renderList() | 목록 화면 갱신 | - | void |

## 5. 화면 설계 (Wireframe)

와이어프레임은 [wireframe.html](./wireframe.html) 파일을 참고한다.

## 6. 요구사항 추적 매트릭스 (RTM)

| 요구사항 ID | 대응 모듈 / 함수 |
|------------|------------------|
| FR-01, FR-02 | addTransaction() |
| FR-03 | deleteTransaction() |
| FR-04 | calculateBalance() |
| FR-05 | validateInput() |
| FR-06 | renderList() |

## 7. 설계 결정 사항

- **프레임워크 미사용**: 학습 목적상 순수 JS로 구현해 동작 원리를 명확히 파악
- **DB 미사용**: 요구사항 NFR-01(즉시 실행)을 만족하기 위해 메모리 저장 방식 선택
- **단일 파일 구조**: 소규모 프로젝트 특성상 index.html 하나에 통합

## 8. 적용한 설계 원칙

- **단일 책임 원칙(SRP)**: 각 함수는 하나의 역할만 담당
- **낮은 결합도, 높은 응집도**: 모듈 간 의존성 최소화
- **DRY**: 반복되는 렌더링 로직은 renderList() 하나로 통합
