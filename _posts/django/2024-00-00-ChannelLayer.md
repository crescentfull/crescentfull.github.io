---
title: "Django Channel Layer"
description: "Django Channel Layer란 뭐냐"
date: 2024-10-04 10:00:00 +0900
categories: [Django]
tags: [Django, ChannelLayer]
---


# Channel Layer
- Django Channels의 핵심 개념 중 하나이다.
- 애플리케이션 인스턴스 간의 메시지를 전송(Redis, RabbitMQ 등 브로커 이용)하고, 이를 통해 비동기 통신을 가능하게 하는 추상화된 메시징 시스템.
WebSocket, 채팅, 알림 시스템 등의 비동기 작업에서 주로 사용된다.
- 그룹 통신을 지원

## 구성요소
channel, Group, Backend
- Channel: 메세지를 전송하고 수신할 수 있는 이름 기반 큐, 각 채널은 메세지를 수신하는 consumer와 연결
- Group: 다수의 채널을 묶어서 그룹화, 한 그룹에 메세지 전송
- Backend: 메세지를 전송하고 수신하는 백엔드 시스템, 메세지 브로커와 통신하는 역할

## 코드 구현
1. 설정

```python
# settings.py
CHANNEL_LAYERS = {
    'default': {
        'BACKEND': 'channels.layers.InMemoryChannelLayer',
        'CONFIG': {
            'hosts': [('127.0.0.1', 6379)], # Redis 호스트와 포트
        },
    },
}
```

2. 메세지 전송
```python
from channels.layers import get_channel_layer
from asgiref.sync import async_to_sync

# 채널 레이어 가져오기
channel_layer = get_channel_layer()

# 특정 그룹으로 메시지 전송
async_to_sync(channel_layer.group_send)(
    "chat_group",  # 그룹 이름
    {
        "type": "chat.message",  # 호출될 핸들러 이름
        "message": "Hello, world!",  # 전송할 데이터
    },
)

```

3. 메세지 수신: 메세지 처리를 위해서 Consumer.py 에서 핸들러 구성
```python
# consumers.py
from channels.generic.websocket import AsyncWebsocketConsumer
import json

class ChatConsumer(AsyncWebsocketConsumer):
    async def connect(self):
        self.group_name = "chat_group"

        # 그룹에 추가
        await self.channel_layer.group_add(
            self.group_name,
            self.channel_name
        )
        await self.accept()

    async def disconnect(self, close_code):
        # 그룹에서 제거
        await self.channel_layer.group_discard(
            self.group_name,
            self.channel_name
        )

    # 그룹 메시지 핸들러
    async def chat_message(self, event):
        message = event["message"]

        # 클라이언트에 메시지 전송
        await self.send(text_data=json.dumps({
            "message": message
        }))

```
>  전송할 데이터는 JSON 직렬화 가능한 데이터여야만 한다.

## 채널 레이어로 전송할 수 있는 데이터 타입은?
`msgpack`에서 지원하는 타입만 지원 가능.

> msgpac? </br>
Django Channels의 채널 레이어에서 사용하는 데이터 직렬화 포맷 중 하나</br>
이는 데이터를 바이너리 포맷으로 직렬화하여 저장하거나 전송하기 위한 포맷으로, MessagePack 라이브러리를 사용</br>
redis를 사용할 경우 msgpack을 통해 메시지를 직렬화하고 역직렬화


- `dict`, `list`, `str`, `int`, `float`, `bool`, `None`
- 전송할 데이터는 크기 제한이 있음, Redis 기준 최대 문자열 512MB

