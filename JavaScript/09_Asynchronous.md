# Asynchronous JavaScript

1. 동기 Synchronous
    - 프로그램의 실행 흐름이 순차적으로 진행
    - 하나의 작업이 완료된 후에 다음 작업이 실행되는 방식
2. 비동기 Asynchronous
    - 특정 잡업의 실행이 완료될 때까지 기다리지 않고 다음 작업을 즉시 실행하는 방식
    - 작업의 완료 여부를 신경쓰지 않고 동시에 다른 작업들을 수행할 수 있음
    - 특징
        - 병렬적 수행
        - 당장 처리를 완료할 수 없고 시간이 필요한 작업들은 백드라운드에서 실행되며 빨리 완료되는 작업부터 처리

## JavaScript와 비동기

1. JavaScript는 Single Thread 언어
    - Tread : 작업을 처리할 때 실제로 작업을 수행하는 주체, multi-thread라면 업무를 수행할 수 있는 주체가 여러 개라는 의미
    - 따라서 Single Thread 언어인 JavaScript는 한 번에 여러 일을 수행 할 수 없다.

### JavaScript Runtime

1. 의미 : JavaScript가 동작할 수 있는 환경
    - 브라우저, Node.js
2. 브라우저 환경에서 JavaScript 비동기 처리 관련 요소
    - JavaScript Engine의 Call Stack
        - 요청이 들어올 때 마다 순차적으로 처리하는 Stack(LIFO)
        - 기본적인 JavaScript의 Single Thread 작업 정리
    - Web API
        - JavaScript 엔진이 아닌 브라우저에서 제공하는 runtime 환경
        - 시간이 소요되는 작업을 처리
    - Task Queue
        - 비동기 처리된 Callback 함수가 대기하는 Queue(FIFO)
    - Event Loop
        - 테스크가 들어오기를 기다렸다가 테스크가 들어오면 이를 처리, 처리할 테스크가 없는 경우 잠드는, 끊임없이 돌아가는 자바스립트 내 루프
        - Call Stack과 Task queue를 지속적으로 모니터링
        - Call Stack이 비어있는지 확인 후 비어 있다면 Task Queue에서 대기중인 오래된 작업을 Call Stack으로 Push
3. 런타임의 동작 과정
    
    ![런타임 구조](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcS4ascwWUV8P1sCNgSUB4iKO2CI0b9MakGMdQ&s)
    - 모든 작업은 Call Stack(LIFO)으로 들어간 후 처리
    - 오래 걸리는 작업이 Call Stack으로 들어오면 Web API로 보내 별도로 처리
    - Web API에서 처리가 끝난 작업들은 곧바로 Call Stack으로 들어가지 못하고 Task Queue(FIFO)에 순서대로 들어감
    - Event Loop가 Call Stack이 비어있는 것을 계속 체크하고 Call Stack이 빈다면 Task Queue에서 가장 오래된(가장 먼저 처리되어 돌아온) 작업을 Call Stack으로 보냄

> **정리** 
> - JavaScript는 한 번에 하나의 작업을 수행하는 Single Thread 언어로 동기적 처리를 진행
> - 하지만 브라우저 환경에서는 Web API에서 처리된 작업이 지속적으로 Task Queue를 거쳐 Event Loop에 의해 Call Stack에 들어와 순차적으로 실행됨으로써 비동기 작업이 가능한 환경이 됨 