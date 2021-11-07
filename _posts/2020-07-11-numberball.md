---
layout: post
categories: posts
title: number baseball
subtitle: baseball is fun
tags: [code post]
date-string: JULY 11, 2020
---

```python


import random
randomm=[]
while True:
    c=random.randint(1,9)
    if not c in randomm:
        randomm.append(c)
    if len(randomm)==3:
        break
while True:
    ball=[]
    st=0
    bol=0
    for c in range(1,4):
        ball.append(int(input('{} 번째 숫자를 입력해주세요. : '.format(c))))
    for n in range(len(randomm)):
        for a in range(len(randomm)):
            if ball[a] == randomm[n]:
                st=st+1
        if ball[n] in randomm:
            bol=bol+1
    if bol ==0 and st==0:
        print('아웃입니다. 계속합니다.')
    elif st==3:
        print('3 스트라이크 입니다. 게임을 종료합니다.')
        break
    else :
        print('{} 스트라이크 : {} 볼 입니다. 계속합니다.'.format(st,bol))

    
```
