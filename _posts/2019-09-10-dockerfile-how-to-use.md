---
layout: post
title: Dockerfile 문법, 내가 놓치고 있던 것들...
color: brown
tags: [Docker, Container]
---

## Container 어디에 활용할까?

요즈음 나는 Docker를 활용하여 개발환경을 구성한다. 기존에는 주로 가상머신(Virtual Machine) 상에 개발환경을 구성하고, 배포역시 VM Image 자체를 배포하는 방식으로 개발을 진행하였으나, 최근  경량화를 무기로 하는 컨테이너로 넘어가는 추세이니, 나도 한번 따라서 넘어가 보기로 했다.(개발자가 유행에 뒤쳐지면 안되지 않곘는가..)

Docker에 개발환경을 구성하기 위해서는, 우선적으로 Docker 명령과 Docker File을 작성하는 방법을 배우는 것으로 활용은 충분해 보인다.(이건 순전히 나의 경험에 의거한 개인적인 의견이다.)

물론 실제 서비스 환경에 적용까지를 고려한다면 Kubernetes와 같은 Ochestration 환경까지의 고려가 필수로 보여지며, 개인적으로는 Kubernetes의 난이도는 Docker에 비해서 공부량과 복잡도가 Docker의 3배정도는 되는 것 같다.

그리고 Docker와 Kubernetes의 명령은 닮은 듯하면서도 상이한 부분이 많아서, 아직도 kubernetes에서 docker의 파라미터를 입력하는 경우가 허다하다. 하지만, 내 머리탓은 절대하지 않고, 나이탓이라고 자조하고 있는 중이다.

## Dockefile 문법을 정리해 보자

"그동안 무엇을 공부했던 거지?"

처음에 이 포스트를 작성할 때는 이미 알고 있는 뻔한 내용은 제외하고, 내가 가진 지식을 한단계 업그레이드하여 정리할 것을 목적으로 작성하였는데, 적다보니 모두 새롭다.

그래도 혹시 처음 Dockerfile을 접하시는 분은 다른 분들이 정리해 놓으신 훌륭한 글이 있으니, 그분들의 글을 보시는 것이 정신건강에 도움이 될 것 같다.(도움이 될 만한 링크는 추후 계속 업데이트하겠습니다.)

