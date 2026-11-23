# pojjak-homepage

뽀짝(pojjak.kr) 서비스 소개 홈페이지. 정적 HTML/CSS/JS 단일 페이지.

## 서빙

nginx가 `pojjak.kr/main/` 경로로 이 폴더(`C:/coupon/homepage`)를 직접 정적 서빙한다.
빌드 과정 없이 파일 저장 = 즉시 반영.

- `index.html` : 소개 페이지 본문 (인라인 CSS/JS)
- `privacy.html` : 개인정보처리방침
- `pojjak-icon.png` : 로고

## 관련 저장소

- `pojjak-api` : Go 백엔드 (간편문의 접수 API `/api/inquiries` 포함)
- `pojjak-admin` : React 관리자 웹 (`pojjak.kr/admin`)
- `pojjak-tablet` : Flutter 태블릿 앱
