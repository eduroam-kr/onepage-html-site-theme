# eduroamKR 참여기관 서비스 안내 템플릿

참여기관의 eduroam 서비스 안내 사이트 템플릿입니다. 국문·영문 두 페이지로 된 단순한 웹사이트며, GitHub Pages를 통한 정적 호스팅이 가능합니다.

순수 HTML/CSS로 되어 있어, GitHub Pages가 아닌 기관 웹서버에 파일을 그대로 올려도 동작합니다.


## 1. 두 가지 사용 방식

참여기관 실정에 맞게 다음 방식 중 한가지를 선택합니다.

### 1.1. 방법 1 — `eduroam.기관도메인`으로 안내 사이트를 운영합니다

`index.html`(국문)과 `en/index.html`(영문)을 사용합니다. 기기별 설정, 인증서 정보, 이용장소, 보안정책을 모두 담고 인증서 파일도 내려받게 할 수 있습니다. 대신 DNS 레코드와 호스팅(GitHub Pages 또는 기관 웹서버) 설정이 필요합니다.

### 1.2. 방법 2 — 기존 안내 페이지에 내용을 추가합니다

정보통신팀 홈페이지처럼 이미 있는 웹사이트에 `short.html`의 조각만 붙여 넣습니다. 별도 도메인도 호스팅도 필요 없습니다.

이 경우 **인증서 지문을 조각 안에 직접 적습니다.** 사용자가 서버 인증서를 대조할 다른 페이지가 없기 때문입니다. `short.html`에 지문 표가 들어 있는 이유입니다.


## 2. 수정할 곳

예제는 모두 예제연구소(Example Research Institute) 기준입니다. `index.html`, `en/index.html`, `short.html`에서 아래 문자열을 기관에 해당하는 것으로 바꿉니다.

> [!IMPORTANT]
> 다음 문자열이 여러 곳에 나옵니다. 하나라도 빠뜨리지 않고 바꿉니다.

| 예제 값 | 설명 |
|---------|------|
| `assets/images/logo.png` | 기관 로고 (머리글에 eduroam 로고와 나란히 표시) |
| `예제연구소`, `Example Research Institute` | 기관명 (국문, 영문) |
| `example.re.kr` | RADIUS realm (`아이디@realm`, `anonymous@realm`) |
| `rad.eduroam.example.re.kr` | RADIUS 서버 인증서의 CN (DNS 등록 불필요, 아래 참고) |
| `eduroam-kr.github.io/site-template` | 이 사이트 주소 (`canonical`, `og:url`, `og:image`, `hreflang`) |
| `정보통신팀`, `IT Team`, `helpdesk@example.re.kr`, `042-000-0000`, `+82-42-000-0000` | 문의처 |
| 인증서 CN, 유효기간, SHA-256 | `assets/certs/`의 파일과 일치시킴. `short.html`에도 같은 값이 들어갑니다 |
| `index.html`의 `3.1. 원내 이용장소` 표 | 기관 건물·구역별 설치 현황 |
| `en/index.html`의 `3.1. On-site coverage` 표 | 위와 같은 내용 (영문) |
| `short.html`의 `이용장소` 첫 항목 | 위와 같은 내용 (한 줄 요약) |

> [!WARNING]
> 예제 기관명 `예제연구소`는 받침이 없어 뒤에 `는`, `를`, `와`, `가`, `로`가 붙어 있습니다. 기관명이 받침으로 끝나면(`한국과학기술정보연구원`처럼) 조사도 같이 `은`, `을`, `과`, `이`, `으로`로 고쳐야 합니다.
>
> ```bash
> # <기관명>을 기관 이름으로 바꿔서 실행합니다. 결과가 없어야 합니다.
> grep -o '<기관명>[는를와가로]' index.html short.html
> ```


## 3. 인증서 정보

> [!IMPORTANT]
> 인증서버의 CA/서버 인증서 또는 인증서의 해시값은 사용자가 중간자 공격(MITM)을 받지 않고 올바른 서버와 통신한다는 것을 확인하는 중요한 정보입니다.  
> 보안을 위하여 웹사이트에 공개하고, 최초 접속시 사용자가 확인하도록 해야 합니다.

인증서는 다음 경로에 올립니다.

```
assets/certs/ca.pem.crt    기관 CA 인증서 (이용자가 내려받아 설치)
assets/certs/server.pem    RADIUS 서버 인증서 (지문 대조·확인용)
```

