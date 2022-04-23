# JS로 렌더링 성능 높이는 법

1. View 업데이트에 setTimeout, setInterval 을 피한다

콜백으로 인해 프레임의 마지막 부분에서 다시 렌더링 파이프라인이 실행될 수 있다.

2.
