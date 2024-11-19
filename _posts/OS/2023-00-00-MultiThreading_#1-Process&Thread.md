---
title: "멀티스레딩(Multithreading) #1 - 프로세스와 스레드"
description: "멀티스레딩의 개요, 프로세스와 스레드의 차이점, 프로세스의 개요, 프로세스의 구성, 프로세스 제어 블록, 프로세스 상태"
date: 2023-01-01 10:00:00 +0900
categories: [CS, OS(Operating System)]
tags: [Computer Science, Operating System, Multithreading, Process, Thread]
image:
---

# 멀티스레딩(Multithreading)
>멀티스레딩(multithreading) 
 컴퓨터는 여러 개의 스레드를 효과적으로 실행할 수 있는 하드웨어 지원을 갖추고 있다. 이는 스레드가 모두 같은 주소 공간에서 동작하여 하나의 CPU 캐시 공유 집합과 하나의 변환 색인 버퍼 (TLB)만 있는 멀티프로세서 시스템 (멀티 코어 시스템)과는 구별한다. 그러므로 멀티스레딩은 프로그램 안에서 병렬 처리의 이점을 맛볼 수 있지만 멀티프로세싱 시스템은 여러 개의 프로그램들을 병렬로 처리할 수 있다. 멀티프로세싱 시스템이 여러 개의 완전한 처리 장치들을 포함하는 반면 멀티스레딩은 스레드 수준뿐 아니라 명령어 수준의 병렬 처리에까지 신경을 쓰면서 하나의 코어에 대한 이용성을 증가하는 것에 초점을 두고 있다. [위키백과-멀티스레딩](https://ko.wikipedia.org/wiki/%EB%A9%80%ED%8B%B0%EC%8A%A4%EB%A0%88%EB%94%A9)


## (1) 프로세스
> 프로세스(process)는 컴퓨터에서 연속적으로 실행되고 있는 컴퓨터 프로그램을 말한다. 종종 스케줄링의 대상이 되는 작업(task)이라는 용어와 거의 같은 의미로 쓰인다. 여러 개의 프로세서를 사용하는 것을 멀티프로세싱이라고 하며 같은 시간에 여러 개의 프로그램을 띄우는 시분할 방식을 멀티태스킹이라고 한다. [위키백과-프로세스](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4)
	
### 프로세스의 개요
- 프로세스(process) : 실행 중인 프로그램 
	- 프로그램 : 동작을 하지않는 정적,수동적 개체
    - 프로세스 : 동작을 하는 능동적 개체
- 운영체제로 부터 자원을 할당 받아서 동작한다
	- 자원 : CPU, 메모리, 입출력 장치, 파일 등
    - 동작 : CPU가 프로세스의 명령을 실행
    
### 프로세스의 구성
- 메모리 구조
	- 프로그램 실행에 직접적으로 필요한 코드와 데이터
![](https://velog.velcdn.com/images/sicksong/post/c272378d-6844-496f-a180-6177a6d3f0cf/image.png)

> • 텍스트 섹션 - 실행 가능한 코드
• 데이터 섹션 - 전역 변수
• 힙 섹션 - 프로그램 실행 중 동적으로 할당되는 메모리
• 스택 섹션 - 함수 호출시 일시적인 데이터 저장(함수 매개변수, 반환 주소 및 지역 변수 등)

- 프로세스 제어 블록(Process Control Block: PCB)
	- 운영체제가 프로세스를 관리하기 위해 필요한 정보
    - 각 프로세스 마다 존재
    - 여러 프로세스가 번갈아 실행되는 경우 PCB에 저장된 정보를 활용한다
![](https://velog.velcdn.com/images/sicksong/post/678a8552-8839-4d57-a59a-4c2dac6181d9/image.png)

> • 프로세스 상태(Process state): 상태는 생성(new), 준비(ready), 실행(running), 대기(waiting), 중지(halted) 등이 될 수 있다
• 프로그램 카운터(Program counter):  카운터는 이 프로세스에서 실행될 다음 명령의 주소를 나타낸다
• CPU 레지스터(CPU registers):  레지스터는 컴퓨터 아키텍처에 따라 개수와 유형이 다르다. 누산기(accumulators), 인덱스 레지스터(index registers), 스택 포인터(stack pointers), 범용 레지스터(general-purpose registers) 및 조건 코드 정보 등이 포함된다. 인터럽트가 발생할 때 이 상태 정보는 프로세스를 올바르게 계속 실행할 수 있도록 저장되어야 하며, 다시 실행되도록 예약될 때 이 정보가 필요하다.
• CPU 스케줄링 정보(CPU-scheduling information):  이 정보에는 프로세스 우선 순위, 스케줄링 큐에 대한 포인터 및 기타 스케줄링 매개 변수가 포함된다.(O.S.C 10th chap05)
• 메모리 관리 정보(Memory-management information):  이 정보에는 베이스(base) 및 리미트(limit) 레지스터의 값 및 페이지 테이블 또는 세그먼트 테이블 등, 운영 체제에서 사용하는 메모리 시스템에 따라 다양한 항목이 포함될 수 있다.(O.S.C 10th chap09)
• 회계 정보(Accounting information):  이 정보에는 사용된 CPU 및 실제 시간의 양, 시간 제한, 계정 번호, 작업 또는 프로세스 번호 등이 포함된다.
• I/O 상태 정보(I/O status information):  이 정보에는 프로세스에 할당된 I/O 장치 목록, 열린 파일 목록 등이 포함된다.

- 프로세스 상태
![](https://velog.velcdn.com/images/sicksong/post/eefcb972-a60c-4e70-a29a-f8dc1c75a9d4/image.png)

> • 생성(New): 프로세스가 생성 중
• 실행(Running): 명령이 실행 중
• 대기(Waiting): 프로세스가 어떤 이벤트(예: I/O 완료 또는 신호 수신)가 발생하기를 기다리고 있음
• 준비(Ready): 프로세스가 프로세서에 할당되기를 기다리고 있다.
• 종료(Terminated). 프로세스가 실행 완료.

## (2) 스레드
> 스레드(thread)는 어떠한 프로그램 내에서, 특히 프로세스 내에서 실행되는 흐름의 단위를 말한다. 일반적으로 한 프로그램은 하나의 스레드를 가지고 있지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다. 이러한 실행 방식을 멀티스레드(multithread)라고 한다. [위키백과-스레드](https://ko.wikipedia.org/wiki/%EC%8A%A4%EB%A0%88%EB%93%9C_(%EC%BB%B4%ED%93%A8%ED%8C%85))

- 프로세스 내에 실행 흐름 단위
- 프로세스 자원 사용
!- Stack만 별도 할당, 나머지는 공유(Code, Data, Heap)
- 한 스레드의 결과가 다른 스레드의 영향을 끼친다.
- 동기화 문제는 정말 주의(!디버깅이 어렵다)


---
✅**Reference**
book - Avraham Silberschatz, ⎾Operating System Concepts 10/E⏌, 2019 John Wiley & Sons, Chapter 03-p144