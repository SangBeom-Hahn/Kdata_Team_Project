# Process Optimization Strategy
*Kdata_Campus_Team_Project

## Description
본 프로젝트는 기계부품 제조업 공장의 생산성 증대를 위한 최적의 생산 운영 계획 및 전략을 제시한다. MES 데이터 분석을 통해 생산 현황을 파악하고 공정에서의 병목 원인을 추측하며 안전 재고량을 계산한다. 이를 통해서 고객 만족도 향상, 작업 및 품질 개선을 목표로 한다.

## Files




## Dataset
```
├── 경영지표
|   ├── 01. 공정별 생산제품.xlsx
|   ├── 02. 설비별 생산제품.xlsx
|   ├── 03. 설비가동리스트.xlsx
|   ├── 04. 장기정체재공.xlsx
|   ├── 05. 품질리스트.xlsx
|   ├── 06. 재공리스트.xlsx
|   └── 07. 수주리스트.xlsx
|
├── 생산실적조회
|   ├── 01. 생산제품.xlsx
|   ├── 02. 모델.xlsx
|   ├── 03. 공정세그먼트.xlsx
|   ├── 04. 공정.xlsx
|   ├── 05. 설비.xlsx
|   └── 06. 라우트.xlsx
|
├── 가공부서_CYCLE_TIME
|   ├── CNC
|   |   └── CNC_cycle_time.xlsx 
|   ├── MCT
|   |   └── mct_cycle_time.xlsx
|   ├── 리드타임_추출_원본.xlsx
|   └── 수주현황.xlsx
```

### 최종 데이터 셋 컬럼
|컬럼명|설명|
|:-:|:-:|
|날짜|특정 제품의 공정을 시행 날짜|
|품명|제품명|
|MCT_생산량|해당 제품의 MCT 공정 결과 생산량|
|MCT_입고량|해당 제품의 MCT 공정 중에 발생한 입고량|
|MCT_양품수|해당 제품의 MCT 공정 결과 양품수|
|MCT_표준공수|해당 제품의 MCT 작업량|
|MCT_싸이클타임|해당 제품의 MCT 공정 소요시간|
|CNC_입고량|해당 제품의 CNC 공정 중에 발생한 입고량|
|CNC_생산량|해당 제품의 CNC 공정 결과 생산량|
|CNC_양품수|해당 제품의 CNC 공정 결과 양품수|
|CNC_표준공수|해당 제품의 CNC 작업량|
|CNC_사이클타임|해당 제품의 CNC 공정 소요시간|
|14~20열|7개 공정 각각의 리드타임|
|총 리드타임|해당 제품 생산의 총 소요시간|
|총 생산량|해당 제품의 총 생산 개수|
|총 입고량|해당 제품의 총 입고 수량|



## Preprocessing

## EDA

## Solution
### 1. 수요 전략 및 생산 운영 계획 관점

![sol1](https://github.com/SangBeom-Hahn/Kdata_Team_Project/blob/main/solution/sol1.PNG)

수요량과 생산량 그래프를 이중 축으로 나타낸 결과 AY, BU, ZD 고객사의 제품은 생산량이 수요량보다 많았다. AY, BU, ZD 고객사의 1달간 수요량을 이동평균법으로 예측한 결과 예측된 수요량에 비해 재고량이 압도적으로 많아서 생산량을 줄여야 한다는 결론을 도출할 수 있다.



### 2. 재고 관리 관점

![sol1](https://github.com/SangBeom-Hahn/Kdata_Team_Project/blob/main/solution/sol2.PNG)

DP, KT 고객사는 수요량이 생산량보다 많을 시기가 종종 있었고 이러한 원인을 안전 재고량을 고려하지 않았기 때문이라고 판단하였다. 따라서 공식에 따라 안전 제고량을 계산하였다.



### 3. 최적의 공정 관점

![sol1](https://github.com/SangBeom-Hahn/Kdata_Team_Project/blob/main/solution/sol3.PNG)

프로세스 마이닝 결과 동일한 제품이 다양한 라우트로 생산 된다는 점을 발견하였다. 이 점은 해당 기업이 job-shop 형태의 공정과정을 사용하기 때문이다. 동일한 제품의 다양한 라우트 중 리드 타임이 짧은 라우트가 최적의 공정임을 알 수 있다.

![sol1](https://github.com/SangBeom-Hahn/Kdata_Team_Project/blob/main/solution/sol3-2.PNG)

이러한 차이가 발생하는 원인이 병목이라고 의심하였고, 이를 시뮬레이션을 통해 해결할 수 있었다.





## Q&A
Q) 프로세스 마이닝을 실제 공정에 바로 사용 가능한가?
A) 물론 pt 결과도 실제 데이터를 처리한 것이지만 과제를 진행하면서 전처리와 데이터 병합을 진행했다. 프로세스 마이닝을 돌리기 위해서는 데이터를 전처리하던가 아니면 미래지향적으로 데이터를 측정할 때 프로세스 마이닝 포멧으로 수집되도록 데이터 수집 절차에 변화를 주는게 옳다.
<hr>

Q) 파트와 아세이를 구분하지 않고 결과를 돌렸는데 신빙성이 있다고 볼 수 있나?
A) 파트가 모여 아세이를 구성하므로 파트의 공정 과정에 변화를 주면 아세이도 영향을 받을 것이라고 생각한다. 하지만 이 또한 데이터 수집 과정에서 파트와 아세이를 구분하여 다른 파일에 수집되도록 하는 것이 현명하다.
<hr>

Q) 리드타임 예측은 왜 불가능한가?
A) 최초 목표로 잡았던 리드타임 예측을 하기에는 데이터의 컬럼이 부족하다고 판단되었다. 또한 시계열 예측을 하기 위한 계절성이나 추세가 발견되지 않았다.



## Feedback

## reference

