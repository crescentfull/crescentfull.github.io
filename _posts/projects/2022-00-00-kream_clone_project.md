---
title: "Kream 사이트 클론 프로젝트"
description: "Kream 사이트 클론 프로젝트 소개"
date: 2022-01-06 10:00:00 +0900
categories: [Project, Project]
tags: [Django, project, python, mysql, git, backend, frontend]
image: https://user-images.githubusercontent.com/78721108/139569482-db28b424-c233-4df5-9520-4da68e528439.gif
---

#  SHOEKREAM Project

![shokreammain](https://user-images.githubusercontent.com/78721108/139569482-db28b424-c233-4df5-9520-4da68e528439.gif)

## 🎇 팀명 : shoekream - 슈크림

> 의류 경매 서비스를 제공하는 [KREAM](https://kream.co.kr/)을 모티브로 제작하게 된 SHOE-KREAM 팀의 프론트엔드 레포지토리 입니다.
> 짧은 프로젝트 기간동안 개발에 집중해야 하므로 디자인/기획 부분만 클론했습니다.
> 개발은 초기 세팅부터 전부 직접 구현했으며, 백앤드와 연결하여 실제 사용할 수 있는 서비스 수준으로 개발할 수 있도록 2주간 고군분투 하였습니다.

### 프로젝트 선정이유
- 조사결과, 해당 사이트의 경매 입찰 기능과 결제 플로우, 차트 구현, 상품리스트 필터 구현 등 배울 점이 많다고 판단하여 선정하게 되었습니다.

## 📅 개발 기간 및 개발 인원

- 개발 기간 : 2021/10/18 ~ 2021/10/29
- 개발 인원 <br/>
 👨‍👧‍👦 **Front-End** 4명 : 김현진, 박산성, 이선호, 하상영<br/>
 👨‍👧‍👦 **Back-End** 3명 : 박치훈, 양가현, 송영록<br/>
- [Back-end github 링크](https://github.com/wecode-bootcamp-korea/25-2nd-SUNKREAM-backend)

## 🎬 프로젝트 구현 영상

- 🔗 [구현영상] : https://youtu.be/N63MUdDmDFI

## ⛓️ 적용 기술
- **Front-End** : HTML5, CSS3, React, SASS, JSX
- **Back-End** : Python, Django, MySQL, jwt, bcypt, AWS RDS, AWS EC2
- **Common** : Git, Github, Slack, Trello, Postman or Insomnia

## ⚙️ 데이터베이스 Diagram
![kream](https://user-images.githubusercontent.com/78721108/139569506-39104ecf-7060-4aa0-8d45-c834bc1a4174.png)

## 💻 구현 기능

Endpoint documentation - [Postman API](https://documenter.getpostman.com/view/17773566/2s7ZE5r4jy)

### BACKEND
> 로그인 구현

```python
import jwt
import requests
from json.decoder import JSONDecodeError

from django.http  import JsonResponse
from django.views import View

from users.models import User
from my_settings  import SECRET_KEY, ALGORITHMS

class KakaoLogin(View):
    def get(self, request):
        try: 
            token = request.headers.get('Authorization')

            if token == None:
                return JsonResponse({'messsage': 'INVALID_TOKEN'}, status=401)

            kakao_account = requests.get('https://kapi.kakao.com/v2/user/me', headers = {'Authorization': f'Bearer {token}'}).json()
            print('::::kakao_account:', kakao_account)

            if not User.objects.filter(kakao_id=kakao_account['id']).exists():
                user = User.objects.create(
                    kakao_id = kakao_account['id'],
                    email    = kakao_account['kakao_account']['email'],
                    name     = kakao_account['kakao_account']['profile']['nickname']
                )
            user = User.objects.get(kakao_id=kakao_account['id'])

            access_token = jwt.encode({'user_id': user.id}, SECRET_KEY, algorithm=ALGORITHMS)

            return JsonResponse({'access_token': access_token}, status=201)

        except KeyError:
            return JsonResponse({'message': 'KEY_ERROR'}, status=400)
        
        except JSONDecodeError:
            return JsonResponse({'message': 'JSON_DECODE_ERROR'}, status=400)

        except jwt.DecodeError:
            return JsonResponse({'message': 'JWT_DECODE_ERROR'}, status=400)

        except ConnectionError:
            return JsonResponse({'message': 'CONNECTION_ERROR'}, status=400)
```
- 카카오 연동 로그인 구현
- ```Authorization``` 헤더에서 카카오 엑세스 토큰을 받아옴
- ```request.get()``` 함수를 통해 카카오 API 엔드포인트로 사용자 정보 요청, json 형식으로 받아옴
- 카카오 아이디로 사용자 조회, 없으면 새로운 유저 생성
- 사용자 정보에서 ```user모델```의 ```id```를 이용하여 jwt 토큰 발급
- ```jwt.encode()```함수에 ```SECRET_KEY```와 ```ALGORITHMS```를 활용하여 JWT 토큰 생성 후 반환 
- 프론트엔드에 발상할 오류의 종류를 예외처리하여 명확히 명시

> 로그인 데코레이터 구현

```python
import json
import jwt

from django.http  import JsonResponse

from my_settings  import SECRET_KEY, ALGORITHMS
from users.models import User

def login_decorator(func):
    def wrapper(self, request, *args, **kwargs):
        if 'Authorization' not in request.headers : 
            return JsonResponse ({'message' : 'UNAUTHORIZED'}, status=401)

        access_token = request.headers.get('Authorization')
        
        try:
            payload      = jwt.decode(access_token, SECRET_KEY, algorithms=ALGORITHMS)
            user         = User.objects.get(id=payload['user_id'])
            request.user = user

        except jwt.exceptions.DecodeError:
            return JsonResponse({'MESSAGE': 'INVALID_TOKEN'}, status=401)

        except User.DoesNotExist:
            return JsonResponse({'MESSAGE': 'INVALID_USER'}, status=401)

        return func(self, request,  *args, **kwargs)

    return wrapper
```
- python decorator 활용
- 로그인 성공 시 토큰 발급(jwt)
- 로그인 실패 시 예외처리
- jwt.decode() 함수를 통해 토큰 검증
- 디코딩한 토큰에서 user_id 추출, user모델에서 해당 user_id 조회 후 가져옴
- 추출한 사용자를 request.user에 저장, 이후 로그인 상태 유지

> 상세 페이지

```python
class DetailProductView(View) :
    def get(self, request, product_id) :
        try :
               
            buy_price_filter  = (Q(productsize__bidding__bidding_position_id = 2) & Q(productsize__bidding__bidding_status_id = 1))
            sell_price_filter = (Q(productsize__bidding__bidding_position_id = 1) & Q(productsize__bidding__bidding_status_id = 1))

            if not Product.objects.filter(id = product_id).exists():
                return JsonResponse({'MESSAGE':'product_id_not_exist'}, status = 404)
            
            product  = Product.objects\
                                    .annotate(buy_price = Min('productsize__bidding__price', filter = buy_price_filter), 
                                            sell_price = Max('productsize__bidding__price', filter = sell_price_filter))\
                                    .get(id = product_id)
                                                    
            orders   = Order.objects.\
                                    filter(bidding__product_size__product_id = product_id).\
                                    order_by('-created_at')

            wishlist = len(Wishlist.objects.filter(id=product_id).all())
            
            product_detail = [{
                                'product_id'      : product.id,
                                'name'            : product.name,
                                'brand_name'      : product.brand.name,
                                'release_price'   : product.release_price,
                                'model_number'    : product.model_number,
                                'image_list'      : [image.image_url for image in product.productimage_set.all()],
                                'recent_price'    : orders.first().bidding.price if orders.exists() else None,
                                'buy_price'       : product.buy_price,
                                'sell_price'      : product.sell_price,
                                'total_wishlist'  : wishlist,
                            }]

            return JsonResponse({'product_detail' : product_detail}, status=200)

        except AttributeError as e :
            return JsonResponse({'message' : f'{e}'}, status=400)        
        except TypeError as e :
            return JsonResponse({'message' : f'{e}'}, status=400)
```

- 필터 조건 정의: 구매 입찰 정보중 입찰 상태가 유효한 조건(입찰 포지션 2, 상태가 1인 경우)과 판매 입찰 정보 중 입찰 상태가 유효한 조건(입찰 포지션 1, 상태가 1인 경우)
- 효율적인 데이터 처리와 코드의 간결성을 위하여 annotate()함수 활용
- 데이터베이스 단계서 집계 함수(Min, Max)를 활용하여 필요한 값을 미리 계산
- 

> 상품 리스트 출력 API(ProductView)

````python

````

> 브랜드 리스트 출력 API(BrandView)
```python
class BrandView(View):
    def get(self, request):
            brands = Brand.objects.all()

            brand_list = [{
                        'brand_id'   : brand.id,
                        'brand_name' : brand.name,
                    } for brand in brands]
            return JsonResponse({'brand_list' : brand_list}, status = 200)
```
- 메인 페이지, 제품 페이지 상단 브랜드 출력을 위하여 API 작성


## ❗ Reference
- 이 프로젝트는 [KREAM](https://kream.co.kr/) 사이트를 참조하여 학습목적으로 만들었습니다.
- 실무수준의 프로젝트이지만 학습용으로 만들었기 때문에 이 코드를 활용하여 이득을 취하거나 무단 배포할 경우 법적으로 문제될 수 있습니다.
- 이 프로젝트에서 사용하고 있는 사진 모두는 copyright free 사이트들의 이미지들을 취합 및 canva 에서 직접 제작한 이미지들로 제작되었습니다.
