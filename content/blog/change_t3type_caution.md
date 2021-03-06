---
title: "인스턴스 t3타입으로 변경시 주의 사항"
date: 2021-10-21T23:57:49+09:00
slug: ""
description: ""
keywords: []
draft: true
tags: ["Cloud","AWS"]
math: false
toc: false
---


AWS를 운영하다 보면 될 수 있음 비용을 줄이기를 바랄 것이다. 

특히나 인스턴스는 비용의 많은 부분을 차지하고 있길래 비용 최적화에 많이 언급된다.

(물론 RDS를 쓰거나 하면 다르겠지만) 

저렴한 스펙을 찾다보면 자주 언급되는 인스턴스 유형이  t쪽이다. 

범용으로 사용하기에 무난한 용도로 사용된다. 문제는 t3가 가지고 있는 특징이다.

T3는 다른 인스턴스와는 다르게 순간확장 기능을 사용한다. 이 순간 확장이란 기본 사용률에서 초과 될 경우 순간적으로 CPU 사용률 확장하여 사용할 수 있는 개념이다. 다만 무조건적으로 확장을 하는건 아니고 크레딧을 가지고 있을 경우에만 확장이 가능하다.

이 크레딧은 기본 사용률 밑으로 CPU를 사용할시 축적할 수 있다. 

즉 CPU 사용량이 낮을때는 크레딧을 모았다고 CPU를 많이 사용하게 되는 경우가 오면 크레딧을 써가면서 CPU를 확장 하는 것이다. 

[CPU credits and baseline utilization for burstable performance instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-credits-baseline-concepts.html#earning-CPU-credits)

그럼 가장 중요하게 봐야할 부분이 기본 사용률이다. 위에 링크를 보면 인스턴스 유형마다 기본 사용률이 다르다. 

이 기본 사용률이 자주 넘는다면 t3가 적합하지 않는 것이고 평소에는 거의 사용하지 않는 다면 t3가 적합할 수도 있다. 

그렇다고 사용률만으로 바로 바꿔도 되는 것은 아니다. 기존에 사용하고 있던 타입대비 클럭이 낮거나 하는 경우도 있기 때문이다. 

그래서 반드시 한 번 테스트를 해야하는 부분이 있다.

결론: 기본사용률 확인 후 기본 CPU 클럭 수 변동 여부 등을 고려해서 테스트 후 RI 구매