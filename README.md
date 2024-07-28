# 🐳 Janus Gateway Docker로 시작하기

**Janus Gateway**는 많은 의존 라이브러리를 요구하기 때문에 설치하기 까다로운 면이 있습니다.
<br />
[Canyan.io](https://github.com/canyanio)에서 제공하는 도커 이미지를 사용하면 쉽게 설치할 수 있습니다.

<br />

## 🚀 시작하기

이 레포지토리는 최소한의 기본 설정으로 세팅되어 있습니다.
<br />
플러그인들의 추가 설정이 필요하다면 설정 파일을 추가하고 해당 파일을 `docker-compose`에 있는 `volumes`에 추가합니다.
<br />
필요한 설정 파일은 [여기에](https://github.com/meetecho/janus-gateway/tree/master/conf) 정리되어 있습니다.

> ⚠️ `Docker`가 설치되어야 실행이 가능합니다.

1. 레포지토리 클론 및 디렉터리 이동

```shell
git clone https://github.com/chan9yu/janus-gateway-docker
cd janus-gateway-docker
```

2. 도커 이미지 다운로드

```shell
docker pull canyan/janus-gateway:latest
```

3. docker-compose 확인

```yaml
version: "3.8"
services:
  janus-gateway:
    image: "canyan/janus-gateway:latest"
    command: ["/usr/local/bin/janus", "-F", "/usr/local/etc/janus"]
    ports:
      - "8188:8188"
      - "8088:8088"
      - "8089:8089"
      - "8889:8889"
      - "8000:8000"
      - "7088:7088"
      - "7089:7089"
    volumes:
      - "./janus/janus.jcfg:/usr/local/etc/janus/janus.jcfg"
      - "./janus/janus.plugin.videoroom.jcfg:/usr/local/etc/janus/janus.plugin.videoroom.jcfg"
      - "./janus/janus.transport.websockets.jcfg:/usr/local/etc/janus/janus.transport.websockets.jcfg"

  web_server:
    user: "root"
    image: httpd:alpine
    ports:
      - 80:80
    volumes:
      - ./html:/usr/local/apache2/htdocs
```

4. 도커 실행

```shell
docker-compose up --build -d
```

성공적으로 실행했다면 web_server에 설정한 80포트, 즉 http://localhost 에 접속하면 Janus 데모 페이지가 실행됩니다.
<br />
동시에 WebSocket도 설정된 8188 포트로 사용 가능합니다.

<br />

## 🔗 참고 링크

- [GitHub](https://github.com/canyanio/janus-gateway-docker)
- [Docker Hub](https://hub.docker.com/r/canyan/janus-gateway)
