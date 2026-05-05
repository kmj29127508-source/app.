# 🌿 원예장비 제조업체 총괄생산계획 시스템

> 스마트제조 중간과제 | Chunghun Ha 교수님 강의록 기반  
> Pyomo LP/IP 최적화 · GLPK Solver · Streamlit 대시보드

---

## 📋 과제 개요

- **과목**: 스마트제조
- **주제**: 총괄생산계획(Aggregate Production Planning) 웹앱 개발
- **기능**: 수요/파라미터 변경 시 실시간 재최적화 + 다양한 시각화 대시보드

---

## 🚀 로컬 실행 방법

### 1. 환경 설정
```bash
# GLPK 설치 (Ubuntu/Debian)
sudo apt-get install glpk-utils

# Python 패키지 설치
pip install -r requirements.txt
```

### 2. 앱 실행
```bash
streamlit run app.py
```

브라우저에서 `http://localhost:8501` 접속

---

## ☁️ Streamlit Cloud 배포 방법

1. GitHub 저장소에 아래 파일 업로드:
   - `app.py`
   - `requirements.txt`
   - `packages.txt`

2. [share.streamlit.io](https://share.streamlit.io) 접속 → New app

3. 저장소 선택 → `app.py` 지정 → **Deploy!**

> `packages.txt`에 `glpk-utils`가 있으면 Streamlit Cloud가 자동으로 GLPK 설치

---

## 📐 모델 설명 (강의록 기반)

### 결정변수
| 변수 | 의미 | 단위 |
|------|------|------|
| Wₜ | t월 종업원 수 | 인/월 |
| Hₜ | t월 고용 인원 | 인/월 |
| Lₜ | t월 해고 인원 | 인/월 |
| Pₜ | t월 생산량 | ea/월 |
| Iₜ | t월 말 재고 | ea |
| Sₜ | t월 말 부족재고 | ea |
| Cₜ | t월 하청 수량 | ea/월 |
| Oₜ | t월 초과근무 총시간 | hr/월 |

### 목적함수
```
Z = Σ(640·Wt + 6·Ot + 300·Ht + 500·Lt + 2·It + 5·St + 10·Pt + 30·Ct)
  = 정규노동비 + 초과근무비 + 고용비 + 해고비 + 재고유지비 + 부족재고비 + 재료비 + 하청비
```

### 제약조건
1. **노동력**: Wt = W(t-1) + Ht - Lt  
2. **생산능력**: Pt ≤ 40·Wt + Ot/4  
3. **재고균형**: It = I(t-1) + Pt + Ct - Dt - S(t-1) + St  
4. **초과근무**: Ot ≤ 10·Wt  
5. **초기조건**: W₀=80, I₀=1000, S₀=0  
6. **최종조건**: I₆≥500, S₆=0  

---

## 🎨 대시보드 구성

| 탭 | 내용 |
|----|------|
| 📊 생산계획 결과표 | 최적화 결과 + 월별 비용 상세표 |
| 📈 수요·생산·재고 | 수요vs생산 바차트, 재고추이, 누적비교, 초과근무 |
| 👷 인력 계획 | 작업자수 변화, 고용/해고 현황, 생산능력 활용률 |
| 💰 비용 분석 | 파이차트, 스택바, 워터폴 수익분석 |
| 🔍 적절성 평가 | 종합점수 게이지, 제약충족 체크, 감도분석 |

---

## 📁 파일 구조
```
├── app.py           # Streamlit 메인 앱
├── requirements.txt # Python 패키지
├── packages.txt     # apt 패키지 (GLPK)
└── README.md        # 설명서
```
