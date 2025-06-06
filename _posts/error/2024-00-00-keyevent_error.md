---
title: "채팅창에서 한글 입력시에 enter키 누르면 마지막 문자 두번 입력되는 상황"
description: "keydown 이벤트 오류"
date: 2024-11-20 10:00:00 +0900
categories: [Error]
tags: [Error, javascript, keyboardEvent]
---

이유 : 한글 입력 시 마지막 글자가 두 번 입력되는 문제는 **keydown 이벤트**가 조합 중인 문자를 처리하기 때문이다. 
 이를 해결하기 위해 KeyboardEvent.isComposing 속성을 사용하여 조합 중인 문자가 아닌 경우에만 메시지를 전송하도록 할 수 있다.
 
```javascript
        // Enter 키를 눌러 메시지를 전송하는 함수
        document.getElementById('content').addEventListener('keydown', function(event) {
            if (event.key === 'Enter' && !event.shiftKey && !event.isComposing) {
                event.preventDefault(); // 기본 Enter 키 동작(줄 바꿈)을 방지
                setTimeout(sendMessage, 0); // 메시지 전송 함수 호출을 약간 지연
            }
        });
```


[출처](https://na0i.tistory.com/entry/%ED%95%9C%EA%B8%80-%EB%91%90%EB%B2%88%EC%94%A9-%EC%9E%85%EB%A0%A5%EB%90%98%EB%8A%94-%EC%98%A4%EB%A5%98-%ED%95%B4%EA%B2%B0%ED%95%98%EA%B8%B0keydown-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EC%8B%9C-%EB%81%9D%EA%B8%80%EC%9E%90%EA%B0%80-%EB%91%90%EB%B2%88-%EB%B0%98%EB%B3%B5%EB%90%98%EB%8A%94-%ED%98%84%EC%83%81)