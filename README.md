## Mobile Game A/B Testing

### 1. Mobile Game A/B Testing 목차
* 데이터 소개
* 문제 정의
* 기대 효과
* 문제 해결 프로세스
* 결과 및 해결방안

### 2. 데이터 소개
A/B Testing에 사용하는 데이터는 [Mobile Games A/B Testing - Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)를 참고해서 진행했습니다. 데이터는 유저별 게임 version에 따른 sum_gamerounds / retention 1 / retention 7 정보를 포함하고 있습니다.

### 3. 문제 정의
새롭게 적용 예정인 version_40이 유저들의 게임 경험(전체 game rounds / Retention 1, 7)에 미치는 영향을 알 수 없다.

### 4. 기대 효과
version_40가 긍정적이라면, 유저들의 플레이 시간(game rounds)과 재방문(Retiontion 1, 7)율을 높일 수 있다.

### 5. 문제 해결 프로세스
#### 5.1 가설 수립 및 검정 방법
* 가설 수립 - 새롭게 적용되는 version_40은 유저들의 게임 경험(전체 game rounds / retention 1, 7)을 높인다.
* 가설 검정 - version(30/40)이 유저의 경험에 미치는 영향을 파악하기 위해 A/B Testing을 진행한다.

#### 5.2 실험군 / 대조군 정의
* 대조군 - 과거 version. version_30을 플레이한 유저
* 실험군 - 새로운 version. version_40을 플레이한 유저

#### 5.3 데이터 탐색 결과
다음과 같이 3개 그룹의 유저로 나눠서 A/B Test를 진행한다.
* 전체 유저
* 플레이 경험이 많은 유저 제외
* 플레이 경험이 많거나 적은 유저 제외

#### 5.4 A/B Testing
version이 sum_gamerounds(연속형) / retention 1, 7(범주형)에 미치는 영향을 증명한다.

##### 5.4.1 독립 표본 t 검정
* H0. 귀무 가설 - version_30의 sum_gamerounds 표본 평균과 version_40의 sum_gamerounds 표본 평균은 동일하다.
* H1. 대립 가설 - version_30의 sum_gamerounds 표본 평균과 version_40의 sum_gamerounds 표본 평균은 다르다.

version이 sum_gamerounds(범주형)에 미치는 영향을 분석하기 위해 **독립 표본 t검정**을 진행한다. 가설 검정을 진행하기에 앞서 두 그룹간의 독립 / 정규성 / 등분산 검정을 먼저 수행한다. 이후에 독립 표본 t검정을 진행해 pvalue와의 비교를 통해 version이 sum_gamerounds에 미치는 영향을 분석한다.

##### 5.4.2 카이 제곱 검정
* H0. 귀무 가설 - version(범주형 변수)와 retention 1, 7(범주형 변수)는 독립이다.
* H1. 대립 가설 - version(범주형 변수)와 retention 1, 7(범주형 변수)는 종속이다.

version이 retention(범주형)에 미치는 영향을 분석하기 위해서 **카이제곱검정**을 진행한다. 카이제곱검정을 진행해 pvalue와의 비교를 통해 version이 retention에 미치는 영향을 분석한다.

### 6. 결과 및 해결방안
게임을 이용하는 유저들을 크게 3가지(전체 / 이상치 제거 / 일반)로 나눠서 version이 미치는 영향을 살펴봤다.

* version / sum_gamerounds - 3가지 모든 경우에 대해서 version이 sum_gamerounds에 미치는 영향이 없다고 판단한다.
* version / retention 1 - 3가지 모든 경우에 대해서 version이 sum_gamerounds에 미치는 영향이 없다고 판단한다.
* version / retention 7 - 3가지 모든 경우에 대해서 version이 retention 7에 미치는 영향이 있다고 판단한다. 하지만, 부정적 영향이 미치는 것으로 판단했다.

**해결 방안**으로 현재의 update(version 40)은 진행하면 안 된다. 유저의 플레이 경험에 영향을 주지 못 하며, 오히려 일주일이 지나면 접속 자체를 낮춘다. 그래서 현재의 버전을 유지하거나 다른 방향으로의 업데이트를 진행해야 한다.
