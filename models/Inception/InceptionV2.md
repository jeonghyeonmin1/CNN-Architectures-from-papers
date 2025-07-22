### InceptionV2

![1](./images/1.png)

---

#### 🔷 Factorizing Convolutions with Large Filter Size

---

#### 1. Factorization into Smaller Convolutions

- `5x5` 컨볼루션은 넓은 영역을 보지만, **계산량이 매우 큼**
- 이를 `3x3 → 3x3` 두 개로 나누면:
  - 같은 receptive field를 가지면서도
  - **파라미터 수 약 28% 감소**
  - **ReLU 비선형성도 한 번 더 적용**되므로 **모델 표현력 향상**
- 결과적으로 연산 효율성과 성능을 동시에 개선함

> ✅ **핵심**: `5x5 → 3x3 + 3x3` 구조는 더 적은 계산량으로 더 깊은 표현 가능

![2](./images/2.png)

---

#### 2. Spatial Factorization into Asymmetric Convolutions

- `3x3` 컨볼루션도 더 나눌 수 있을까?
- `3x3 → 3x1 → 1x3`으로 분해하는 **비대칭 커널(asymmetric convolution)** 사용
  - 같은 receptive field를 유지하면서
  - **연산량을 더 줄일 수 있음**
- 단, **입력 초반에는 spatial 정보를 충분히 활용해야 하므로 부적합**
- 대신, **feature가 추상화된 중간~후반부 layer**에서는 효과적임

> ✅ **핵심**: `3x3 → 3x1 + 1x3`은 계산 효율성과 학습 안정성을 높이는 전략 (단, 위치 주의)

![3](./images/3.png)

---

#### 🔧 Utility of Auxiliary Classifiers

- GoogLeNet(InceptionV1)에서는 **gradient vanishing** 문제 해결을 위해 auxiliary classifier 사용
- 그러나 실험 결과:
  - **초반부 auxiliary classifier는 학습에 큰 도움이 되지 않음**
  - → **InceptionV2에서는 초반부 auxiliary 제거**
- 대신, **후반부 auxiliary classifier는 regularization 역할**을 함
  - Dropout, BatchNorm을 넣었더니 → **메인 분류기의 성능이 향상**
  - → 보조 분류기가 **overfitting 방지 및 일반화 향상**에 기여

> ✅ **핵심**: 보조 분류기는 이제 gradient 보조 장치가 아닌 **정규화 장치로 해석**


---

#### 📐 Efficient Grid Size Reduction

- InceptionV1에서는 `MaxPooling → Inception` 순으로 처리
  - 이 구조는 **representational bottleneck**을 일으킴
  - → pooling으로 정보 손실 후에 특징 추출이 이뤄지기 때문
- InceptionV2에서는 이를 **병렬적으로 처리**
  - `Inception branch`와 `stride가 있는 convolution branch`로 나눠
  - 동시에 처리하여 **정보 손실을 줄이고 계산 효율도 향상**

> ✅ **핵심**: Grid size reduction을 **병렬 구조로 개선** → 정보 손실과 계산 낭비를 동시에 해결

![4](./images/4.png)