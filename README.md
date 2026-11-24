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
웹 파일만 바뀌는 pull이면 nginx는 건드릴 필요 없다.

## 서버(nginx) 설정 - nginx 고칠 때 먼저 읽을 것

설정 파일은 homeGym 서버의 `C:\nginx\conf\nginx.conf` (서버 로컬 파일, git 대상 아님).
`pojjak.kr` server 블록 하나에 홈페이지(`/`), 관리자 웹(`/admin`), API(`/api`)가 같이 있으니
**고치기 전에 같은 폴더에 날짜를 붙여 백업**하고, 끝나면 아래 확인 목록을 전부 돌린다.
API·관리자 쪽 nginx 규칙(`/api/internal/` 차단 등)은 `pojjak-api/CLAUDE.md` 13·15번 참고.

### 홈페이지 규칙 (2026-11-24 정리)

- **root는 `C:/coupon/homepage/public`.** 저장소 루트(`C:/coupon/homepage`)로 되돌리면 안 된다.
  2026-11-24까지 저장소 루트를 서빙해서 `https://pojjak.kr/.git/config`, `/README.md` 등이
  인터넷에서 그대로 다운로드됐다 (로그인 정보는 없었음). 그래서 웹 파일을 `public/`으로 옮겼다.
- 차단 규칙 두 줄 (server 블록 안, **다른 location들보다 위**에 둔다. nginx 정규식 location은
  위에서부터 처음 맞는 것을 쓰기 때문):
  ```nginx
  location ~ /\.(?!well-known) { return 404; }   # .git 같은 숨김 파일·폴더
  location ~* \.md$ { return 404; }               # README 같은 문서
  ```
  `public/`만 서빙하므로 이중 안전장치다. `.well-known`은 인증서 갱신용이라 일부러 뺐다.
- 없는 경로는 `index.html`로 떨어진다 (404 대신 홈페이지가 뜸). 그래서 "200이 나온다"만으로는
  파일이 있는지 알 수 없다 - 크기나 내용으로 확인할 것.
- `/main`, `/main/` → `/` 로 301 (예전 홈페이지 주소).
- `/robots.txt`는 지금 `public/robots.txt`가 아니라 관리자 웹 빌드 쪽 파일이 나간다
  (둘 다 "수집 허용"이라 문제는 없음). `public/robots.txt`를 쓰게 하려면 그 규칙을 정리할 것.
- **인증서 갱신 확인 필요:** `/.well-known/acme-challenge/` 요청도 홈페이지 root(`public/`)로 간다
  (전용 location 없음). `C:\Certbot\renew-pojjak.bat`이 webroot 방식이면 그 경로(`-w`)가
  `C:/coupon/homepage/public`이어야 갱신된다. 바꾼 뒤엔 `certbot renew --cert-name pojjak.kr --dry-run`
  으로 시험해 볼 것. 갱신이 조용히 실패하면 최대 90일 뒤 사이트 전체가 인증서 오류로 막힌다.

### 적용

nginx는 NSSM 서비스 `bro-nginx`로 돈다. 이 구성에서는 `nginx -s reload`가 먹히지 않아서
재시작으로 적용한다 (재시작 동안 몇 초 끊김). 관리자 PowerShell에서:

```powershell
cd C:\nginx; .\nginx.exe -t          # 문법 검사 - 통과할 때만 다음 줄
C:\nssm\nssm.exe restart bro-nginx
```

검사가 실패하면 재시작하지 말고 백업으로 되돌린다.

### 바꾼 뒤 확인 목록 (서버 밖에서, 예: 다른 PC의 curl)

| 주소 | 기대 결과 |
|---|---|
| `/.git/HEAD`, `/.git/config`, `/README.md`, `/.gitignore` | 404 |
| `/` | 200, 홈페이지 |
| `/privacy.html`, `/sitemap.xml`, `/favicon.ico`, `/pojjak-stamp.png` | 200, 각 파일 (홈페이지 HTML이 아님) |
| `/naver8f9d1271d5b02dcba43246b91f2dcb3a.html` | 200, `naver-site-verification: ...` 한 줄 (홈페이지가 뜨면 소유확인이 풀림) |
| `/main/` | 301 → `https://pojjak.kr/` |
| `/admin/` | 200, 관리자 웹 |
| `/api/inquiries` (GET) | `404 page not found` 짧은 텍스트 = Go 서버까지 닿은 것 (POST 전용 경로라 정상) |
| `/api/internal/inquiries` | 403 (nginx 차단) |

## 관련 저장소

- `pojjak-api` : Go 백엔드 (간편문의 접수 API `/api/inquiries` 포함)
- `pojjak-admin` : React 관리자 웹 (`pojjak.kr/admin`)
- `pojjak-tablet` : Flutter 태블릿 앱
