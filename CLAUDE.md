# CLAUDE.md - AI 에이전트 작업 지침

이 파일은 Claude가 이 프로젝트를 열 때 자동으로 읽는 파일입니다.

## 프로젝트 개요
2026년 AI·디지털 활용 선도학교 현황을 보여주는 한국 지도 웹사이트

- **배포 주소**: https://jaehuijeon.github.io/digital-leading-schools/
- **GitHub 저장소**: https://github.com/jaehuijeon/digital-leading-schools
- **로컬 경로**: `C:/Users/jaehu/Desktop/digital-leading-schools/`
- **상세 작업 노트**: `PROJECT_NOTES.md` 참고

## 작업 전 필수 확인

1. 로컬 파일 수정 후 반드시 **git push까지** 해야 GitHub Pages에 반영됨
2. 학교 데이터는 `index.html` 안의 `const schoolData`에 **인라인**으로 들어 있음
3. 출처 데이터는 `const sourceData`에 있음
4. 지도는 SVG 경로가 HTML에 직접 삽입되어 있음 (외부 파일 fetch 아님)

## 개발 규칙: TDD (Test-Driven Development)

새 기능/데이터 추가 시 반드시 아래 순서로 진행:
1. **Red**: 테스트/체크리스트 먼저 작성
2. **Green**: 최소한의 코드로 통과
3. **Refactor**: 코드 정리

## Git 워크플로우
```bash
cd "C:/Users/jaehu/Desktop/digital-leading-schools"
git add index.html
git commit -m "커밋 메시지"
git push
# → 1~2분 후 GitHub Pages 자동 반영
```

## 자주 쓰는 확인 명령 (브라우저 콘솔)
```javascript
// 지역별 학교 수
Object.keys(schoolData).map(k => `${k}: ${schoolData[k].length}개`)

// 출처 누락 지역 확인
Object.keys(schoolData).filter(k => !sourceData[k])
```

## 미완료 지역 (데이터 없음)
경기도, 대전광역시, 세종특별자치시, 울산광역시, 경상남도
→ 해당 교육청 홈페이지에서 xlsx/pdf 공개 시 추가할 것
