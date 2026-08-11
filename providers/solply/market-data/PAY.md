---
name: market-data
title: "Solply Food Supply Trade Index"
description: "Real-world food-supply settlement data from a franchise ecosystem - executed price and demand indexes, verified by on-chain settlement, sold via x402"
use_case: "Executed prices and demand trends for real-world food-supply SKUs (not crypto tokens). Every sample is an on-chain settled trade on Solana. For supply-chain price research and procurement negotiation grounding."
category: data
service_url: https://solply-api-965647250280.us-central1.run.app
openapi:
  path: openapi.json
---

프랜차이즈 본사-가맹점 식자재 대금(물대) 자율 정산 생태계 **Solply**가 실거래에서
집계한 데이터 상품. 에이전트들이 x402로 협상·정산한 체결(청구서 정산 + 지점 간
직거래)만 표본으로 쓴다 — 모든 표본에 온체인 증빙이 있다.

> **결제 네트워크: Solana devnet USDC** (해커톤 데모 생태계 — 메인넷 전환 전).
> 유료 경로는 `/x402/data/*` 두 개뿐이며, 나머지 API는 데모 대시보드용 공개 조회다.

## 상품

| 경로 | 상품 | 내용 |
|---|---|---|
| `GET /x402/data/market/{sku}` | 체결가 지수 | 최근 7일 가중 평균 단가 · 표본수(본사 발주/직거래 구분) · 전주 대비 추세 |
| `GET /x402/data/demand/{sku}` | 수요 지수 | 최근 7일 판매량 · 일평균 · 참여 지점 수 · 추세 |

구매 흐름은 표준 x402: GET이 `402`와 주문서(memo = 주문 ID)를 주고, USDC 지불 후
`POST /x402/data/orders/{order_id}/settle`에 서명을 제출하면 지수를 인도한다.
주문 하나는 한 번만 이행된다 (같은 서명의 재시도는 멱등).

## Spend-aware usage

- 지수는 7일 창 집계라 분 단위로 변하지 않는다 — **같은 SKU를 10분 안에 두 번 사지 말 것**.
- SKU 목록은 무료 조회(`GET /api/shop`)로 먼저 확인하고, 필요한 품목만 살 것.
- 추세만 필요하면 `market` 하나로 충분하다 — `demand`는 수량 계획이 필요할 때만.
