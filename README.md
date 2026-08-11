# MediOps Pilot

병원 내부 운영을 위한 파일럿 웹앱입니다.

현재 목표는 한 병원에서 실제 사용 흐름을 검증하는 것이며, 향후 Firebase 기반 데이터 축적과 다병원 확장을 고려합니다.

## 현재 구현 범위

- 관리자/직원 모드 분리
- 관리자 대시보드
- 직원 홈
- 직원 재고 조회 및 원터치 사용 등록 UI
- 재고 부족/유통기한 표시
- 출퇴근/근태 화면
- 연차 승인/신청 화면
- 직원 관리 화면
- 거래처/패키지 관리 화면
- 모바일 우선 반응형 UI

## Firebase Hosting

`firebase/public/index.html`이 현재 파일럿 사이트입니다.

Firebase CLI가 설치되어 있다면 저장소 루트에서 다음 순서로 배포할 수 있습니다.

```bash
firebase login
firebase use --add
firebase deploy --only hosting
```

## 데이터베이스 계획

다음 단계에서 Cloud Firestore를 연결해 아래 데이터를 축적합니다.

- 재고 입출고 이력
- 일/주/월 평균 소모량
- 예상 소진일
- 평균 주문 주기
- 평균 주문 수량
- 거래처별 평균 리드타임
- 권장 주문 시점/수량
- LOT/유통기한
- 패키지 잔액/서비스 수량/만료/재계약
- 출퇴근/근태/연차 이력
- 감사 로그

## 폴더 구조

```text
firebase/          Firebase Hosting 배포 파일
prototype/         독립 실행형 파일럿 HTML
docs/              요구사항, 와이어프레임, 개발/테스트 문서
database/          초기 관계형 DB 설계 참고자료
```

## 주의

현재 파일럿 UI에는 데모 데이터가 포함되어 있습니다. 실제 직원/병원 데이터를 저장하기 전에는 Firebase Authentication 및 Firestore Security Rules를 반드시 적용해야 합니다.
