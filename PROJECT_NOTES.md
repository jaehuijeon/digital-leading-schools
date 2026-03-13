# 2026년 AI·디지털 활용 선도학교 현황 웹사이트 - 작업 노트

## 📌 기본 정보

| 항목 | 내용 |
|------|------|
| **GitHub 저장소** | https://github.com/jaehuijeon/digital-leading-schools |
| **배포 주소 (GitHub Pages)** | https://jaehuijeon.github.io/digital-leading-schools/ |
| **GitHub 사용자명** | jaehuijeon |
| **로컬 프로젝트 경로** | `C:/Users/jaehu/Desktop/digital-leading-schools/` |
| **원본 데이터 경로** | `C:/Users/jaehu/Desktop/ailabs_first sesh/` |

---

## 📁 주요 파일 구조

```
digital-leading-schools/
├── index.html          ← 메인 파일 (지도 + 데이터 + 스타일 모두 포함)
├── schools.json        ← 학교 데이터 (148KB, 12개 지역)
├── korea-provinces.json← 한국 행정구역 GeoJSON (3.3MB, 참고용)
└── PROJECT_NOTES.md    ← 이 파일
```

> ⚠️ `index.html` 안에 `const schoolData = {...}`로 학교 데이터가 **인라인 삽입**되어 있음
> (외부 fetch 방식은 GitHub Pages에서 문제가 생겨 인라인으로 전환)

---

## ✅ 완료된 작업

### 1. 데이터 수집 (12개 교육청)
각 교육청 홈페이지에서 xlsx/pdf 파일 직접 다운로드 후 파싱

| 지역 | 학교 수 | 파일 형식 | 출처 URL |
|------|--------|----------|---------|
| 서울특별시 | 190개 | xlsx | https://buseo.sen.go.kr/ |
| 부산광역시 | ~100개 | xlsx | https://www.pen.go.kr/ |
| 대구광역시 | ~80개 | pdf | https://www.dge.go.kr/ |
| 인천광역시 | ~90개 | xlsx | https://www.ice.go.kr/ |
| 광주광역시 | ~60개 | xlsx | http://www.gen.go.kr/ |
| 강원특별자치도 | ~100개 | xlsx | https://www.gwe.go.kr/ |
| 충청북도 | ~80개 | xlsx | https://www.cbe.go.kr/ |
| 충청남도 | ~90개 | xlsx | http://www.cne.go.kr/ |
| 경상북도 | ~120개 | xlsx | https://www.gbe.kr/ |
| 전라남도 | ~100개 | xlsx | https://www.jne.go.kr/ |
| 전북특별자치도 | ~80개 | xlsx | https://www.jbe.go.kr/ |
| 제주특별자치도 | ~40개 | xlsx | https://www.jje.go.kr/ |
| **합계** | **약 1,316개** | | |

### 2. 지도 구현
- 실제 한국 행정구역 GeoJSON 기반 SVG 경로 생성
- D3.js 외부 fetch → 로컬 파일 → **SVG 경로 직접 인라인 삽입** (최종)
- 클릭 이벤트: `data-name` 속성 기반, `selectRegion(regionId)` 함수

### 3. UI 기능
- 지역 클릭 → 학교 목록 + 통계 표시
- 초/중/고/특수 학교급별 필터
- 학교명 검색
- 지역별 교육지원청 그룹핑
- 출처(교육청 공식 홈페이지) 링크 표시

### 4. 배포
- GitHub Pages 사용 (main 브랜치 자동 배포)

---

## ⚠️ 데이터 없는 지역 (회색 표시)
아직 교육청에서 공개하지 않은 지역:
- 경기도, 대전광역시, 세종특별자치시, 울산광역시, 경상남도

→ 해당 교육청이 파일 공개 시 추가 가능

---

## 🔧 데이터 추가 방법 (다음 작업 시 참고)

### 새 지역 데이터 추가 절차
1. 교육청 홈페이지에서 xlsx/pdf 파일 다운로드
2. Python으로 파싱 (openpyxl / pdfminer 사용)
3. `index.html` 내 `const schoolData` 객체에 해당 지역 배열 추가
4. `const sourceData` 객체에 출처 URL 추가
5. git commit & push

### Git 명령어
```bash
cd "C:/Users/jaehu/Desktop/digital-leading-schools"
git add index.html
git commit -m "Add: OO 지역 선도학교 데이터 추가"
git push
```

---

## 🐛 과거에 겪었던 주요 문제 & 해결법

| 문제 | 원인 | 해결 |
|------|------|------|
| `<script src="">` 안에 인라인 코드가 실행 안 됨 | src 속성이 있으면 내부 코드 무시됨 | script 태그 분리 |
| "지도 로딩 실패" 오류 | raw.githubusercontent.com fetch 차단 | GeoJSON을 SVG 경로로 직접 인라인 삽입 |
| GeoJSON 파일 너무 큼 (7.5MB) | 원본 좌표 정밀도 | 좌표 소수점 4자리로 반올림 → 3.3MB |
| 한글 깨짐 (GeoJSON 속성) | cp949 인코딩 | `name_eng` 영문 속성 사용 후 매핑 |
| 파이썬 터미널 한글 깨짐 | Windows cp949 터미널 | 실제 파일은 utf-8로 정상 저장됨 (무시해도 됨) |
| 전북 파일 다운로드 시 HTML 오류 페이지 반환 | 잘못된 fileSid 파라미터 | `fileSid=691012` 파라미터로 수정 |

---

## 📊 index.html 핵심 구조

```javascript
// 1. 출처 데이터
const sourceData = {
  '서울특별시': { name: '서울특별시교육청', url: 'https://buseo.sen.go.kr/' },
  // ...
};

// 2. 학교 데이터 (인라인)
const schoolData = {
  '서울특별시': [ {지역:'강남서초', 학교명:'OO초등학교', 학교급:'초'}, ... ],
  // ...
};

// 3. 지역 클릭 처리
function selectRegion(regionId) { ... }
function renderInfo(regionId) { ... }
```

---

*마지막 업데이트: 2026-03-13*
