# 📊 Blinkit BI Dashboard  
### 카테고리 수익 구조 및 악성재고 모니터링 대시보드

>Quick Commerce 기업 **Blinkit**의 매출·마진·재고 데이터를 통합 분석하여  
>MD 의사결정을 지원하는 BI 대시보드를 구축했습니다.

“어디에 리소스를 먼저 투입해야 수익이 개선되는가?” 를 정량적으로 답하는 것이 본 프로젝트의 핵심 목표입니다.

---

## 1. 프로젝트 개요

- **분석 기간**: 2026.02 ~ 2026.02
- **Stakeholder**: MD 팀 팀장  
- **목적**: 카테고리 수익성 진단 및 Dead Stock 리스크 관리  
> 🔗 **Dashboard Link**  https://lookerstudio.google.com/s/vMMvuDnGdD0

---
## 2. 문제 정의

### 기존 문제 상황

- 카테고리별 매출은 확인 가능했으나 **매출 대비 마진 효율 판단 체계 부재**
- 저매출·고마진 카테고리 식별 어려움
- Dead Stock(30일 이상 미판매 재고)에 대한 선제적 대응 체계 부족
  
### 비즈니스 질문 구조화
1️⃣ 우선순위형 질문

- 가용 리소스를 우선 투입해야 할 핵심 카테고리는 어디인가?
- 매출·마진 구조 상 수익 개선 효과가 가장 높은 카테고리는 어디인가?

---

2️⃣ 상태확인형 질문

- 현재 매출, 총 마진, 주문 건수의 전주 대비 증감 현황은 어떠한가?
- 카테고리 내 주요 품목의 판매량, 마진, 악성 재고 현황은 어떠한가?

---

3️⃣ 비교판단형 질문

- 카테고리별 매출 대비 마진 기여도와 원가 비중의 차이는 어떠한가?
- 판매 상위(Top 3)와 하위(Bottom 3) 품목 간의 분기·월별 성장 추이는 어떠한가?
- 품목별 악성 재고로 인한 실질적인 손실 규모는 얼마인가?

### BI 표현 목표

1. 카테고리별 매출 · 마진 구조를 4분면으로 시각화
2. 전략적 확대 대상 카테고리 식별
3. 악성 재고 손실 규모 정량화 및 우선순위 도출

---
    
## 3. 사용 기술

> ### Data Processing & Analysis

| 기술 | 사용 목적 | 세부 활용 내용 |
|------|------------|----------------|
| **Python (Pandas)** | 데이터 정제 및 통합 | Multi-table join (orders, order_items, products, inventory), groupby 집계, 누적합 계산 |
| **Datetime 처리** | 기준일 계산 | 30일 기준 Dead Stock 판별, 주차/연월 파생 변수 생성 |
| **파생 변수 설계** | KPI 생성 | 매출, 총 마진, 가중 평균 마진율, 재고 누적 계산 |

---

> ### Database & Query

| 기술 | 사용 목적 | 세부 활용 내용 |
|------|------------|----------------|
| **SQL** | KPI 집계 및 로직 구현 | GROUP BY 집계, CASE WHEN 사분면 분류, 서브쿼리, 가중 평균 계산 |
| **ERD 설계** | 데이터 모델링 | Row (orders, order_items) / Dimension (products, customers, inventory) 구조 설계 |
| **조인 설계** | 통합 분석 구조 구축 | 주문 → 상품 → 재고 흐름 기반 분석 구조 구현 |

---

> ### BI & Visualization

| 기술 | 사용 목적 | 세부 활용 내용 |
|------|------------|----------------|
| **Looker Studio** | 대시보드 설계 | KPI 스코어카드, 4분면 산점도, Stacked Bar, 시계열 분석 |
| **Field** | 동적 지표 생성 | CASE WHEN 분류 로직, DATE 파싱, 가중 평균 계산식 구현 |
| **필터 구조 설계** | 인터랙션 설계 | 주차/연월/카테고리 필터 기반 의사결정 흐름 구현 |
| **Dashboard UX 설계** | 의사결정 지원 | Page1(수익성 판단) → Page2(재고 대응) 구조화 |

---

> ### Business Analytics Capability

| 역량 | 적용 내용 |
|------|------------|
| **KPI 정의 및 구조화** | 매출·마진·Dead Stock 기준 재정의 |
| **사분면 분석 모델 설계** | 고매출·고마진 / 저매출·저마진 전략 구분 |
| **재고 리스크 정량화** | 손실 비용 기반 우선순위 도출 |
| **의사결정 질문 구조화** | 우선순위형 / 상태확인형 / 비교판단형 질문 설계 |

---

## 4. KPI 정의

| KPI | 정의 | 계산식 |
|------|------|--------|
| **Gross Revenue** | 총 매출 | quantity × mrp |
| **Gross Profit** | 총 마진 | (mrp - price) × quantity |
| **Order Count** | 주문 건수 | COUNT(DISTINCT order_id) |
| **Dead Stock** | 30일 이상 미판매 재고 | MAX(order_date) - 30일 기준 |

---

## 5. 데이터 구조

### 프로젝트 구조 및 설정

---

### 📂 폴더 구조

```bash
BIproject/
├── data/                # 원본 데이터셋
├── docs/                # BI 기획서 및 대시보드 설명 문서
├── outputs/             # KPI 산출 테이블 및 대시보드 결과물
├── DA/                  # 데이터 전처리 및 JOIN 산출 notebooks
└── README.md
```

