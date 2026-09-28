# CLAUDE.md — AI 유지보수 가이드

이 저장소는 대한민국 eduroam 참여기관이 쓰는 1페이지 안내 사이트 템플릿입니다. NRO는 KISTI KREONET이며, 참여기관이 fork 해서 기관 값만 바꿔 씁니다. 사람이 읽고 고치기 쉬운 상태를 유지하는 것이 가장 중요합니다.

## 구성

- `index.html` (한국어), `en/index.html` (영문): 같은 내용을 언어별로 나눈 두 페이지. 한쪽을 고치면 다른 쪽도 같은 커밋에서 고칩니다. 언어 전환은 링크 두 개로만 하고 스크립트를 쓰지 않습니다.
  - 두 페이지의 `<style>` 블록은 내용이 같습니다. 한쪽만 고치면 화면이 갈라집니다.
  - 섹션 구성과 앵커(`#intro`, `#howto`, `#visitor`, `#member`, `#area`, `#policy`)는 두 페이지가 동일합니다.
  - 인증서 지문 표는 두 페이지에 모두 둡니다. 값이 갈라지지 않도록 `assets/certs/`의 파일에서 읽어 고칩니다.
- `short.html`: 기존 웹사이트 본문에 붙여 넣는 조각. `BEGIN/END eduroam-KR short` 사이가 실제 배포 대상. 여기에는 Bootstrap을 쓰지 않습니다 — 붙여 넣는 쪽 사이트의 CSS와 충돌하면 안 되기 때문입니다.
- `assets/`: 아이콘, eduroam 로고(`eduroam-logo.svg`), 기관 로고(`logo.png`), 공유 카드 그림(`og-image.png`), 인증서.
  - `og-image.png`는 `eduroam-logo.svg`를 흰 바탕 1200×630 가운데에 600px 폭으로 놓고 2배로 렌더한 뒤 줄인 것입니다. 링크 미리보기는 배경이 어두운 앱이 많아 투명 배경을 쓰지 않습니다. 정사각으로 잘려도 로고가 안 잘리도록 폭을 절반으로 둡니다.
- 기관이 바꿀 값의 목록은 `README.md`의 '수정할 값'에 있습니다. 값을 추가하거나 옮기면 README 표도 함께 고칩니다.

## 표기 규칙

- **eduroam은 항상 소문자로 씁니다.** 문장 첫머리, 제목, 버튼, `<title>`, alt 텍스트에서도 첫 e를 대문자로 쓰지 않습니다.
  - 올바름: `eduroam`, `eduroam 안내`, `eduroam은`
  - 틀림: `Eduroam`, `EduRoam`, `EDUROAM`, `eduRoam`
  - 영문 문장 첫머리도 소문자입니다: `eduroam (education roaming) is ...`
  - 예외 없이 소문자. 외부 서비스명도 원래 표기를 따릅니다 (`eduroam Companion`, `eduroam Korea`)
- 한글 표기는 `에듀롬`입니다. 본문은 `eduroam`, 권리 고지 문구는 원문대로 `에듀롬`을 씁니다.
- GÉANT는 `GÉANT`로 씁니다 (`GEANT`, `Geant` 아님).
- `Wi-Fi`로 씁니다 (`WiFi`, `wifi` 아님).
- 외부 링크는 모두 `https://`로 씁니다.

## 권리 고지 문구 (수정 금지)

`BEGIN eduroam-KR notice` ~ `END eduroam-KR notice` 사이 문단은 NRO가 정한 공통 문구입니다. `index.html`에 국문, `en/index.html`에 영문(`notice (en)`), `short.html`에 국문이 하나씩 있습니다.

- 문장, 링크 URL, 링크 순서를 바꾸지 않습니다. 요약·번역·줄바꿈 편집도 하지 않습니다.
- 문단 첫 줄의 제목(`대한민국 eduroam 서비스 및 상표 고지`, 영문 `eduroam Korea service and trademark notice`)도 문구의 일부입니다. 지우거나 바꾸지 않습니다. ©는 저작권 표시라 여기에 쓰지 않습니다 — 이 문단은 저작권 고지가 아니라 참여 사실과 상표권에 대한 고지입니다.
- 바꿔도 되는 것은 두 가지뿐입니다: 기관명, 기관명 뒤 조사(`은`/`는`). NRO(KISTI KREONET)와 RO(KREN)는 문구에 포함되어 있으므로 지우지 않습니다.
- 링크에 `rel="nofollow"`, `rel="sponsored"`, `rel="ugc"`를 붙이지 않습니다.
- 가시성 기준을 지킵니다:
  - 글자 크기 14px(0.875rem) 이상 (현재 `.edurkr-notice`는 0.9375rem)
  - 배경 대비 4.5:1 이상 (WCAG 2.1 AA), 링크 밑줄 유지
  - `display:none`, `visibility:hidden`, `opacity` 1 미만, 화면 밖 배치, `aria-hidden`, 접기/모달 안에 넣기 금지
  - 이미지가 아닌 HTML 텍스트로, JavaScript로 나중에 넣지 않고 HTML 원문에 포함
- 문구 자체를 고쳐야 하면 이 저장소가 아니라 NRO(eduroam.kreonet.net)에 요청합니다.

head의 `대한민국 eduroam 공통 메타` 블록도 같은 이유로 수정하지 않습니다.

## 하지 말 것

- 빌드 도구, 템플릿 엔진(Jekyll, Hugo 등), 패키지 관리자 도입
- geteduroam·eduroam CAT 안내 추가 (둘 다 국내 등록 기관이 적어 제외함. NRO 방침이 바뀌면 그때 추가)
- 세 번째 언어, 공지사항, 기관 메타데이터 게시 기능 추가 (페이지는 국문·영문 둘로 유지)
- `.nojekyll` 삭제

## 수정 후 점검

```bash
# eduroam 대소문자 (예외 이름 제외 후 결과가 없어야 함)
grep -n -i 'eduroam' index.html en/index.html short.html README.md | grep -E 'Eduroam|EDUROAM|eduRoam|EduRoam'

# http 링크 (결과가 없어야 함, xmlns 제외)
grep -n 'href="http://' index.html en/index.html short.html

# 권리 고지 문구가 index.html 과 short.html 에서 같은지 (국문)
notice() { sed -n "/BEGIN eduroam-KR notice/,/END eduroam-KR notice/p" "$1" | sed -e 's/<[^>]*>//g' -e 's/^ *//' -e '/^$/d'; }
diff <(notice index.html) <(notice short.html)

# 표시된 인증서 값이 실제 파일과 맞는지
for f in assets/certs/ca.pem.crt assets/certs/server.pem; do
  openssl x509 -in "$f" -noout -subject -dates
  openssl x509 -in "$f" -noout -fingerprint -sha256
  openssl x509 -in "$f" -noout -fingerprint -sha1
done
```