두 파일 모두 PEM 형식입니다. CA 인증서만 `.crt` 확장자를 쓰는데, Windows 가 `.pem` 을 인증서 파일로 연결해 두지 않아 두 번 눌러도 설치 창이 뜨지 않기 때문입니다. 서버 인증서는 설치하는 파일이 아니라 지문을 대조하는 참고 자료이므로 `.pem` 으로 둡니다 — `.crt` 로 두면 이용자가 설치 창을 보고 루트 인증서로 잘못 설치할 수 있습니다.

인증서 지문은 다음 명령으로 확인하고, html 파일 본문에 갱신합니다.

운영체제마다 보여 주는 지문이 다릅니다. Windows 연결 창은 SHA-1, macOS·iOS 인증서 화면은 SHA-256 을 씁니다. 이용자가 어느 화면을 보든 대조할 수 있도록 **두 값을 모두 싣습니다.**

```sh
openssl x509 -in assets/certs/ca.pem.crt -noout -subject -dates -fingerprint -sha256
openssl x509 -in assets/certs/ca.pem.crt -noout -fingerprint -sha1

openssl x509 -in assets/certs/server.pem -noout -subject -dates -fingerprint -sha256
openssl x509 -in assets/certs/server.pem -noout -fingerprint -sha1
```

한 번의 호출로 두 해시를 함께 뽑을 수는 없습니다. `-fingerprint -sha256 -fingerprint -sha1` 은 `Multiple digest or unknown options` 로 거부됩니다.

MD5 는 싣지 않습니다. 충돌 공격이 실증된 해시라 인증서 확인 수단으로 쓰면 안 됩니다.

다음은 예제 인증서의 정보입니다. 인증서 이름은 CN(CommonName) 입니다. RADIUS 서버 인증서의 CN은 기관 DNS 서버에 등록되지 않아도 무방합니다.

```
# openssl x509 -in assets/certs/ca.pem.crt -noout -subject -dates -fingerprint -sha256
subject=C=KR, ST=Daejeon, L=Daejeon, O=Example Research Institute, CN=Example Research Institute eduroam Root CA
notBefore=Sep 28 03:55:57 2026 GMT
notAfter=Sep 25 03:55:57 2036 GMT
sha256 Fingerprint=96:6D:12:B5:33:50:17:AA:18:EC:A1:E9:EA:E3:EA:DD:88:0D:1A:B3:F8:A2:8D:30:68:3F:1D:15:DC:F1:A6:6C

# openssl x509 -in assets/certs/ca.pem.crt -noout -fingerprint -sha1
sha1 Fingerprint=83:70:D4:FE:1B:18:A8:8A:1C:2C:64:5A:24:47:3F:D9:67:5A:57:05

# openssl x509 -in assets/certs/server.pem -noout -subject -dates -fingerprint -sha256
subject=C=KR, ST=Daejeon, O=Example Research Institute, CN=rad.eduroam.example.re.kr
notBefore=Sep 28 03:55:57 2026 GMT
notAfter=Sep 27 03:55:57 2028 GMT
sha256 Fingerprint=F8:14:6B:FA:ED:C6:60:0A:D1:9A:F6:EB:92:E7:26:C1:26:D4:99:FB:6A:4B:D6:AE:D6:2C:39:C1:D5:C0:DA:EC

# openssl x509 -in assets/certs/server.pem -noout -fingerprint -sha1
sha1 Fingerprint=A8:26:01:64:C6:90:EE:00:16:C4:EE:7C:10:2F:2C:1E:46:82:E3:42
```


## 4. 권리 고지 문구

두 파일 하단의 `BEGIN eduroam-KR notice` ~ `END eduroam-KR notice` 사이 문구는 대한민국 eduroam 참여기관 공통 문구입니다. `index.html`에 국문, `en/index.html`에 영문이 하나씩 있습니다. 문구와 링크는 수정하지 않고, 다음 두 가지만 바꿉니다.

1. 기관명
2. 기관명 뒤 조사를 기관명에 맞게 `은` 또는 `는`으로

글자 크기(14px 이상)와 색 대비를 줄이거나 문구를 숨기면 안 됩니다.


## 5. 사용 방식

### 5.1. GitHub Pages를 통한 배포

#### DNS 설정

`eduroam.example.re.kr`로 서비스하는 예입니다. 기관 DNS에 CNAME 레코드를 추가합니다. `<계정>`은 저장소를 가진 GitHub 사용자 또는 조직 이름입니다.

```
eduroam.example.re.kr.    IN CNAME    <계정>.github.io.
```

확인:

```sh
dig +short eduroam.example.re.kr CNAME
```

#### GitHub Pages 설정