우선 내가 이 포스트를 작성하면서 참고한 글의 링크는 아래와 같다.(<http://longbe00.blogspot.com/2015/03/dockerfile.html>)

1. Dockerfile의 기본 명령어는 모두 대문자

    * FROM, COPY, RUN 등 Dockerfile 작성시에 사용하는 명령어는 모두 대문자를 사용한다.

2. MAINTAINER

    * Dockerfile의 작성자 정보를 작성한다. 형식은 자유이고, 보통은 이름과 이메일을 입력한다.

    ```dockerfile
    MAINTAINER Hong, Gildong <gd@yuldo.com>
    ```

3. RUN
    * dockerfile에서 RUN 명령은 워낙 처음에 나오는 명령이라, '이정도는 알고 있지'라고 생각했는데, shell 상에서 실행시키는 방법과 shell 없이 실행시키는 방법 2가지가 있다고 한다.
    * RUN으로 실행된 결과는 캐싱되며, 다음 빌드(Build)시에 캐싱된 결과를 사용하지 않으려면 ```docker build```명령에서 ```--no-cache``` 옵션을 사용하면 된다.

    * 쉘(/bin/sh)로 명령 실행하기
        : 쉘스크립트 구문을 사용가능

    ```dockerfile
    RUN apt-get install -y nginx
    RUN echo "Hello Docker" > /tmp/hello
    RUN curl -ssl https://golang.org/dl/go1.3.1.src.tar.gz | tar -v -C /usr/local -xz
    RUN git clone https://github.com/docker/docker.git
    ```

    * 쉘없이 바로 실행하기
        : 쉘스크립트 문법과 관련된 문자를 그대로 실행파일에 넘겨주는 것이 가능

    ```dockerfile
    RUN ["apt-get", "install", "-y", "nginx"]
    RUN ["/usr/local/bin/hello", "--help"]
    ```

4. CMD
    * CMD는 컨테이너가 실행될 때 수행되는 스크립트 혹은 명령이다. 즉 ```docker run```명령으로 컨테이너를 생성하거나, ```docker start```명령으로 정지된 컨테이너를 기동할 때 수행된다. CMD는 Dockerfile에서 한번만 사용할 수 있다.
    * CMD는 역시 쉘에서 명령을 실행하는 방법과 쉡없이 바로 실행하는 방법을 제공한다.
    * CMD는 ENTRYPOINT에 설정한 명령에 매개변수를 전달하여 실행하도록 할 수도 있다. Dockerfile에 ENTRYPOINT가 있으면 CMD는 ENTRYPOINT에 매개변수만 전달하는 역할을 수행한다.

    ```dockerfile
    # 쉘에서 명령실행하기
    CMD touch /home/hello/hello.txt
    ```

    ```dockerfile
    # 쉘없이 바로 명령실행하기
    CMD ["redis-server"]
    ```

    ```dockerfile
    # ENTRYPOINT를 사용하는 경우
    ENTRYPOINT ["echo"]
    CMD ["hello"]
    ```

5. EXPOSE
    * EXPOSE는 호스트와 연결할 포트번호를 설정한다.(```docker run --expose```옵션과 동일)
    * EXPOSE 하나로 복수의 포트를 동시에 설정할 수 있다.
    * EXPOSE는 호스트와 연결만 할 뿐 외부에 노출이 되는 포트는 아니다. 포트를 외부에 노출하려면 ```docker run```명령의 -p 옵션을 사용해야 한다.

    ```dockerfile
    EXPOSE 80 443
    ```

6. WORKDIR
    * RUN, CMD, ENTRYPOINT에서 설정한 실행파일이 실행될 디렉터리를 지정한다.

    ```dockerfile
    WORKDIR /path/to/workdir
    ```

7. ENV
    * ENV는 환경변수를 설정한다. ENV로 설정한 환경변수는 RUN, CMD, ENTRYPOINT에 적용된다.
    * ENV는 ```docker run```의 -e 옵션으로도 설정할 수 있다.

    ```dockerfile
    ENV GOPATH /go
    ENV PATH /go/bin:$PATH # 환경변수를 사용시 $를 사용하는 것은 동일
    ```

    ```bash
    sudo docker run -e GOPATH=/go example
    ```

8. ADD, COPY
    * ADD, COPY 명령은 파일을 이미지에 추가하는 명령이다.
    * ADD, COPY 명령 다음에는 <소스파일경로> <타겟경로>가 제공되어야 하고, 소스파일 경로는 컨텍스트(Dockerfile이 존재하는 위치)를 기준으로 하위경로만을 지정할 수 있으며 상위 디렉터리 또는 절대경로를 사용할 수 없다. 반면, 타겟경로는 항상 절대경로로 설정한다.
    * ADD와 COPY 명령의 차이는 ADD 명령의 경우 압축파일 해제 후 복사, 또는 URL로 부터 파일을 다운로드하여 복사가 가능하지만, COPY 명령의 경우 압축파일 그대로 복사하고, URL에서 파일다운로드 후 복사를 지원하지 않는다.
    * 디렉터리를 복사하는 경우 .dockerignore 파일에 설정한 파일과 디렉터리는 제외된다.

    ```dockerfile
    ADD hello-entrypoint.sh /entrypoint.sh
    ADD http://example.com/hello.txt /
    ADD zlib-1.2.8.tar.gz /lib/ # 압축이 해제되어 복사..
    ADD *.txt /root/
    ```

    ```dockerfile
    COPY hello-entrypoint.sh /entrypoint.sh
    COPY hello-dir /hello-dir
    COPY zlib-1.2.8.tar.gz /lib/ # 압축을 해제하지 않고 압축파일 그대로 복사
    COPY *.txt /root/
    ```

9. VOLUME
    * VOLUME은 디렉토리의 내용을 컨테이너에 저장하지 않고 호스트에 저장하도록 설정한다. 이는 컨테이너의 특정볼륨을 호스트의 특정 디렉터리와 연결하는 ```docker run -v```옵션 명령과는 다르며, 컨테이너가 종료되더라도 저장된 파일을 계속 유지하여야 하는 경우 사용한다.(예를 들면, 컨테이너가 재기동되는 경우 이전에 저장된 파일을 계속 사용해야 하는 경우)

    ```dockerfile
    VOLUME /data
    VOLUME ["/data", "/var/log/hello"]
    ```

10. USER
    * RUN, CMD, ENTRYPOINT 명령을 실행할 사용자 계정을 설정한다.
    * 중간에 USER를 다시 지정하는 경우 이후 실행되는 CMD, RUN, ENTRYPOINT 명령은 변경된 USER 계정으로 실행하게 된다.

    ```dockerfile
    USER nobody
    ```

11. ONBUILD
    * ONBUILD 명령을 포함하는 dockerfile 빌드시에는 수행되지 않고, 생성된 이미지 기반으로 다른 이미지가 생성될 때 명령이 실행된다.(즉, 다음번 이미지가 FROM으로 사용될 때 실행할 명령을 예약하는 기능)
    * ONBUILD는 이미지를 생성 후에 해당 이미지를 기반으로 커스터마이징을 할 때 활용할 수 있다.(단, 바로 아래 자식 이미지를 생성할 때만 적용되고 손자 이미지에는 적용되지 않음)

    ```dockerfile
    FROM ubuntu:latest
    ONBUILD RUN touch /hello.txt
    ```

----
