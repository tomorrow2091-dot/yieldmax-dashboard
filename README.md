# YieldMax ETF 자동 배당 대시보드

**매주 자동 업데이트되는 일드맥스 ETF 배당 정보 대시보드**

![Dashboard Preview](https://via.placeholder.com/800x400/1a1a2e/ffffff?text=YieldMax+ETF+Dashboard)

## 🚀 주요 기능

- **자동 데이터 수집**: 매주 금요일 오후 6시(KST) 자동 업데이트
- **실시간 대시보드**: YieldMax ETF 배당 정보 실시간 표시
- **스마트 검색 & 정렬**: 심볼, 배당률, SEC Yield 등 다양한 기준으로 정렬
- **반응형 디자인**: PC, 태블릿, 모바일 모든 기기 지원
- **GitHub Pages 자동 배포**: 코드 변경시 자동으로 웹사이트 업데이트

## 🟢 라이브 데모

🌐 **웹사이트**: https://tomorrow2091-dot.github.io/yieldmax-dashboard

## 📋 설정 가이드

### 1단계: 리포지토리 생성

```bash
# 새 리포지토리 생성
git clone https://github.com/tomorrow2091-dot/yieldmax-dashboard.git
cd yieldmax-dashboard

# 또는 기존 리포지토리에 파일 추가
mkdir yieldmax-dashboard
cd yieldmax-dashboard
git init
```

### 2단계: 필수 파일 추가

프로젝트에 다음 파일들이 이미 포함되어 있습니다:

```
yieldmax-dashboard/
├── .github/workflows/
│   └── update-etf-data.yml     # GitHub Actions 워크플로우
├── data/
│   └── etf_data.json          # ETF 데이터 (자동 생성)
├── index.html                 # 메인 대시보드 페이지
├── yieldmax_scraper.py        # Python 스크래퍼
├── requirements.txt           # Python 의존성
└── README.md                  # 이 파일
```

### 3단계: GitHub Pages 활성화

1. GitHub 리포지토리 → **Settings** 탭
2. 왼쪽 메뉴에서 **Pages** 클릭
3. Source를 **GitHub Actions**로 설정
4. **Save** 클릭

### 4단계: Actions 권한 설정

1. GitHub 리포지토리 → **Settings** → **Actions** → **General**
2. **Workflow permissions**에서 다음 설정:
   - ✅ **Read and write permissions** 선택
   - ✅ **Allow GitHub Actions to create and approve pull requests** 체크

### 5단계: 자동 업데이트 테스트

리포지토리에서 Actions 탭으로 이동하여 수동으로 실행:

1. **Actions** → **Update YieldMax ETF Data** 선택
2. **Run workflow** → **Run workflow** 클릭

## 🤖 자동화 상세 설정

### 스케줄 수정

기본적으로 **매주 금요일 오후 6시(KST)**에 실행됩니다. 변경하려면:

`.github/workflows/update-etf-data.yml` 파일의 cron 표현식 수정:

```yaml
schedule:
  # 매일 오후 3시 (KST) = 오전 6시 (UTC)
  - cron: '0 6 * * *'

  # 매주 월요일 오전 9시 (KST) = 일요일 자정 (UTC)
  - cron: '0 0 * * 1'
```

### 수동 실행

1. GitHub 리포지토리 → **Actions** 탭
2. **Update YieldMax ETF Data** 워크플로우 선택
3. **Run workflow** → **Run workflow** 클릭

### 데이터 소스 커스터마이징

`yieldmax_scraper.py` 파일에서 데이터 수집 로직을 수정할 수 있습니다:

```python
# 추가 ETF 심볼
sample_etfs = [
    "TSLY", "NVDY", "MSTY", "APLY", "GOOY", 
    "AMZY", "NFLY", "CONY", "OARK", "PYPY",
    # 여기에 새 ETF 추가
    "NEWY", "CUSTM"
]

# 다른 데이터 소스 추가
def scrape_additional_source(self):
    # 커스텀 스크래핑 로직
    pass
```

## 📊 API 연동 (선택사항)

더 정확한 데이터를 위해 금융 API를 연동할 수 있습니다:

### Alpha Vantage API 사용

```python
# yieldmax_scraper.py에 추가
import os

API_KEY = os.environ.get('ALPHA_VANTAGE_API_KEY')

def get_dividend_data(symbol):
    url = f"https://www.alphavantage.co/query?function=OVERVIEW&symbol={symbol}&apikey={API_KEY}"
    response = requests.get(url)
    return response.json()
```

GitHub Secrets에 API 키 추가:
1. Repository → **Settings** → **Secrets and variables** → **Actions**
2. **New repository secret** 클릭
3. Name: `ALPHA_VANTAGE_API_KEY`, Value: `your_api_key`

## 🎨 대시보드 커스텀마이징

### 색상 테마 변경

`index.html`의 CSS 변수를 수정하세요:

```css
:root {
  --primary-color: #3b82f6;    /* 파란색 → 원하는 색상 */
  --secondary-color: #1e3a8a;  /* 진한 파란색 */
  --background-color: #0f0f23; /* 배경색 */
}
```

### 새로운 필터 추가

```javascript
// 커스텀 필터 함수
function filterByROC(threshold) {
    return etfData.filter(etf => etf.roc >= threshold);
}
```

## 🔧 트러블슈팅

### 일반적인 문제들

**Q: Actions가 실행되지 않습니다**
- Settings → Actions에서 권한 확인
- 워크플로우 파일의 YAML 문법 확인
- Repository가 private인 경우 GitHub Pro 계정 필요

**Q: 데이터가 업데이트되지 않습니다**
- Actions 탭에서 실행 로그 확인
- `yieldmax_scraper.py`의 오류 메시지 확인
- API 제한이나 웹사이트 구조 변경 가능성

**Q: 웹사이트가 표시되지 않습니다**
- GitHub Pages 설정 확인 (Settings → Pages)
- `index.html` 파일이 루트 디렉토리에 있는지 확인
- 브라우저 캐시 삭제 후 재시도

### 로그 확인 방법

1. GitHub → **Actions** 탭
2. 최근 실행된 워크플로우 클릭
3. **update-data** job 클릭
4. 각 step의 로그 확인

## 📈 고급 기능

### 알림 설정

슬랙이나 디스코드로 업데이트 알림 받기:

```yaml
# .github/workflows/update-etf-data.yml에 추가
- name: Send Slack notification
  uses: 8398a7/action-slack@v3
  with:
    status: ${{ job.status }}
    channel: '#etf-updates'
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### 데이터 백업

```yaml
# 데이터 백업 step 추가
- name: Backup data
  run: |
    cp data/etf_data.json backups/etf_data_$(date +%Y%m%d).json
    git add backups/
    git commit -m "Backup data $(date)"
```

## 📞 지원

문제가 발생하면 다음 방법으로 해결하세요:

1. **Issues 탭**에서 유사한 문제 검색
2. 새로운 이슈 생성시 다음 정보 포함:
   - 오류 메시지
   - Actions 로그 스크린샷
   - 브라우저 및 운영체제 정보

## 📝 라이선스

MIT License - 자유롭게 사용, 수정, 배포 가능합니다.

---

**🎯 추가 기능 제안이나 버그 리포트는 Issues 탭을 이용해 주세요!**