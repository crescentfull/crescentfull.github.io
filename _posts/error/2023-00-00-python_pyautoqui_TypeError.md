---
title: "python pyautoqui로 화면 캡쳐 프로그램을 만드는 와중에 typeerror 발생"
description: "python pyautoqui로 화면 캡쳐 프로그램을 만드는 와중에 typeerror 발생"
date: 2023-06-29 10:00:00 +0900
categories: [Error]
tags: [Error, python, pyautoqui]
---

### python  pyautoqui로 화면 캡쳐 프로그램을 만드는 와중에 typeerror 발생

```python
  File "/path/to/venv/lib/python3.11/site-packages/pyscreeze/__init__.py", line 527, in _screenshot_osx
    if tuple(PIL__version__) < (6, 2, 1):

TypeError: '<' not supported between instances of 'str' and 'int'
```

pyautoqui 소스코드에서 ```def _screenshot_osx(imageFilename=None, region=None):``` 함수 부분의 실행 첫단의 if문이 오류가 났다.

PIL version을 판별해주는 version은 6.2.1처럼 .과 같이 작성 되어있다.

'.'을 기준으로 split해주고 반복숫자를 map으로 처리해준후 tuple로 변환

```python
if tuple(PIL__version__) < (6, 2, 1):

--->

if tuple(map(int, PIL__version__.split("."))) < (6, 2, 1):


```

실행 후 오류해결 완료

출처 : https://stackoverflow.com/questions/76361049/how-to-fix-typeerror-not-supported-between-instances-of-str-and-int-wh