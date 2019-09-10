---
layout: post
title: Dockerfile 문법, 내가 놓치고 있던 것들...
tags: [Docker, Container]
---

## Container.. 어디에 활용할까?

요즈음 나는 Docker를 활용하여 개발환경을 구성한다. 기존에는 주로 가상머신(Virtual Machine) 상에 개발환경을 구성하고, 배포역시 VM Image 자체를 배포하는 방식으로 개발을 진행하였으나, 최근  경량화를 무기로 하는 컨테이너로 넘어가는 추세이니, 나도 한번 따라서 넘어가 보기로 했다.(개발자가 유행에 뒤쳐지면 안되지 않곘는가..)

Docker에 개발환경을 구성하기 위해서는, 우선적으로 Docker 명령과 Docker File을 작성하는 방법을 배우는 것으로 활용은 충분해 보인다.(이건 순전히 나의 경험에 의거한 개인적인 의견이다.)

물론 실제 서비스 환경에 적용까지를 고려한다면 Kubernetes와 같은 Ochestration 환경까지의 고려가 필수로 보여지며, 개인적으로는 Kubernetes의 난이도는 Docker에 비해서 공부량과 복잡도가 Docker의 3배정도는 되는 것 같다.

그리고 Docker와 Kubernetes의 명령은 닮은 듯하면서도 상이한 부분이 많아서, 아직도 kubernetes에서 docker의 파라미터를 입력하는 경우가 허다하다. 하지만, 내 머리탓은 절대하지 않고, 나이탓이라고 자조하고 있는 중이다.

## Dockefile 문법을 정리해 보자

"그동안 무엇을 공부했던 거지?"

처음에 이 포스트를 작성할 때는 이미 알고 있는 뻔한 내용은 제외하고, 내가 가진 지식을 한단계 업그레이드하여 정리할 것을 목적으로 작성하였는데, 적다보니 모두 새롭다.

그래도 혹시 처음 Dockerfile을 접하시는 분은 다른 분들이 정리해 놓으신 훌륭한 글이 있으니, 그분들의 글을 보시는 것이 정신건강에 도움이 될 것 같다.(도움이 될 만한 링크는 추후 계속 업데이트하겠습니다.)

우선 내가 이 포스트를 작성하면서 참고한 글의 링크는 아래와 같다.(<http://longbe00.blogspot.com/2015/03/dockerfile.html)>

1. Dockerfile의 기본 명령어는 모두 대문자..
1. Test

FROM, COPY, RUN 등 Dockerfile 작성시에 사용하는 명령어는 모두 대문자를 사용한다.

1. MAINTAINER.

Dockerfile의 작성자 정보를 작성한다. 형식은 자유이고, 보통은 이름과 이메일을 입력한다.

```dockerfile
MAINTAINER Hong, Gildong <gd@yuldo.com>
```
