---
layout: post
categories: posts
title: randonmunber
subtitle: 비슷한 숫자 모으기
tags: [code post]
date-string: MAY 15, 2020
---

```python
import operator
import random
d=0
n=dict()
a=[]
def number(a):
    b=set(a)
    for c in b:
        d=a.count(c)
        n[c]=d
    g = sorted(n.items(), key=operator.itemgetter(1), reverse=True)
    d=int(g[0][1])-int(g[-1][1])
    return str(d) + str(g)
while len(a)<10:
    a.append(random.randint(1,5))
print(number(a))
```

1~5까지의 숫자 10개를 랜덤하게 리스트에 넣어 제일 많이 나온 수의 갯수와 적게 나온 수의 갯수의 차를 구하는 간단한 코드
