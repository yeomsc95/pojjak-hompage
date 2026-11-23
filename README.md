# pojjak-homepage

뽀짝(pojjak.kr) 서비스 소개 홈페이지. 정적 HTML/CSS/JS 단일 페이지.

## 서빙

nginx가 `pojjak.kr/main/` 경로로 이 폴더(`C:/coupon/homepage`)를 직접 정적 서빙한다.
빌드 과정 없이 파일 저장 = 즉시 반영.

- `index.html` : 소개 페이지 본문 (인라인 CSS/JS). 글꼴은 Pretendard를 jsDelivr CDN에서 받는다.
- `privacy.html` : 개인정보처리방침 (`pojjak-admin/front/public/privacy.html`과 같은 내용 - 고칠 때 둘 다)
- `pojjak-icon.png` : 로고
- `pojjak-stamp.png` : 손님 화면 목업의 빨간 뽀짝 도장. `pojjak-tablet/app/assets/images/`의 같은 파일을 복사한 것
- `pojjak-of-main-page.png` : 히어로의 태블릿 메인 화면 목업 왼쪽 위 티켓 로고. 위와 같은 폴더의 같은 파일을 복사한 것
  (메인 화면 목업은 `pojjak-tablet` **main 브랜치**의 `main_page.dart` 기준. 매장 브랜치는 도장 이미지 등이 다를 수 있음)

## 작업 흐름

이 폴더에서 바로 고치면 수정 중인 상태가 그대로 손님에게 보인다. 다른 PC에서 고치고
커밋·푸시한 뒤, 서버(`C:/coupon/homepage`)에서는 `git pull`만 한다.

## 관련 저장소

- `pojjak-api` : Go 백엔드 (간편문의 접수 API `/api/inquiries` 포함)
- `pojjak-admin` : React 관리자 웹 (`pojjak.kr/admin`)
- `pojjak-tablet` : Flutter 태블릿 앱
