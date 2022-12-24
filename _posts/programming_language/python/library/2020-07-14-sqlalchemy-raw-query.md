---
layout: post
title:  "SQLAlchemy에서 실제 생성된 Query가 보고 싶을 경우"
comments: true
categories: [Programming Language, Python, Library, SQLAlchemy, Database]
shortinfo: "테스트 또는 성능 확인을 위해 SQLAlchemy에서 만들어진 Query를 보고 싶을 경우의 방법이다"
tags: [Programming Language, Python, ORM, Database]
---

```python
from sqlalchemy.sql import table, column, select

t = table('t', column('x'))

s = select([t]).where(t.c.x == 5)

# compile_kwargs에 해당 parameter를 넘겨주지 않을 경우, 일부 변수에 대해 치환되지 않은 Query를 Return 한다
print(s.compile(compile_kwargs={"literal_binds": True}))

```

### 참조 링크

[SQLAlchemy: print the actual query](https://stackoverflow.com/questions/5631078/sqlalchemy-print-the-actual-query)
