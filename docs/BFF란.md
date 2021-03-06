# BFF란?

MSA 아키텍쳐의 여러 패턴 중 하나 / 프론트엔드와 마이크로 서비스 간의 간단한 인터페이스 역할을 한다. 단일 BFF 는 단일 UI 및 해당 UI에만 초점을 맞춘다. 

### 해결하려는 문제

 현대의 서비스는 클라이언트의 다양한 device에 대한 대응이 필수적이다. (ex. 태블릿, 모바일, 웹) 그런데 API Gateway 패턴을 사용하여 통합된 API 엔드포인트 사용한다면 나라별로 비즈니스 로직이 다르다던가, 사용자의 디바이스에 따른 요청에 대응하기가 힘들다. 

→ 많은 API와 서드파티 API들을 UI별로(사용자의 디바이스별로) 나눠 효율적으로 대응하기 위하여 BFF를 추가한다.

장점

- 우려사항 분리 : 백엔드와 프론트엔드의 요구사항을 분리하여 유지보수 용이
- 탄력적인 API 유지 및 수정 - 클라이언트 어플리케이션이 API구조에 대해 자세히 알 필요 없음 (unfied api 에 비해)
- 더 나은 오류 처리
- 여러 디바이스가 백엔드를 병렬 호출 가능 → 응답속도 향상