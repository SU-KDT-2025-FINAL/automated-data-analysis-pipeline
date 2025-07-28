# 🔄 OCR 기반 데이터 분석 자동화 파이프라인-gpt 4o

## 📌 개요

OCR(Optical Character Recognition, 광학 문자 인식)은 **이미지 또는 스캔된 문서에서 텍스트 데이터를 자동으로 추출하는 기술**입니다.  
반복적인 데이터 분석 작업에서 OCR은 **비정형 문서 → 정형 데이터로의 전환**을 가능하게 하며, 자동화 파이프라인의 핵심 구성요소로 활용됩니다.

---

## 🏗️ 전체 파이프라인 구조

```
[1] 데이터 수집
  ↓
[2] OCR 텍스트 추출
  ↓
[3] 텍스트 정제 및 구조화
  ↓
[4] 데이터 저장
  ↓
[5] 분석 및 시각화
```

---

## 1️⃣ 데이터 수집

- 대상: 스캔된 문서, 이미지(PDF, JPG, PNG 등), 사진, 팩스
- 수집 방법:
  - 디렉터리 감시 (inotify, watchdog)
  - 이메일/FTP 자동 수신
  - 사용자 업로드
  - API 연동

---

## 2️⃣ OCR 텍스트 추출

- 사용 도구:
  - **Tesseract OCR** (Java 또는 Python)
  - **EasyOCR**, **PaddleOCR**, **Google Vision API** (Python)
- 기능:
  - 이미지 내 문자 인식
  - 블록 단위 인식 (행, 열, 문단)
  - 언어별 지원 (예: 한국어, 영어, 숫자 등)

### ✅ Python 예시 (EasyOCR)

```python
import easyocr

reader = easyocr.Reader(['ko', 'en'])
results = reader.readtext('sample_invoice.jpg')

for _, text, _ in results:
    print(text)
```

---

## 3️⃣ 텍스트 정제 및 구조화

- **불필요한 정보 제거** (예: 특수문자, 공백)
- **패턴 인식 및 분리**
  - 정규표현식 활용 (예: 날짜, 금액, 코드)
- **구조화**
  - Pandas DataFrame, JSON, CSV 등으로 정렬

### ✅ 예시

```python
import re

text = "날짜: 2025-07-28 금액: 12,000원 항목: 점심"
match = re.search(r'(\d{4}-\d{2}-\d{2}).*?(\d{1,3}(,\d{3})*).*?항목: (.+)', text)
if match:
    date, price, _, item = match.groups()
    print(date, price, item)
```

---

## 4️⃣ 데이터 저장

- 저장 대상:
  - **CSV 파일**
  - **Database (MySQL, PostgreSQL, MongoDB)**
  - **클라우드 스토리지 (S3, Google Drive)**
- 저장 방식:
  - 자동 배치 처리
  - REST API 전송
  - Airflow, Prefect 등 워크플로우 관리

---

## 5️⃣ 분석 및 시각화

- 분석 도구:
  - Pandas, NumPy
  - Scikit-learn, XGBoost 등 (머신러닝)
- 시각화:
  - Matplotlib, Seaborn, Plotly
  - 대시보드: Streamlit, Dash, Power BI, Tableau

### ✅ 예시: 항목별 소비 금액 분석

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv('extracted_data.csv')
df.groupby('항목')['금액'].sum().plot(kind='bar')
plt.title('항목별 소비 금액')
plt.show()
```

---

## ⚙️ 자동화 예시 시나리오

| 단계           | 도구               | 설명                                |
|----------------|--------------------|-------------------------------------|
| 데이터 수집    | Watchdog (Python)  | 폴더 내 파일 감지                   |
| OCR 처리       | EasyOCR, Tesseract | 이미지 내 텍스트 추출               |
| 정제/구조화    | Pandas, 정규표현식 | 필요한 데이터 추출 및 정렬         |
| 저장           | CSV, DB            | 파일 또는 DB에 자동 저장           |
| 분석 및 시각화 | Pandas, 시각화 도구| 주기적인 분석 리포트 생성          |

---

## ✅ OCR 도입의 이점

- 🔁 **반복 업무 자동화** → 수작업 제거
- 🔎 **정확한 데이터 수집** → 분석 신뢰도 향상
- ⏱ **시간 단축** → 실시간 또는 배치 처리 가능
- 📦 **정형화 어려운 문서 처리** 가능 → 다양한 양식 대응

---

## ⚠️ OCR 활용 시 주의점

| 항목         | 내용                                               |
|--------------|----------------------------------------------------|
| 이미지 품질  | 흐림, 왜곡, 조명에 민감함                         |
| 언어 지원    | 한글 정확도는 모델에 따라 차이 있음               |
| 폰트/서식    | 복잡한 레이아웃 또는 글씨체는 오류 가능성 높음   |
| 개인정보     | OCR 결과에 민감 정보 포함 시 보안 조치 필요       |

---

## 📌 요약

> OCR은 반복적인 문서 기반 데이터 처리에서  
> **비정형 데이터를 정형화**하고,  
> **분석 자동화 파이프라인을 구현하는 핵심 기술**입니다.  
> 특히 Python 환경에서는 강력하고 빠르게 구현 가능하며,  
> 전체 파이프라인을 연계하면 실시간 분석 시스템 구축도 가능합니다.
