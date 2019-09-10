---
layout: post
title: Dockerfile 문법, 내가 놓치고 있던 것들...
tags: [Docker, Container]
---

# Container.. 어디에 활용할까?

요즈음 나는 Docker를 활용하여 개발환경을 구성한다. 기존에는 주로 가상머신(Virtual Machine) 상에 개발환경을 구성하고, 배포역시 VM Image 자체를 배포하는 방식으로 개발을 진행하였으나, 최근  경량화를 무기로 하는 컨테이너로 넘어가는 추세이니, 나도 한번 따라서 넘어가 보기로 했다.(개발자가 유행에 뒤쳐지면 안되지 않곘는가..)

Docker에 개발환경을 구성하기 위해서는, 우선적으로 Docker 명령과 Docker File을 작성하는 방법을 배우는 것으로 활용은 충분해 보인다.(이건 순전히 나의 경험에 의거한 개인적인 의견이다.)

물론 실제 서비스 환경에 적용까지를 고려한다면 Kubernetes와 같은 Ochestration 환경까지의 고려가 필수로 보여지며, 개인적으로는 Kubernetes의 난이도는 Docker에 비해서 공부량과 복잡도가 Docker의 3배정도는 되는 것 같다.

그리고 Docker와 Kubernetes의 명령은 닮은 듯하면서도 상이한 부분이 많아서, 아직도 kubernetes에서 docker의 파라미터를 입력하는 경우가 허다하다. 하지만, 내 머리탓은 절대하지 않고, 나이탓이라고 자조하고 있는 중이다.


# Dockefile 문법을 정리해 보자..