---

### 원본 데이터셋 구성 (9개 테이블)

| CSV 파일명 | 테이블명 | 행 수 | 비고 |
|------------|----------|--------|------|
| blinkit_orders.csv | orders | 5,000 | 주문 마스터 |
| blinkit_order_items.csv | order_items | 5,000 | 주문 상세 |
| blinkit_customers.csv | customers | 2,500 | ⚠️ address 컬럼에 줄바꿈(\n) 포함 |
| blinkit_products.csv | products | 268 | 상품 마스터 |
| blinkit_delivery_performance.csv | delivery_performance | 5,000 | 배송 실적 |
| blinkit_customer_feedback.csv | customer_feedback | 5,000 | 고객 피드백 |
| blinkit_inventory.csv | inventory | 75,172 | ⚠️ 날짜 형식: DD-MM-YYYY |
| blinkit_inventoryNew.csv | inventory_new | 18,105 | ⚠️ 날짜 형식: Mon-YY |
| blinkit_marketing_performance.csv | marketing_performance | 5,400 | 마케팅 실적 |

---

### ⚠️ 데이터 전처리 주의사항

- **inventory**
  - 날짜 형식: `DD-MM-YYYY`
  - 예시: `17-03-2023`
  - 👉 `datetime` 변환 필요

- **inventory_new**
  - 날짜 형식: `Mon-YY`
  - 예시: `Mar-23`
  - 👉 연/월 기준 변환 필요

- **customers**
  - `address` 컬럼에 줄바꿈(`\n`) 약 1,900건 포함
  - 👉 문자열 정제 필요 (replace 또는 정규식 활용)

---
### Low Tables
- `orders` (주문 1건 grain)
- `order_items` (주문 × 상품)

### Dimension Tables
- `products`
- `customers`
- `inventory`
- `delivery_performance`
- `marketing_performance`

### Join 구조

orders → order_items → products  
orders → customers  
products → inventory
> 산출 테이블 : `Business.csv`, `Dead_stock.csv`, `stock_dashboard.csv` 

→ 매출 / 마진 / 재고 리스크를 하나의 분석 구조로 통합 설계

---

## 6. 대시보드 구조

### 📄 Page 1: 매출 & 마진 구조 분석

- KPI 스코어카드 (주간 증감 비교)
- 카테고리 매출-마진 4분면 산점도
- 카테고리별 원가 + 마진 구조 (Stacked Bar)
- 매출 Top3 / 마진 Top3 카테고리

**의사결정 포인트**

- 고매출·고마진 → 핵심 유지/확대
- 저매출·고마진 → 성장 잠재 카테고리
> **고매출·저마진 → 가격/원가 구조 개선 필요 → 매출/마진 상승 기대 분면**
- 저매출·저마진 → 운영 축소 검토

---

### 📄 Page 2: 품목 & 재고 분석

- 카테고리별 월별 판매 추이
- 품목별 판매량·총 마진 테이블
- Dead Stock 리스트 및 손실 비용 산출
- Top3 / Bottom3 품목 월별·분기별 추이 분석

**의사결정 포인트**

- 성장 품목 → 추가 소싱 및 재고 확보
- 판매 하위 품목 → 운영 중단 또는 프로모션 검토
- 손실 규모 높은 Dead Stock → 즉각 대응 필요

---

## 6. 주요 인사이트

- 일부 고매출 카테고리에서 마진율이 낮게 나타남 → 프로모션 의존 구조 가능성
- Dairy & Breakfast 카테고리 Dead Stock 손실 비용 최대
- 판매 Bottom 품목은 지속적 하락 추세 → 재고 운영 재검토 필요

---

## 7. 팀 구성 및 역할

| 팀원 | 역할 및 기여 내용 |
|------|------------------|
| **김O연** | • 주제 및 분석 방향성 아이디어 제시 <br> • 데이터 시각화를 위한 기획 및 와이어프레임 제작 <br> • 분석 결과 기반 Looker 대시보드 작성 |
| **김O비** | • 주제 및 분석 방향성 아이디어 제시 <br> • 프로젝트 데이터 분석 및 전처리 수행 <br> • 핵심 KPI 인사이트 도출 |
| **조O준** | • 주제 및 분석 방향성 아이디어 제시 <br> • 대시보드 설명서 작성 및 재검토 <br> • Looker 대시보드 작성 |
| **전O솔** | • 주제 및 분석 방향성 아이디어 제시 <br> • 데이터 검증 수행 |
| **최O민** | • 주제 및 분석 방향성 아이디어 제시 <br> • 프로젝트 진행 감독 <br> • 프로젝트 기획서 작성 및 재검토 |
| **최O진** | • 분석 방향성 아이디어 제시 <br> • 프로젝트 기획서 작성 <br> • 대시보드 설명서 작성 및 재검토 |

---

## 8. 실행 환경

- Python 3.10~11.x
- pandas
- Looker Studio
- Google Silde

---
## 9. 협업 규칙

> Git 커밋 컨벤션
- [feat]: 새로운 기능 추가 (전처리 로직 등)
- [fix]: 버그 수정
- [docs]: 문서 수정 (README, 회의록 등)
- [refactor]: 코드 구조 개선