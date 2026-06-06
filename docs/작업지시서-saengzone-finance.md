# 작업지시서 — 생존(saengzone) 금융계산기 이식

**대상 저장소:** `nroadcompany-ux/saengzone` (Next.js · TypeScript)
**목표 경로:** `saengzone.vercel.app/finance`
**작성일:** 2026-06-06
**상태:** 베이스 코드 완성 → Next.js 이식만 진행

---

## 0. 한 줄 요약

이미 **18종 계산기 로직·디자인·수식 검증이 끝난 단일 HTML 베이스 코드**가 있습니다.
개발자(또는 다음 세션)는 **로직을 새로 짜지 말고**, 이 베이스를 Next.js 컴포넌트로 분리 + Tailwind 전환만 하면 됩니다.

---

## 1. 베이스 코드 위치 (가장 중요)

- **저장소:** `nroadcompany-ux/nroad-education`
- **파일:** `finance/index.html` (986줄, 자기완결형, 빌드 불필요)
- **Raw:** `https://raw.githubusercontent.com/nroadcompany-ux/nroad-education/main/finance/index.html`
- **라이브 데모:** `https://nroadcompany-ux.github.io/nroad-education/finance/`

이 파일 하나에 다음이 전부 들어 있습니다:
- 18종 계산기 로직 (`CALCS` 객체, id별 `body()` + `init()`)
- 생존 디자인 시스템 CSS (`:root` 변수 = 아래 토큰과 1:1)
- 해시 라우팅(`#/loan` 등), PayPlay CTA 배너, SEO 메타, GA 이벤트 스텁

> ⚠️ 계산 수식은 이미 전수 검증됨(섹션 5). **수식은 복붙**하고 UI만 React로 옮기세요.

---

## 2. 구현할 계산기 18종 (베이스 `CALCS` 키 = 컴포넌트명)

| # | CALCS key | 카테고리 | 계산기명 | 핵심 기능 |
|---|---|---|---|---|
| 1 | `loan` | 대출 | 대출계산 | 원리금균등/원금균등/만기일시 (3탭) |
| 2 | `loanCompare` | 대출 | 대출비교 | A vs B 나란히 |
| 3 | `savings` | 예적금 | 적금계산 | 단리/복리 × 세금유형 |
| 4 | `deposit` | 예적금 | 예금계산 | 단리/월복리 × 세금유형 |
| 5 | `bestRate` | 예적금 | 최고금리찾기 | 상품목록 + 슬라이더 필터 |
| 6 | `targetAmount` | 예적금 | 목표금액 | 역산(목표→월저축) |
| 7 | `profit` | 투자 | 수익률(국내) | 수수료+증권거래세 |
| 8 | `profitGlobal` | 투자 | 수익률(해외) | 환율·해외수수료 |
| 9 | `avgPrice` | 투자 | 평단가계산 | 추가매수 평균단가 |
| 10 | `feeCheck` | 투자 | 수수료진단 | 4단계 위저드 |
| 11 | `feeRate` | 투자 | 수수료율계산 | 국내/해외 |
| 12 | `exchange` | 환율 | 환율계산 | 8개 통화 |
| 13 | `netSalary` | 생활 | 실수령계산 | 연봉→월 실수령(공제 상세) |
| 14 | `salaryIncrease` | 생활 | 연봉인상률 | 인상률+월 인상액 |
| 15 | `severance` | 생활 | 퇴직금계산 | 근속일수 × 평균임금 |
| 16 | `pyeong` | 생활 | 평수계산 | ㎡↔평 양방향 |
| 17 | `memo` | 생활 | 계산노트 | 항목 합산 |
| 18 | `basic` | 생활 | 기본계산기 | 사칙연산 |

---

## 3. 디자인 시스템 (베이스 `:root` → Tailwind theme)

```ts
// tailwind.config.ts → theme.extend.colors
primary:      '#00C471',   // 생존 바이탈 그린
primaryDark:  '#00A85E',
primaryLight: '#E8FBF3',
primarySoft:  '#F0FDF8',
navy:         '#0D1B2A',
```

- **폰트:** Pretendard (`@import` 또는 `next/font` 로컬). 토스뱅크 동일.
- **인풋:** h-56px / radius-12 / border-1.5 / focus 시 그린 / font 18px·600
- **계산 버튼:** h-56px / radius-14 / 그린 / shadow `0 4px 20px rgba(0,196,113,.3)`
- **결과카드:** radius-20 / padding-24 / 결과금액 32px·800 navy
- **컨셉:** "토스뱅크 미니멀 × 생존 그린" — 큰 숫자, 풍부한 여백

