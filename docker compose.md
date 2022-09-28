# docker compose

### Docker Compose ?

- `docker compose`는 다중 컨테이너 도커 애플리케이션을 정의하고 실행하기 위한 도구
    - 즉 멀티 컨테이너 상황에서 쉽게 네트워크를 연결시켜주기 위해서 `Docker Compose`를 이용

### 기존 컨테이너 사용시 문제점

![Untitled](https://user-images.githubusercontent.com/106054507/192779177-eb3df3a8-173a-40f9-9b4a-5e9b9f9eb1d4.png)

```bash
(node:1) UnhandledPromiseRejectionWarning: Error: Connection timeout
    at Socket.<anonymous> (/usr/src/app/node_modules/@redis/client/dist/lib/client/socket.js:165:124)
    at Object.onceWrapper (events.js:519:28)
    at Socket.emit (events.js:400:28)
    at Socket._onTimeout (net.js:495:8)
    at listOnTimeout (internal/timers.js:557:17)
    at processTimers (internal/timers.js:500:7)
(Use `node --trace-warnings ...` to show where the warning was created)
(node:1) UnhandledPromiseRejectionWarning: Unhandled promise rejection. This error originated either by throwing inside of an async function without a catch block, or by rejecting a promise which was not handled 
with .catch(). To terminate the node process on unhandled promise rejection, use the CLI flag `--unhandled-rejections=strict` (see https://nodejs.org/api/cli.html#cli_unhandled_rejections_mode). (rejection id: 1)(node:1) [DEP0018] DeprecationWarning: Unhandled promise rejections are deprecated. In the future, promise rejections that are not handled will terminate the Node.js process with a non-zero exit code.
PS C:\Users\User\Desktop\docker\code\docker-compose-app> docker-compose up
```

- 기존에는 각 컨테이너를 실행하고 컨테이너끼리 연결을 위해 사용하려고 하면 **해당 오류 메시지가 검출**
    - 즉, 서로 다른 컨테이너에 있는데 이렇게 **컨테이너 사이에는 아무런 설정 없이는 접근을 할 수 없기**에 `노드 JS 앱`에서 `레디스 서버`에 접근없음.

### 도커 Compose 파일 작성
![Untitled 1](https://user-images.githubusercontent.com/106054507/192779193-1b7f5139-0bce-485b-8224-e6cae0b69a71.png)

```yaml

version: '3'
services:
  redis-server:
    image: "redis"
  node-app: 
    build: .
    ports:
     - "5000:8080"
```

- **version** : 도커 컴포즈 버전
- **services** : 실행하려는 컨테이너 설정
- **redis-server** : 컨테이너 이름
- **images** : 컨테이너에서 사용하는 이미지
- **node-app**  : 컨테이너 이름
- **build** : 디렉토리에 있는 Dockerfile 사용
- **ports** : 포트 매핑

### docker compose 실행

- **docker-compose build :** 이미지를 빌드하기만 하며, 컨테이너를 시작하지는 않음
- **docker-compose up :**  이미지가 존재하지 않을 경우에만 빌드하며, 컨테이너를 시작
- **docker-compose up --build** : 필요치 않을때도 강제로 이미지를 빌드하며, 컨테이너를 시작
- **docker-compose up --no-build** : 이미지 빌드 없이, 컨테이너를 시작 (이미지가 없을시 실패)
