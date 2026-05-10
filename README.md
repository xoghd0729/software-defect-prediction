# software-defect-prediction

> 소프트웨어 결함 탐지 머신러닝 프로젝트 — Kaggle 데이터셋 기반 결함 분류 모델

소프트웨어 모듈의 코드 메트릭을 입력으로 받아 결함 여부를 예측하는 머신러닝 분류 모델을 구현한 프로젝트입니다.

---

## 사용 기술

- Python 3 / Jupyter Notebook
- pandas, numpy
- matplotlib, seaborn, plotly
- scikit-learn (Linear Regression)
- Kaggle Dataset

---

## 데이터셋

**출처**: [Kaggle - Software Defect Prediction](https://www.kaggle.com/datasets/semustafacevik/software-defect-prediction)

| 피처 | 설명 |
|------|------|
| `loc` | 모듈 라인 수 |
| `v(g)` | 순환 복잡도 (McCabe) |
| `branchCount` | 분기 수 |
| `b` | Halstead Volume |
| `defects` | **타깃**: 결함 여부 (True / False) |

---

## 분석 과정

1. 결함 클래스 분포 시각화 (Histogram)
2. 변수 간 상관관계 분석 (Heatmap)
3. Volume-Bug 산점도로 주요 피처 확인
4. 선형 회귀 모델 학습 및 결과 분석

---

## 배운 점

단순히 `model.fit()` 한 줄을 실행하는 것보다 **데이터를 이해하는 과정**이 훨씬 중요하다는 것을 배웠습니다. 상관관계 히트맵을 그려보면서 `v(g)`(순환 복잡도)와 `branchCount`(분기 수)가 결함과 높은 상관관계를 보인다는 인사이트를 얻었고, 복잡한 코드일수록 결함이 많다는 소프트웨어 공학적 원칙을 데이터로 확인하는 경험이었습니다.

---

## 어려웠던 점

결함 데이터는 `False`(정상)가 `True`(결함)보다 훨씬 많은 **클래스 불균형** 문제가 있었습니다. 당시에는 이를 인식하지 못하고 모델을 학습시켰지만, 나중에 정확도(Accuracy)가 높게 나와도 실제로 결함을 제대로 탐지하지 못하는 경우가 있다는 것을 알게 됐습니다. 불균형 데이터 처리가 분류 문제에서 매우 중요한 과제임을 배웠습니다.

또한 결함 여부는 True/False의 이진 분류 문제인데, 이에 선형 회귀를 적용한 것은 적절하지 않았습니다. 로지스틱 회귀나 분류 전용 모델을 먼저 고려했어야 했는데, 당시에는 알고리즘 선택 기준을 잘 몰랐습니다.

---

## 현재 문제점

- **잘못된 알고리즘 선택**: 결함 여부(True/False)는 이진 분류 문제인데 선형 회귀(Linear Regression)를 적용했습니다. 선형 회귀는 연속적인 값을 예측하는 알고리즘으로, 이진 분류에 적합하지 않습니다. 로지스틱 회귀(Logistic Regression)가 최소한의 기준선이 됐어야 합니다.
- **클래스 불균형 미처리**: 결함 없는 샘플(False)이 결함 있는 샘플(True)보다 훨씬 많을 가능성이 높습니다. 이 불균형을 처리하지 않으면 모델이 "항상 False" 예측만 해도 높은 정확도가 나오는 문제가 생깁니다.
- **단일 평가 지표**: Accuracy만 사용했는데, 불균형 데이터에서 Accuracy는 의미 없는 지표일 수 있습니다. Precision·Recall·F1·AUC-ROC를 함께 측정해야 합니다.
- **재현성 미보장**: 랜덤 시드가 고정되어 있지 않아 실행할 때마다 결과가 달라질 수 있습니다.

## 고민해야 할 점

- **분류 vs 회귀 선택 기준**: 어떤 경우에 분류 알고리즘을, 어떤 경우에 회귀 알고리즘을 써야 하는지 명확한 기준이 없었습니다. 타깃 변수가 범주형(True/False)이면 무조건 분류, 연속형이면 회귀라는 단순한 원칙부터 확실히 익혀야 합니다.
- **Feature 선택 방법**: 14개 피처 중 어떤 것이 결함 예측에 실제로 유효한지 검증하지 않았습니다. 상관관계 높은 피처끼리 다중공선성 문제는 없는지, Feature Importance로 중요 변수를 선별해야 하는지 고민이 필요합니다.
- **데이터 전처리 수준**: 결측치 처리, 이상치 제거, 스케일링을 얼마나 엄밀하게 해야 하는지 기준을 세우고 싶습니다. 특히 소프트웨어 메트릭 데이터의 특성상 이상치가 많을 수 있는데, 제거해야 하는지 유지해야 하는지 판단이 어려웠습니다.

## 아쉬운 점 및 개선 방향

- 선형 회귀 하나만 사용했는데, Logistic Regression, RandomForest, XGBoost 등 여러 모델을 비교해 성능 차이를 분석하고 싶습니다.
- 클래스 불균형 문제를 해결하기 위해 SMOTE(오버샘플링) 또는 가중치 조정 기법을 적용하면 결함 탐지 성능을 높일 수 있을 것 같습니다.
- 정확도(Accuracy) 외에도 Precision, Recall, F1-Score 등 분류 문제에 적합한 평가 지표를 함께 측정해야 한다는 것을 이후에 알았습니다.

---

## 추가로 공부해야 할 내용

- 분류(Classification) vs 회귀(Regression) 문제 구분 기준
- 클래스 불균형 처리 (SMOTE, Class Weight)
- 모델 평가 지표 (Precision, Recall, F1, AUC-ROC)
- Feature Engineering 및 Feature Selection
- RandomForest, XGBoost, LightGBM