> 베이스 HTML의 `<style>` 블록이 이 규격 그대로이니, 클래스명만 Tailwind로 기계적 치환하면 됩니다.

---

## 4. 권장 파일 구조 (App Router)

```
app/finance/
├── layout.tsx              # 생존 네브바 공유 + SEO 메타
├── page.tsx                # 랜딩(18종 그리드)
└── [calc]/page.tsx         # 개별 계산기 (또는 단일 페이지 탭 유지)
components/finance/
├── CalculatorShell.tsx     # 공통 래퍼(뒤로/제목/부제)
├── InputField.tsx          # F() → 컴포넌트
├── ResultCard.tsx          # card() → 컴포넌트
├── Segmented.tsx           # SEG() → 토글/탭
├── PayplayCTA.tsx          # 하단 전환 배너
└── calculators/
    ├── LoanCalc.tsx ... BasicCalc.tsx   # CALCS 18개 1:1
```

**이식 패턴 (각 계산기 공통):**
1. 베이스 `CALCS.<id>.body()` 의 인풋 HTML → JSX
2. 베이스 `CALCS.<id>.init()` 의 `compute()` 로직 → `useState` + 핸들러 (수식 그대로)
3. 인라인 className → Tailwind 클래스 (토큰명 동일)

---

## 5. 검증된 계산 수식 (그대로 사용 — 재작성 금지)

```js
// 대출 원리금균등
const r = 연이율/100/12;
const monthly = p*r*Math.pow(1+r,n)/(Math.pow(1+r,n)-1);
// 대출 원금균등: 총이자 = p*r*(n+1)/2
// 대출 만기일시: 총이자 = p*r*n

// 적금 단리: 이자 = 월납*(n*(n+1)/2)*월이율
// 적금 복리: 총액 = 월납*((1+r)^n-1)/r*(1+r)
// 예금 단리: 이자 = p*(연이율/100)*(n/12)
// 예금 월복리: 총액 = p*(1+월이율)^n

// 세율
const TAX = { 일반과세:0.154, 비과세:0, 세금우대:0.099 };

// 실수령(간이세액 근사): 월 과세급여 구간별 소득세
// <106만:0 / <150만:(x-106만)*.06 / <300만:26400+(x-150만)*.15
// <450만:251400+(x-300만)*.24 / else:611400+(x-450만)*.35
// 국민연금 min(x*.045, 248850) / 건강 x*.03545 / 장기요양 건강*.1281
// 고용 x*.009 / 지방소득세 소득세*.1

// 환율(2026-06-05): KRW1540 USD1 EUR0.86 CNY6.77 JPY159.85 GBP0.79 HKD7.78 AUD1.55
// 변환: amount / rates[from] * rates[to]

// 평수: 1평 = 3.305785㎡
// 퇴직금: 1일평균임금 = 3개월임금/91.25; 퇴직금 = 1일평균*30*(재직일수/365)
```

> 더 자세한 검증값은 베이스 파일 주석 + nroad-education 원본 작업지시서 섹션 5 참조.

---

## 6. 생존 연동 (이 프로젝트 고유 작업)

- **헤더:** 생존 글로벌 네브바 공유, "금융계산기" 메뉴 활성화
- **PayPlay CTA:** 결과 하단 고정 배너 유지 (그린 그라디언트)
  - 링크: `process.env.NEXT_PUBLIC_PAYPLAY_URL` (기본 `https://payplay.co.kr`)
  - 전환 퍼널: 금융계산기(무료) → "PayPlay로 매장 시작" → 가입
- **SEO:** `<title>금융계산기 | 생존 - 소상공인 창업 플랫폼</title>` + og:image `/finance/og-finance.png`
- **GA(선택):** `calculator_used {calculator_type}`, `cta_click {source:'finance_calculator'}`

---

## 7. 배포

- Vercel(기존 saengzone 프로젝트)에 `/finance` 경로 추가 → `saengzone.vercel.app/finance`
- 환경변수: `NEXT_PUBLIC_PAYPLAY_URL`, `NEXT_PUBLIC_GA_ID`

---

## 8. 완료 체크리스트

- [ ] 18종 전부 동작 (수식 = 섹션 5)
- [ ] 모바일 375~430px 대응
- [ ] 생존 네브바/헤더 연동
- [ ] PayPlay CTA 노출 + 링크
- [ ] SEO 메타 + og 이미지
- [ ] `saengzone.vercel.app/finance` 배포 확인
- [ ] 토스뱅크급 디자인 퀄리티(여백·큰 숫자)
