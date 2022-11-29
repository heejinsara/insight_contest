# insight_contest

## 개요

1. 신용카드 사용자 데이터로 사용자의 대금 연체 정도를 예측
2. 금융업계에게 제안할 수 있는 의미있는 모델링 결과와 인사이트 도출

## 개발 환경

- 라이브러리 및 프레임워크 : Numpy, Pandas, Sklearn, Seaborn
- 개발 도구 : Jupyter Notebook, Colab
- 협력 도구 : Notion, Wandb

## Data

 **train.csv**

- train 데이터 : 신용카드 사용자들의 개인 신상정보
- credit 열 포함
- train.shape : (26457, 20)

**test.csv**

- test 데이터 : 신용카드 사용자들의 개인 신상정보
- credit 열 미포함
- test.shape : (10000, 19)

## Model

XGBoost in sklearn

## Metric

Accuracy Score

## Results

0.7273으로 🥇1st prize🥇

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8db7ec33-9541-45ea-93d9-c3f6af1ec41e/Untitled.png)

## Details

전처리를 통해 다양한 Derived Variable 발굴

- 경제력 = 소득/(살아온 일수 + 근무일수)
    - 신용카드 사용자가 시간당 벌 수 있는 소득을 표현
- 경제활동 시작 나이 = 나이-근무일수
    - 신용카드 사용자가 경제활동을 시작한 나이를 표현
- 부양해야할 가족수 = 가족수 + 자녀수
    - 가족수와 자녀수의 correlation이 상당히 높아 합산

⇒ 전처리 전과 비교하여 accuracy 5%p 상승

XGBoost의 hyperparameter tuning에 wandb 사용

1. Tree 모형에 필요한 hyperparameter를 폭넓게 테스트
2. 성능에 유의미한 변화를 주는 hyperparamer를 선택하여 유의미한 값 주변의 값을 좁게 테스트
→ 학습의 안정성 확보
3. 복잡한 트리모델에 규제를 적용한 모델과 단순한 트리모델에 규제를 적용하지 않은 모델 비교

⇒ K-fold validation을 사용하지 않을 때, 각각의 eteration마다 train dataset의 비율을 조정하여 일부로만 학습시키는 `subsample` hyperparameter가 가장 큰 요인으로 작용했다.

⇒ Tabular 형식의 데이터에서 Tree model은 트리를 깊고 넓게 가져가고 규제를 적용하는 것보다, 간단한 트리모델을 사용하고 규제를 약하게 적용하는 것이 효과가 좋았다.

⇒ hyperparameter tuning 이후 accuracy 0.5%p 상승
