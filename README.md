# miosk
Custom Kiosk

# 계획

- **Java**와 **Spring Framework** 사용
- `Underworld-Line`에서 처럼 **서버-클라이언트** 구조로 설계
- 내부적으로 `서버`, `관리자`, `POS기`, `알림판`으로 분할
    1. 서버
        - 내부적으로 POS기 번호를 관리한다. **POS기**가 **서버**에 연결되면 POS기 번호를 부여받는다.
        - 내부적으로 `주문 순번`과 `주문 번호`를 관리한다.
            - `주문 순번`은 `POS기`에게 전달 받은 주문의 순서이다. 1000 이상이 될 경우 1로 초기화한다.
            - `주문 번호`는 `POS기 번호(1자리)` + `주문 순번(3자리)`이다.
        - **POS기**로부터 주문 정보를 전달받아 **POS기**에 `주문 번호`를 전달하고, **관리자**에게 주문 정보를 전달한다.
            - `Key:Value` 형태로 `주문 번호`, `주문 목록`을 관리한다.
        - **관리자**가 제작을 완료했다는 정보를 전달받으면 `주문 번호`를 **알림판**에 전달한다.
    2. 관리자
        - **서버**로부터 `주문 정보`를 받아 내부 리스트에 저장하여 관리한다.
        - 주문이 완료되면 해당 `주문 번호`를 **서버**로 전달한다.
    3. POS기
        - **서버**에 연결되면 **서버**로부터 `POS기 번호`를 부여받아 내부적으로 관리한다.
        - 주문은 console clear를 이용해 구현한다. 이때 콘솔창에 메뉴 탭과 현재 주문 목록을 같이 출력한다.
        - 사용자로부터 주문을 받아 **서버**로 전달한다.
            - 전달하는 정보: `POS기 번호`, `주문 시각`, `메뉴/수량`
        - **서버**로부터 주문이 완료되었다는 정보를 받아 `주문 번호`를 출력한다.
        - 사용처는 직원 복지를 위해 한시적으로 결제를 받지 않는다고 가정한다.
    4. 알림판
        - **서버**로부터 주문 정보를 받으면 콘솔창의 정보를 갱신한다.
        - **서버**로부터 주문 완료 알림을 받으면 콘솔창의 정보를 갱신한다. 이때, 주문 완료 정보는 5개만 출력한다.
            - `대기`는 배열로, `완료`는 큐로 구현한다.
