---
layout: post
categories: posts
title: Coding class
subtitle: This is a test post.
tags: [code post]
date-string: NOVEMBER 25, 2019
---



```python

class point():
    def __init__(self, x,y):
        self.x=x
        self.y=y
    def set(self, x,y):
        self.x=x
        self.y=y
    def get(self):
        return (self.x, self.y)
    def move(self, x1,y1):
        self.x = self.x+x1
        self.y = self.y+y1


a=point(1, 3)
print(a.get())
a.set(1, 3)
print(a.get())
a.move(4, 5)
print(a.get())

```
