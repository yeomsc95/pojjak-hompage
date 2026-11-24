# pojjak-homepage

뽀짝(pojjak.kr) 서비스 소개 홈페이지. 정적 HTML/CSS/JS 단일 페이지.

## 서빙

nginx가 `pojjak.kr/` 루트로 **`public/` 폴더만** (`C:/coupon/homepage/public`) 정적 서빙한다
(예전 주소 `pojjak.kr/main/`은 `/`로 리다이렉트). 빌드 과정 없이 파일 저장 = 즉시 반영.
없는 경로는 `index.html`로 떨어지므로, 새 파일은 실제로 올라갔는지 크기·내용으로 확인할 것.

**손님에게 보여줄 파일은 전부 `public/` 안에 둔다.** 저장소 루트(`.git`, README 등)는
인터넷에 나가지 않게 일부러 서빙 폴더 밖에 둔 것이다. 루트에 웹 파일을 두면 안 보인다.

`public/` 안의 파일:

- `index.html` : 소개 페이지 본문 (인라인 CSS/JS). 글꼴은 Pretendard를 jsDelivr CDN에서 받는다.
- `favicon.ico`, `apple-touch-icon.png` : 탭·홈 화면 아이콘. `pojjak-admin/front/public/`의 같은 파일을 복사한 것
- `robots.txt` : 검색엔진 수집 허용 + 사이트맵 위치
- `privacy.html` : 개인정보처리방침 (`pojjak-admin/front/public/privacy.html`과 같은 내용 - 고칠 때 둘 다)
- `pojjak-icon.png` : 로고
- `pojjak-stamp.png` : 손님 화면 목업의 빨간 뽀짝 도장. `pojjak-tablet/app/assets/images/`의 같은 파일을 복사한 것
- `pojjak-of-main-page.png` : 히어로의 태블릿 메인 화면 목업 왼쪽 위 티켓 로고. 위와 같은 폴더의 같은 파일을 복사한 것
  (메인 화면 목업은 `pojjak-tablet` **main 브랜치**의 `main_page.dart` 기준. 매장 브랜치는 도장 이미지 등이 다를 수 있음)
- `sitemap.xml` : 검색엔진용 사이트맵 (네이버 서치어드바이저에 제출)
- `naver8f9d1271d5b02dcba43246b91f2dcb3a.html` : 네이버 서치어드바이저 사이트 소유확인 파일. **지우지 말 것**
  (`index.html` 머리말의 `naver-site-verification` 메타 태그도 같은 용도)

## 작업 흐름

이 폴더에서 바로 고치면 수정 중인 상태가 그대로 손님에게 보인다. 다른 PC에서 고치고
커밋·푸시한 뒤, 서버(`C:/coupon/homepage`)에서는 `git pull`만 한다.

## 관련 저장소

- `pojjak-api` : Go 백엔드 (간편문의 접수 API `/api/inquiries` 포함)
- `pojjak-admin` : React 관리자 웹 (`pojjak.kr/admin`)
- `pojjak-tablet` : Flutter 태블릿 앱
