# MediOps Pilot

병원 내부 운영을 위한 파일럿 웹앱입니다. **한 병원 파일럿**으로 실사용 흐름을 검증하며, 향후 Firebase 기반 데이터 축적과 다병원(SaaS) 확장을 고려합니다.

## 현재 구현 (실동작)

모바일 우선 단일 페이지 웹앱(`index.html`), 설치·빌드 불필요.

- **로그인**: 전화번호 + 개인 PIN (무료, SMS 미사용). 최초 실행 시 관리자 계정 등록.
- **권한**: 관리자 / 직원. 직원은 재고·출퇴근·연월차 신청, 관리자는 승인·관리·설정.
- **재고**: 품목(거래처·단가·유통기한), 원터치 입출고, 부족/품절/유통기한 알림, 엑셀(.xlsx/CSV) 일괄 등록/내보내기.
- **임플란트**: 브랜드·부품종류(픽스처/어버트먼트/힐링캡/커버스크류 등) 직접 등록, 지름×길이 **규격 매트릭스**, LOT 기록.
- **출퇴근**: 출근/퇴근 원터치, 근무시간 계산, 월별 집계. (직원은 본인만)
- **연월차**: 본인 신청 → 관리자 승인/반려, 잔여 연차 자동 계산.
- **알림 인박스**: 부족·품절·유통기한 임박·연월차 승인대기 집계.
- **공유**: Firebase Firestore 실시간 동기화(익명 로그인) — 한 링크로 전 직원 공유.

## 배포

앱은 정적 파일이라 저장소 **루트**에서 바로 서빙됩니다. 둘 중 편한 방법:

### GitHub Pages (가장 간단)
저장소 **Settings → Pages → Deploy from a branch → 브랜치 선택 → `/(root)` → Save.**
→ `https://<계정>.github.io/mediops-pilot/`

### Firebase Hosting
```bash
firebase login
firebase use --add
firebase deploy --only hosting
```
`firebase.json`의 `public`은 루트(`.`)로 설정돼 있습니다.

## 설정 (공유 켜기)

`firebase-config.js`에 Firebase 웹 설정을 넣으면 실시간 공유가 켜집니다(값이 비면 이 기기 저장). **웹 config의 apiKey는 비밀키가 아니라 공개 식별값**입니다(클라이언트에 배포됨). 실제 보안은 `firestore.rules`(로그인 사용자만 접근) + 링크 비공개로 관리합니다.

Firestore 보안규칙은 `firestore.rules`에 있습니다. 배포:
```bash
firebase deploy --only firestore:rules
```

## 문서

- `docs/PRD.md` — 전체 MVP 요구사항(패키지·발주·거래처 등 확장 계획 포함)
- `docs/DATA_MODEL.md` — Firestore 데이터 모델 설계(현재값/거래이력 분리, hospitalId, 역할, append-only)
- `docs/DEV_PROMPTS.md` / `docs/TEST_PLAN.md` / `docs/WIREFRAMES.md`

## 폴더 구조

```text
index.html            파일럿 앱(본체)
firebase-config.js    Firebase 웹 설정(공유용)
manifest.webmanifest, sw.js, icon-*.png   PWA(홈화면 설치·오프라인)
xlsx.mini.min.js      엑셀(.xlsx) 읽기 라이브러리(SheetJS, MIT)
firebase.json, firestore.rules            Firebase Hosting·보안규칙
docs/                 요구사항·데이터모델·와이어프레임·테스트
```

## 주의

- 개인정보/환자정보·`.env`·서비스계정 JSON·서버 비밀키는 커밋 금지(`.gitignore` 참고).
- 현재 로그인은 병원 내부 신뢰 기반의 소프트 인증(전화번호+PIN)입니다. 강한 본인인증이 필요하면 통신사 SMS OTP(유료)를 후속 옵션으로 검토합니다.
