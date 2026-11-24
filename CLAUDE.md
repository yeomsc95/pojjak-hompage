# pojjak-homepage

뽀짝(pojjak.kr) 소개 홈페이지. 정적 HTML 한 장. 자세한 설명은 `README.md`.

- **웹 파일은 반드시 `public/` 안에 둔다.** 서버 nginx는 `public/`만 서빙한다.
  저장소 루트에 둔 파일은 인터넷에 안 나가고, 루트를 서빙하게 바꾸면 `.git`이 노출된다.
- 서버(`C:/coupon/homepage`)에서 직접 고치지 않는다. 커밋·푸시 → 서버에서 `git pull`만.
- **nginx를 건드릴 일이 있으면 `README.md`의 "서버(nginx) 설정" 절을 먼저 읽고**,
  끝나면 그 절의 확인 목록을 전부 확인한다. `nginx -s reload`는 안 먹히고 NSSM 서비스 재시작으로 적용한다.
- `public/naver8f9d1271d5b02dcba43246b91f2dcb3a.html`과 `index.html` 머리말의
  `naver-site-verification` 메타 태그는 네이버 소유확인용이다. 지우지 않는다.
- 히어로·화면 예시의 목업은 실제 태블릿 앱(`pojjak-tablet`)과 관리자 웹(`pojjak-admin`) 화면을
  옮겨 그린 것이다. 그쪽 화면이 바뀌면 여기도 맞춘다 (`index.html` 주석에 기준 파일이 적혀 있음).
- 커밋 메시지는 한국어.
