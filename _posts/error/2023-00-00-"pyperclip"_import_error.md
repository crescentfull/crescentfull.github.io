---
title: "pyperclip import 에러"
description: "pyperclip import 에러 해결 방법"
date: 2023-02-07 10:00:00 +0900
categories: [Error]
tags: [Error, pyperclip]
---

# "pyperclip" import 에러

아주 간단한 것이였다.
라이브러리 하나만 딱 설치해주고 import를 해주면 해결이 되는 문제였다.
하지만,,,

![](https://velog.velcdn.com/images/sicksong/post/f506ce48-6cbc-4119-beb4-b488fe7ab3f2/image.png)

what?! why?!?!

## 해결방법 1. 인터프린터 확인 및 VSCode 재실행
✅ 간혹 내가 가상환경 선택을 제대로 안해놓을 때가 있을 것임으로 확인을 한다.
✅ IDE에서 빠르게 인식하지 못하는 경우가 생긴다. 종료 후 재실행을 해보자.


```python
Traceback (most recent call last):
  File "/Users/yeongroksong/Desktop/study/code/TIL/language/python/udemy/session30/Password+Manager/main.py", line 4, in <module>
    import pyperclip
ModuleNotFoundError: No module named 'pyperclip'
```
응^^ 아니야 ^^

## 해결방법 2. pip upgrade 및 pyperclip 재설치

```bash
# upgrade
pip install --upgrade pip
# pyperclip 재설치
pip install pyperclip
pip3 install pyperclip
```
등 구글링해서 찾은 터미널에서 할 수 있는 모든 재설치를 해보았지만 


```python
Traceback (most recent call last):
  File "/Users/yeongroksong/Desktop/study/code/TIL/language/python/udemy/session30/Password+Manager/main.py", line 4, in <module>
    import pyperclip
ModuleNotFoundError: No module named 'pyperclip'
```
  
응^^아니야^^

## 해결방법 3. VSCode 와 python shell 둘다 안되는지 찾기

요즘 계속 IDE를 자주 쓰다보니까 python shell에서도 오류가 나는지 확인해 볼 생각을 미쳐 못했다.

일단 VSCode에서는 에러가 뜬다.

그럼 shell에서는??

```bash
#ipython
❯ ipython
Python 3.9.15 (main, Oct 11 2022, 21:39:54) 
Type 'copyright', 'credits' or 'license' for more information
IPython 7.30.1 -- An enhanced Interactive Python. Type '?' for help.

In [1]: import pyperclip

In [2]: pyperclip.copy('hello')

In [3]: pyperclip.paste()
Out[3]: 'hello'

In [4]: 
?!?!????
```
shell에서는 된다..?! 그런데 VSCode에서는 안된다?!!?

남은건 하나다. python library가 저장 되어있는 **path**를 확인해 주는 것이다.

메인 터미널에서 install한 pip는

```python
# 메인터미널
❯ python -m site                                                                                                                                             
sys.path = [
    '/Users/yeongroksong/Desktop/study/code/TIL/language/python/udemy/session30/Password+Manager',
    '/Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python310.zip',
 ❗️'/Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python3.10',
    '/Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python3.10/lib-dynload',
    '/Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python3.10/site-packages',
]
```


하지만 VSCode에서 설정한 나의 python package는 brew의 python@3.9버전에 있다.

```python
# vscode
❯ pip show pyperclip                                      
Name: pyperclip
Version: 1.8.2
Summary: A cross-platform clipboard module for Python. (Only handles plain text for now.)
Home-page: https://github.com/asweigart/pyperclip
Author: Al Sweigart
Author-email: al@inventwithpython.com
License: BSD
❗️ Location: /opt/homebrew/lib/python3.9/site-packages
Requires: 
Required-by: MouseInfo
```

IDE에서는 3.10을 사용하고 있으니 import가 안되고 있던 것..!
이상하다 잘되다가 왜..?!

파이썬 인터프리터에서 선택하는 가상환경은 경로가 제대로 뜬다.
![](https://velog.velcdn.com/images/sicksong/post/9c17324a-647c-45ca-bf6d-e78fe72a32b8/image.png)

 하지만 인터프리터를 선택하고 나서 확인을 해보니

```bash
❯ pip show pip                                                                          
Name: pip
Version: 22.3.1
Summary: The PyPA recommended tool for installing Python packages.
Home-page: https://pip.pypa.io/
Author: The pip developers
Author-email: distutils-sig@python.org
License: MIT
❗️Location: /opt/homebrew/lib/python3.9/site-packages  >>> location 지정 오류가 있다..!
Requires: 
Required-by: 
```
location 오류,,!

혹시나해서 zshrc 환경과 brew 환경을 비교해보았다.

#oh-my-zsh
![oh-my-zsh/zshrc](https://velog.velcdn.com/images/sicksong/post/8090ffd8-49a7-4fe1-b1f7-842623cc7e9f/image.png)

#homebrew
![brew_config](https://velog.velcdn.com/images/sicksong/post/2cf6e65a-104c-470a-9a4c-2ec03f52fba1/image.png)

응 이둘은 괜찮은 것 같다 ^^

>>> 결론 : VSCode 인터프리터 경로 오류






## ✅해결 과정
일단 수동으로 인식이 되는지 확인을 하기 위해서
~~안되면 아 머리...~~
pyperclip라이브러리를 수동으로 받아 설치해주었다.

```bash
/Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python3.10
폴더에 
다운받은 pyperclip.zip를 풀고 src안의 pyperclip(init.py와 main.py가 있는)폴더를 추가해준다!
```

>결과
![](https://velog.velcdn.com/images/sicksong/post/f1fc35e8-8ba8-4d04-9a57-dd8e9e8f4416/image.png)
모듈 인식이 잘된다!

### 그럼 이제 VSCode와 anaconda를 다시 연결시켰을때 잘되나??

```
❯ pip show pyperclip
Name: pyperclip
Version: 1.8.2
Summary: A cross-platform clipboard module for Python. (Only handles plain text for now.)
Home-page: https://github.com/asweigart/pyperclip
Author: Al Sweigart
Author-email: al@inventwithpython.com
License: BSD
✅Location: /Users/yeongroksong/opt/anaconda3/envs/udemy/lib/python3.10/site-packages
Requires: 
Required-by: 
```
잘 잡네,,?
brew에 있는 python@3.9를 삭제하고 VSCode에서 확인을 하니
```
zsh: /usr/homebrew/bin/python3.9/ bad interpreter : no #$#$#

```
라고 뜨고 다시 아나콘다의 파이썬 폴더를 찾길래 VSCode의 터미널 path를 수정해줘야 하나 하고 열심히 찾다가 컴퓨터 종료하고 다시 일어났더니,,
오류 없이 되는 이유가 뭐죠...?

그래도 요즘 핫한 chatGPT한테 VSCode에서 path 설정 어떻게 바꾸냐고 물어봤다.

![](https://velog.velcdn.com/images/sicksong/post/5d23dfe1-a6ab-499c-bccc-a01959cf92a0/image.png)

,,, 예상보다 상세하게 알려준다.
이러면 stackoverflow나 reddit 등 답변 커뮤니티 사이트의 존망은 어떻게 되는 걸까...

## ✅ 추가: 해결
iterm2에서 zsh 의 path설정을 다시해주니 제대로 운용이 되었다.
1. vi ~/.zshrc
2. export PATH="{/path/to/python}:$PATH" >>> {} 원하는 폴더로 바꿔준다 나같은 경우에는 anaconda3로
3. shouce ~/.zshrc
4. echo $PATH 로 확인

### ❓문제원인
아마 가상환경을 miniconda를 쓰다가 anaconda로 바꾸면서 경로 문제가 생겼던것 같다.
가상환경을 새로 쓸때는 하나를 제대로 삭제하고 다른걸 받자!
힘들어진다..