1. 이 저장소를 fork 하거나 새 저장소에 올립니다. (저장소는 public)
2. 위의 '수정할 곳'을 바꿉니다.
3. `CNAME` 파일에 사이트 주소를 한 줄로 적습니다.

    ```
    eduroam.example.re.kr
    ```

    `CNAME` 파일이 있으면 GitHub Pages가 그 도메인을 커스텀 도메인으로 잡습니다. 도메인 없이 `<계정>.github.io/<저장소>/`로 먼저 시험해 보려면 이 파일을 두지 않습니다.

4. 저장소 **Settings → Pages**에서 Source를 `Deploy from a branch`, Branch를 `main` / `/ (root)`로 지정합니다.
5. 같은 화면의 Custom domain에 `eduroam.example.re.kr`이 들어가 있는지 확인합니다.

DNS 전파 후 GitHub가 인증서를 발급하면 **Settings → Pages**에서 **Enforce HTTPS**를 켭니다. 이후 https://eduroam.example.re.kr 로 접속할 수 있습니다.


### 5.2. 기관 웹서버를 통한 배포

저장소 파일을 웹 루트에 그대로 복사합니다. `CNAME`, `.nojekyll`, `README.md`, `CLAUDE.md`는 올리지 않아도 됩니다.


## 5.3. short.html 사용법

`short.html`의 `BEGIN eduroam-KR short` ~ `END eduroam-KR short` 사이를 복사해 기존 웹사이트의 게시글이나 페이지 본문(HTML 편집 모드)에 붙여 넣습니다.


## 6. 파일 구성

| 파일 | 용도 |
|------|------|
| `index.html` | eduroam 안내 사이트 (국문) |
| `en/index.html` | eduroam 안내 사이트 (영문) |
| `short.html` | 기존 기관·부서 웹사이트 본문에 붙여 넣는 짧은 안내 조각 |
| `CNAME` | GitHub Pages 사용자 도메인 (GitHub Pages 배포시 생성) |
| `.nojekyll` | GitHub Pages의 Jekyll 처리를 끔 (지우지 마세요) |
| `assets/icons/` | 파비콘 |
| `assets/images/eduroam-logo.svg` | eduroam 로고 (머리글) |
| `assets/images/og-image.png` | 카카오톡·트위터·페이스북 등에 링크를 붙였을 때 뜨는 그림 (1200×630) |
| `assets/images/logo.png` | 기관 로고 (교체 대상) |
| `assets/certs/` | 기관 CA·RADIUS 서버 인증서 (예제 파일이 들어 있음) |
| `CLAUDE.md` | AI 유지보수 가이드 |


## 7. 에듀롬 인증서 권장 규격

### 7.1. RADIUS CA (사설 루트) 인증서 권장 항목

| 항목 | 권장 |
|---|---|
| 유효기간 | 10~20년 |
| `basicConstraints` | `critical, CA:TRUE, pathlen:0` |
| `keyUsage` | `critical, keyCertSign, cRLSign` |
| `extendedKeyUsage` | 넣지 않음 |
| 키 | RSA 4096 또는 EC P-384 |
| 서명 | SHA-256 |

### 7.2. RADIUS 서버 인증서 권장 항목

| 항목 | 권장 |
|---|---|
| 유효기간 | 2~5년 |
| CN | FQDN (와일드카드 금지), DNS에 등록 필요 없음 |
| `subjectAltName` | `DNS:` CN과 동일 |
| `basicConstraints` | `critical, CA:FALSE` |
| `keyUsage` | `critical, digitalSignature, keyEncipherment` |
| `extendedKeyUsage` | `serverAuth` 필수 |
| 키 | RSA 2048 이상 |
| 서명 | SHA-256 |

* `serverAuth`는 Windows가 없으면 거부하고, `basicConstraints`는 없으면 macOS에서 문제가 보고된 항목이라 둘 다 빼면 안 됩니다. SAN(subjectAltName)은 Windows·Android가 이름 대조에 쓰므로 CN과 반드시 같아야 합니다.
* CA가 그대로면 서버 인증서 갱신은 단말에 아무 영향이 없습니다.

출처: [GÉANT Best Practice — Server Certificate Practices in eduroam](https://archive.geant.org/projects/gn3/geant/services/cbp/Documents/cbp-33_server-certificate-practices-in-eduroam.pdf), [GÉANT wiki — EAP Server Certificate considerations](https://wiki.geant.org/spaces/H2eduroam/pages/121346323/EAP+Server+Certificate+considerations), [Jisc — Certificates in eduroam](https://community.jisc.ac.uk/library/network-and-technology-service-docs/certificates-eduroam/1000)


## 8. 문의

대한민국 에듀롬 공식 웹사이트 : https://eduroam.kreonet.net
