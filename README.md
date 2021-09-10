# 🥚 감성 분석 기반의 상권 분석 모델 제시 및 실효성 확인

> 2021.06.30 - 2021.09.06
>
> 2021 금융 데이터 경진대회 팀 BGJ 김채은, 김현욱, 안수빈

![banner](https://i.imgur.com/XwMJ3dJ.jpg)

<br>

**🍳 분석 배경/목표**

<details>
<summary>상권 및 상권 분석 정의</summary>
<div markdown="1">       
상권은 상업적 거래가 이루어지는 공간적 범위 혹은 배후지를 의미하며, 해당 상권이 특정 업종에 적합한지 등을 다각도로 분석하는 것을 상권 분석이라고 한다. 이러한 상권분석은 창업 위험을 낮추기 위해 반드시 필요하며 판매 예상량을 예측하거나 마케팅 전략을 수립하는 데에도 사용된다. 이에 서울특별시는 우리마을가게 상권분석 서비스를 제공하며 창업자 및 자영업자에게 유용한 정보를 제공하고 있다.
</div>
</details>

<details>
<summary>코로나19로 인한 상권 변화 확인</summary>
<div markdown="1">       
하지만 2019년 12월 발생한 코로나19는 소비 패턴을 크게 바꾸며 상권의 변화를 가져왔다. 기존 발달 상권으로 분류되던 강남, 홍대, 이태원 일대는 사회적 거리 두기 강화로 인해 유령화되어가고 있으며 공실률 역시 높아졌다. 실제로 8월 23일 신한카드 빅데이터 연구소에 의하면 홍대 일대의 카드 이용 건수는 2021년 1분기에 전년 동기 대비 50% 넘게 하락했다. 반면, 주거 지역은 재택근무, 온라인 수업 등으로 인해 오히려 상권이 활성화되기도 했다. 온라인 구매로의 확산 또한 코로나가 가져온 소비 패턴의 변화 중 하나이다.
</div>
</details>

<details>
<summary>감성 분석 기반의 상권 분석 모델 제시</summary>
<div markdown="1">       
이처럼 코로나 이후의 상권 분류에는 이전과는 다른 변수들의 영향이 포함되기 시작했다. 특히, 비대면 거래의 증가로 인해 수치만으로는 알 수 없는 온라인상의 소비자 심리가 중요하게 작용할 것으로 판단했다. 따라서, 코로나 발생 이후의 소비자 심리가 반영된 텍스트 데이터를 분석하여 새로운 상권 분류 모델을 구축하기로 했다. 더 나아가 온라인 상 텍스트 데이터로부터의 감성 분석 결과의 실효성 확인을 통해 현재의 상권분석 서비스를 발전시켜나갈 수 있는 방안을 마련하고자 한다.
</div>
</details>

<br>
<br>

**🍳 분석 요약**
![table](https://i.imgur.com/quetGZb.png)

<br>

**🍳 상권 분류 모델 구축 및 상권 분석**

서울시 우리마을가게 상권분석서비스에서 제공하는 상권 관련 8개의 속성 (상주인구, 생활인구, 아파트, 점포, 직장인구, 집객시설, 추정매출, 상권영역) 을 사용한 새로운 상권 분류

![area](https://i.imgur.com/JV7uk0m.png)

<br>

---

<br>


| 목차                                                         | 내용                                                   |
| ------------------------------------------------------------ | ------------------------------------------------------ |
| [1. 데이터 수집과 전처리](#1.-데이터-수집과-전처리)          | 정규화 및 표준화를 통한 데이터 스케일링                |
| [2. 새로운 상권 분류](#2.-새로운-상권-분류)                  | K 평균 군집 분석을 활용한 새로운 상권 분류             |
| [3. 상권 분류 비교 1](#3.-상권-분류-비교-1)                  | K 평균 군집 분석을 활용한 새로운 상권 분류             |
| [4. 감성 분석 기반의 상권 분류](#4.-감성-분석-기반의-상권-분류) | 상권 키워드별 리뷰의 감성 분석 결과를 추가한 상권 분류 |
| [5. 상권 분류 비교 2](#5.-상권-분류-비교-2)                  | 감성 분석 추가 여부에 따른 두 상권 분류의 비교         |
| [6. 결론](6.-결론)                                           | 감성 분석 기반의 상권 분석 모델 제시 및 실효성 확인    |


<br>
<br>



## 1. 데이터 수집과 전처리

**🍳 데이터 수집**


| 데이터 명                                                    | 출처                                                         | 활용                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 서울시 지역단위 '소득', '지출', '금융자산' 정보              | 2021 금융 데이터 경진대회 참가자 제공용 데이터(출처: 신한은행) | 코로나 전후 서울시 총소비금액 증감 분석                      |
| 지역 별 통계 데이터(기업)                                    | 2021 금융 데이터 경진대회 참가자 제공용 데이터(출처: 신한은행) | 코로나 전후 서울시 매출총액 및 영업이익총액 증감 분석        |
| 서울시 우리마을가게 상권분석서비스 상권 데이터 (상주인구, 생활인구, 아파트, 점포, 직장인구, 집객시설, 추정매출, 상권영역) | [서울시 열린 데이터 광장](https://data.seoul.go.kr/dataList/datasetList.do) | 클러스터링 분석을 위한 변수로 활용                           |
| 행정동 코드 데이터                                           | [행정안정부 주민등록 행정구역 코드](https://www.mois.go.kr/frt/bbs/type001/commonSelectBoardArticle.do?bbsId=BBSMSTR_000000000052&nttId=78739) | `상권_구분_코드_명`을 `행정동`으로 변환하여 감성 분석 대상 키워드로 사용 |
| 감성 분석용 데이터                                           | Naver sentiment movie corpus                                 | 감성 분석을 위한 Word2Vec 딥러닝 모델 학습용 데이터          |

<br>

**🍳 데이터 전처리**

<details>
<summary>서울시 지역단위 '소득', '지출', '금융자산' 정보</summary>
<div markdown="1">       
- 데이터 수집 경로 : [2021 금융 데이터 경진대회] 참가자 제공용 데이터 (출처: 신한은행)
- 데이터 설명 및 항목 : 보안 상 공개 불가
- 전처리 방법 : 보안 상 공개 불가
</div>
</details>

<details>
<summary>지역별통계데이터(기업)</summary>
<div markdown="1">       
- 데이터 수집 경로 : [2021 금융 데이터 경진대회] 참가자 제공용 데이터 (출처: 한국기업데이터)
- 데이터 설명 및 항목 : 보안 상 공개 불가
- 전처리 방법 : 보안 상 공개 불가
</div>
</details>

<details>
<summary>서울시 우리마을가게 상권분석서비스 상권 데이터</summary>
<div markdown="1">       
- 데이터 수집 경로 : 서울 열린 데이터 우리마을가게 상권분석서비스 데이터
- 데이터 설명 및 항목 : 2020년, 2021년의 `상권 구분 코드 명`, `상권 코드 명` 별 `생활 인구`, `아파트 단지 수`, `점포 수`, `직장 인구 수`, `시설 수`, `매출액`, `거주 인구 수` 사용
- 전처리방법: 각 항목을 동일한 scale로 맞추기 위한 전처리를 진행
    - 최소-최대 정규화 (Min-Max Normalization): 최소값과 최대값 사이로 정규화
    - 표준화 (Standardization): 정규화 값이 편향되는 문제를 막기 위해 z-score 표준화 진행
</div>
</details>


<br>
<br>

## 2. 새로운 상권 분류
> 서울특별시 우리 마을가게 상권분석 서비스에서 사용하고 있는 총 1,495개의 상권을 코로나 이후의 데이터 클러스터링으로 재분류

<br>

**🍳 K-Means Clustering**

K-평균 군집분석은 각 군집에 할당된 데이터와 군집의 중심점 사이의 평균 거리를 최소화하도록 중심점을 갱신하며 군집화하는 모델로, 군집 내 데이터의 분산을 최소화하는 방식으로 동작한다.
- 상권 분류는 집단에 대한 정보 없이 유사한 데이터를 계층 없이 묶는 작업이므로 K-평균 군집 분석을 활용했다.
- Elbow 기법과 Sulhouette 기법을 활용하여 최적 클러스터 개수 k를 설정하였다.

<br>

**🍳 K-Means Clustering 결과**

- 변수: 생활 인구, 아파트 단지 수, 점포 수, 직장 인구 수, 시설 수, 매출액, 거주 인구 수
- 총 4개의 클러스터

![](https://i.imgur.com/tDBCaic.png)

| ![](https://i.imgur.com/jzne24E.png) | ![](https://i.imgur.com/jectOWE.png) |
| ------------------------------------ | ------------------------------------ |

<br>

![](https://i.imgur.com/cfT9PBL.png)

| ![](https://i.imgur.com/h6KTWzT.png) | ![](https://i.imgur.com/CqW6MKI.png) |
| ------------------------------------ | ------------------------------------ |
| ![](https://i.imgur.com/Z6EYNAW.png) | ![](https://i.imgur.com/QrbCjWK.png) |


<br>
<br>

## 3. 상권 분류 비교 1

**🍳 노른자 상권 비교**
- 기존 상권 분류에서 발달상권 (83.72 %)을 주로 가지며 관광특구 (9.30%)의 특징을 약간 포함

**🍳 스크램블 상권**
- 기존 상권 분류에서 골목상권 (92.18 %)의 특징을 주로 포함

**🍳 오믈렛 상권**
- 기존 상권 분류에서 골목상권 (57.32%)과 전통시장 (22.40%), 발달상권(20.28%)의 특징을 두루 포함

**🍳 흰자 상권**
- 기존 상권 분류에서 골목상권 (98.08%)의 특징을 주로 포함

| ![](https://i.imgur.com/mC3RXYa.png) | ![](https://i.imgur.com/1yqKqhP.png) |
| ------------------------------------ | ------------------------------------ |
| ![](https://i.imgur.com/gOkMfHE.png) | ![](https://i.imgur.com/clFA1wk.png) |


<br>
<br>

## 4. 감성 분석 기반의 상권 분류
🍳 **Word2Vec**

[출처 - 한국어에 적합한 단어 임베딩 학습 방법](https://www.koreascience.or.kr/article/CFKO201612470014866.pdf)

감성 분석에 사용할 모델을 선정하기 위해 단어를 벡터화하는 임베딩(embedding) 방법론을 조사했다. Word2Vec, Glove, Fasttext가 대표적인 분석 모델로 확인됐으며, 어떤 모델이 한국어 감성분석에 더욱 적절한지를 확인하고자 관련 논문을 조사했다.

감성분석 한국어에 적합한 단어 임베딩 모델 및 파라미터 튜닝에 관한 연구1 논문을 보면 “한국어에 적합한 단어 임베딩 학습 방법은 CBOW 혹은 skip-gram 기반의 Word2Vec 모델(최상혁, 설진석, 이상구, 2016)” 이라는 결론을 내고 있다. 해당 논문을 참고하여 Word2Vec 모델이 한국어 감성분석에 적합하다고 판단하여 Word2Vec 모델을 사용하여 감성분석을 진행했다.
<details>
<summary>감성 분석 과정 확인하기</summary>
<div markdown="1">       
<br>1. 감성 분석을 위한 훈련 데이터로 '네이버 영화 리뷰 데이터'를 선정했다. 해당 데이터셋은 네이버 영화의 200,000개 리뷰(train: 15만, test: 5만)로 이루어져 있고, sentiment에 따라 0, 1로 lable화 되어있다. label화가 되어 있는 리뷰 데이터셋으로 긍정부정어 분석에 적합하다고 판단하여 선정했다. <br><br>
2. 훈련데이터로 사용하기 전 데이터를 정제하는 과정을 거쳤다. 데이터를 로드하여 중복 데이터 검사를 하여 중복 샘플을 제거했다. 이후 Null 값을 가진 데이터가 있는지를 추가로 확인해 Null 값을 가진 샘플을 제거하였다.<br><br>
3. 영어와 달리 한국어/한글 데이터에 대한 텍스트 분류를 위해서는 토큰화 시에 형태소 분석기를 사용한다. KoNLPy에서 제공하는 Okt 형태소 분석기를 사용하여 한국어 토큰화를 진행했다. 추가로 토큰화 과정에서 불용어도 제거해주는 것으로 전처리를 하였다.<br><br>
4. 기계가 텍스트를 숫자로 처리할 수 있도록 훈련 데이터에 정수 인코딩을 수행하였다. 빈 샘플들의 경우 추가로 제거하는 작업을 거쳤다. 마지막으로 서로 다른 길이의 샘플들의 길이를 동일하게 맞춰주는 패딩 작업을 진행하였다.<br><br>
5. 베딩 벡터의 차원은 100으로 정했고, 리뷰 분류를 위해서 LSTM을 사용했다. 에포크는 총 15번을 수행하여 인공지능을 훈련시킨 이후에 테스트 데이터를 통해 긍정부정어 분류의 정확도를 확인하였다.<br><br>
6. 학습시킨 모델을 기반으로 분석의 대상이 될 데이터셋을 마련하기 위해 크롤링을 실시했다. 먼저 상권 검색을 위해 행정안전부의 행정동 코드를 활용해 행정동을 추출하였다. 해당 키워드를 활용해 네이버 블로그 검색 API를 이용하여 1,000개씩 크롤링을 한 뒤 csv 파일로 저장하였다.<br><br>
7. 크롤링한 데이터의 감성 분석을 실시해, 전체 리뷰 개수 대비 긍정 리뷰 개수 비율을 추출했다. 추출한 긍정 리뷰 개수 비율은 새로운 상권 분류 모델을 개발하는 데 변수로 활용하였다.
</div>
</details>

<br>

**🍳 K-Means Clustering 결과**

- 변수: 생활 인구, 아파트 단지 수, 점포 수, 직장 인구 수, 시설 수, 매출액, 거주 인구 수 + 긍정 리뷰 비율
- 총 4개의 클러스터

<br>
<br>

## 5. 상권 분류 비교 2


**🍳 흰자 상권 비교**

- 기존 상권 분류의 골목 상권 특징 약 96% 포함
- 감성 분석 결과를 추가 시 약 57.77% 재분류
    - 스크램블 상권 → 흰자 상권

<br>

![](https://i.imgur.com/zucWuZ8.png)

<br>

**🍳 스크램블 상권 비교**

- 기존 상권 분류의 골목 상권 특징 약 89% 포함
- 감성 분석 결과를 추가 시 약 5.75% 재분류
    - 오믈렛 상권 → 스크램블 상권 (약 1% 이하)
    - 노른자 상권 → 스크램블 상권 (약 1% 이하)
    - 흰자 상권 → 스크램블 상권 약 (4.67%)
- 상권의 큰 변동은 없었지만 재분류가 존재하므로 감성 분석이 유효하다고 볼 수 있다

<br>

![](https://i.imgur.com/DXtmf6v.png)

<br>

**🍳 노른자 상권 비교**

- 기존 분류의 발달 상권 특징 약 84% 포함 
- 감성 분석 결과 추가 시 약 4.55% 재분류
    - 오믈렛 상권 → 노른자 상권
- 감성 분석 결과가 긍정적인 영향을 주었다고 유추해볼 수 있다.
- 상권의 큰 변동은 없었지만 재분류가 존재하므로 감성 분석의 의미가 어느 정도는 있다고 볼 수 있다.

<br>

![](https://i.imgur.com/ex6hjH8.png)

<br>

**🍳 오믈렛 상권 비교**

- 기존 상권 분류에서 골목 상권 특징을 크게 가지면서도 전통 시장과 발달 상권의 특징도 가진다.
- 감성 분석 결과를 추가 시 약 76.16% 재분류 
    - 스크램블 상권 → 오믈렛 상권

<br>

![](https://i.imgur.com/UpXMoiF.png)

<br>
<br>

## 5. 결론

**🍳 K 평균 군집분석을 이용한 새로운 상권 도출**
> `변수` : 총 생활인구수, 총 직장 인구수, 총 상주인구수, 아파트 단지 수, 학교수, 집객시설수, 교통시설수

- 노른자 상권 : 인구와 시설 밀집지역이자 번화가
- 스크램블 상권 : 인구와 시설이 분산되어 상권의 영향력이 상대적으로 약한 지역
- 오믈렛 상권 : 교육시설과 학교 중심의 지역
- 흰자 상권 : 압도적으로 주거 기능이 강한 지역

위의 변수에 ‘감성분석으로 도출한 긍정 리뷰 비율’을 추가하고 클러스터링을 다시 한 번 하고, 온라인 리뷰를 기반으로 한 감성 분석 결과를 바탕으로 상권 모델 분석을 진행했다.

<br>

**🍳 감성분석을 더한 상권의 실효성 확인**

- 전체 4개의 상권 분류 중 흰자 상권과 오믈렛 상권에서 큰 비율의 재분류가 일어났고 나머지 노른자 상권과 스크램블 상권에서도 약간의 재분류가 있었다. 
- 상권 분류 모델 구축에 있어서 리뷰 데이터 이외에도 소비자의 평판이나 인식을 반영하는 다양한 비정형 데이터의 활용은 유의미하다고 볼 수 있다.

<br>

---

<br>

**🍳 업무 분배**

| BGJ                                          | 업무                                                       |
| -------------------------------------------- | ---------------------------------------------------------- |
| [Bright 채은](https://github.com/chenni0531) | 서울시 상권 관련 데이터 전처리, 상권 분류 클러스터링       |
| [Gleaming 수빈](https://github.com/axxsxbxx) | 소비 데이터 전처리, 상권 별 키워드로 리뷰 크롤링           |
| [Jewel 현욱](https://github.com/hyeonuk27)   | 매출총액 및 영업이익 데이터 전처리, 긍정어 부정어 감성분석 |

<br>


**🍳 프로젝트를 마치며**

| BGJ                                          | 한 마디                                                      |
| -------------------------------------------- | ------------------------------------------------------------ |
| [Bright 채은](https://github.com/chenni0531) | 중간중간 어려움이 있었지만 팀원들과 함께 이겨내고 보고서까지 완료해내어 뿌듯했다. 실생활의 데이터를 통해 새로운 의미를 찾는 과정이 즐거웠다:) 이번 기회를 발판 삼아서 다음 프로젝트에서는 더욱 깊이있게 분석 기술을 배우고 적용해보고 싶다. |
| [Gleaming 수빈](https://github.com/axxsxbxx) | 처음으로 참가하는 공모전이어서 걱정도 많고 막혔던 부분도 많았다. 팀원들과 함께 공부하고 도와가면서 무사히 마칠 수 있어서 뿌듯하다. 다음 공모전 떄는 좀 더 깊은 이해도를 가지고 진행할 수 있을 것 같다:) |
| [Jewel 현욱](https://github.com/hyeonuk27)   | 데이터 분석을 처음 해보면서 어려움이 많았지만, 팀원들의 도움으로 분석 목표를 이뤄낼 수 있었다. 데이터 분석을 해보고 싶었는데, 이번 기회로 많이 배우고 성장한 것 같아 뿌듯하다. 이후에도 스터디를 통해서 데이터 분석 지식과 경험을 계속해서 쌓아가며 데이터 분석 실력을 다질 계획이다! 화이팅! |

<br>

---

![](https://i.imgur.com/vLaACTI.png)