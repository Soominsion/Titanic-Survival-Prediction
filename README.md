
# 🛳 Titanic Survival Prediction - GDGoC Final Submission

## 📌 프로젝트 개요
본 프로젝트는 타이타닉 탑승자의 정보를 바탕으로 생존 여부를 예측하는 이진 분류 문제입니다.  
탐색적 데이터 분석(EDA)과 다양한 전처리 조합을 실험한 뒤, 세 가지 모델(Logistic Regression, Random Forest, XGBoost)을 비교하였습니다.  
검증 정확도 기준 가장 성능이 우수한 모델을 최종 제출 모델로 선택하였습니다.

---

## 🔧 전처리 과정 요약
다음과 같은 전처리 조합들을 실험했습니다:

- **결측치 처리**  
  - `Age`: Pclass와 Sex 기준 그룹 평균 대체  
  - `Fare`: Pclass 기준 평균값으로 대체  
  - `Embarked`: 최빈값('S')으로 대체  

- **파생 변수 생성**  
  - `FamilySize = SibSp + Parch + 1`  
  - `AgeGroup`: 나이를 카테고리 (Child, Teen, Young, Adult, Senior)로 변환

- **열 제거**: PassengerId, Name, Ticket, Cabin, SibSp, Parch 등  
- **범주형 인코딩**: Sex, Embarked → Label Encoding  
- **이상치 제거**: Fare 열 기준으로 IQR 방식 적용 (일부 조합에서만)

---

## 🧪 실험 조합 결과 (4가지 전처리 × 3개 모델 = 12개 실험)

| 조합 | AgeGroup | 이상치 제거 | Logistic Regression | Random Forest | XGBoost |
|------|----------|--------------|----------------------|----------------|----------|
| ①    | ✅       | ✅           | 0.7806               | 0.7613         | 0.8129   |
| ②    | ✅       | ❌           | 0.8156               | 0.8212         | 0.7989   |
| ③    | ❌       | ✅           | 0.7613               | 0.7677         | 0.7935   |
| ④    | ❌       | ❌           | 0.8212               | **0.8436**     | 0.7877   |

---

## 🏆 최종 제출 모델
- **모델**: `RandomForestClassifier()` (기본 설정)
- **전처리 조합**: AgeGroup 변환 ❌, 이상치 제거 ❌
- **성능 요약**:
  - Accuracy: **0.8436**
  - f1-score: 클래스 0 → 0.87 / 클래스 1 → 0.81 (균형 양호)

---

## ⚙️ 하이퍼파라미터 튜닝 결과 (GridSearchCV)
```python
Best Parameters:
{
  'n_estimators': 200,
  'min_samples_split': 2,
  'min_samples_leaf': 2,
  'max_features': 'sqrt',
  'max_depth': None
}
Best Accuracy: 0.8272
```
➡️ 하지만 **기본 모델보다 성능이 낮아 채택하지 않음**

---

## 📊 전처리 vs 모델 선택: 무엇이 더 중요한가?

이번 실험에서 두 가지 성능 차이를 정량적으로 비교해보았습니다.

| 구분                        | 최대 성능 차이 |
|-----------------------------|----------------|
| 전처리 조합 변경 (Random Forest 기준) | **+8.23%p** |
| 모델 변경 (동일 전처리 조합 내)       | **+5.59%p** |

### ✅ 결론:
> **이번 실험에서는 모델 선택보다 전처리 방식이 성능에 더 큰 영향을 주었습니다.**
- 특히 Random Forest에서는 전처리를 거의 하지 않았을 때 가장 우수한 성능을 기록하였습니다.
- 반면 XGBoost는 전처리를 더 했을 때 성능이 좋았으며, 전처리에 민감하게 반응하는 경향을 보였습니다.

---

## 📂 참고 사항
- 실험은 Google Colab에서 실행되었으며, GCP 기반 환경에서 데이터와 모델이 관리되었습니다.
- 분석 결과는 Markdown 및 발표용 슬라이드로도 정리되어 있습니다.
