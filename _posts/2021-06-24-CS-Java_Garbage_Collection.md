---
title: JAVA_CS-Garbage Collection에 대해서 정리
author: 강민석
date: 2021-06-24 01:34:50 +0800
categories: [Java,CS]
tags: [JAVA]
math: true
mermaid: true
image: 
comments: true
---
 

2011.12.22 Naver D2글

Java Garbage Collection 에 대해서 - <[https://d2.naver.com/h>elloworld/1329](https://d2.naver.com/helloworld/1329)>

을 제 입 맛대로 정리한 글입니다.

모든 사진에 대한 출처는 <https://d2.naver.com/helloworld/1329>

-----

글을 쓴 이유 : 주관적인 판단으로 GC에 대해 잘 알고 있어야 좋은 Java 개발자라고 생각하심.

### 가비지 컬렉션 과정 - Cenertional Garbage Collection

- stop-the-world : GC를 실행하기 위해 JVM이 어플리케이션 실행을 멈추는 것. 즉, GC를 실행하는 쓰레드 외 모든 작업을 멈춤.
    - 어떤 알고리즘을 사용하더라도 stop-the-world는 발생한다.
    - GC튜닝이란 이 stop-the-world 시간을 줄이는 것
- 메모리 해제 시 객체를 null로 지정하는 방식, System.gc() 메서드를 호출하는 방식이 있다. **System.gc()를 호출하는 것은 시스템 성능에 매우 큰 영향을 끼치므로 사용하지 말아야한다.**
- JAVA에서는 개발자가 명시적으로 메모리 해제를 하지 않기에 GC가 필요없는 객체를 찾아 지우는 작업을 한다.
- 두 가지 가설(전제 조건)에 만들어짐.
    1. 대부분의 객체는 금방 접근 불가능 상태가 된다.
    2. 오래된 객체에서 젊은 객체로의 참조는 아주 적게 존재.
    - 이러한 두 가설을 'weak generational hypothesis'라고 한다.
    - HotSpot VM에서는 이 가설의 장점을 살리기 위해 2개의 물리공간(Young, Old)영역으로 나눔.
- Young영역 : 새롭게 생성한 객체의 대부분이 여기 위치. 첫 가설에 의해 많은 객체가 생기고 사라진다. 이 영역에서 객체가 사라질 때 Minor GC가 발생한다고 한다.
- Old 영역 : Young영역에서 살아남은 객체가 여기로 복사 된다. GC는 상대적으로 적게 발생하고, 여기서 객체서 사라질 때 Major GC(Full GC)가 발생한다고 한다.

![](/assets/img/sample/JAVA/GC.png)  

- Permanent Generation 영역은 Method Area라고도 한다. 객체나 억류(intern)된 문자열 정보를 저장하는 곳. 이 영역에서도 GC가 발생할 수 있고 발생하면 Major GC의 횟수에 포함됨.

그러면 "Old영역에 있는 객체가 Young영역의 객체를 어떻게 참조하는 경우 어떻게 처리 되는가?"에 대한 것은 Old 영역에는 512 바이트의 덩어리(chunk)로 되어 있는 카드 테이블(card table)이 존재.

- 카드 테이블에는 Old 영역에 있는 객체가 Young 영역의 객체를 참조할 때마다 정보가 표시됨. 따라서 Young영역의 GC를 실행할 때에는 Old 영역에 있는 모든 객체를 참조하지 않고, 카드 테이블만 뒤져서 GC대상인지 확인
- 카드 테이블은 write barrier를 사용하여 관리. Minor GC를 빠르게 하도록 도와줌. 오버헤드는 발생하지만 전반적인 GC시간은 줄어든다.

## Young 영역의 구성

객체가 제일 먼저 생성되는 Young 영역은 3개의 영역으로 나뉜다.

- Eden
- Survivor (2개로 나뉨)
    - 새로 생성한 대부분의 객체는 Eden 영역에 위치
    - Eden에서 GC가 발생하고 살아남은 객체는 Survivor 영역 중 하나로 이동
    - 하나의 Suvivor의 영역이 가득 차게 되면 그 중 살아남은 객체를 다른 Suvivor영역으로 옮기고, 가득찬 Suvivor 영역은 비어짐.
    - 이 과정을 반복하다가 계속해서 살아남아 있는 객체는 Old 영역으로 이동.
    - **따라서 Suvivor 영역 중 하나는 반드시 비어 있어야함. 아니라면 시스템이 정상적인 상황이 아니라고 생각하면 된다.**
- HotSpot VM에서는 빠른 메모리 할당을 위해 2가지 기술을 사용.
    - bump-the-point : 스택마냥 마지막 객체의 위치만 본다. 새로 들어오는 객체가 Eden 영역에 넣기 적당하면 넣고, 마지막 객체 정보를 갱신한다. 마지막에 추가된 객체만 점검하면 되기에 빠른 메모리 할당 가능.
    - TLABs(Thread-Local Allocation Buffers) : **위의 방식은 멀티 스레드 환경에서는 lock이 발생하여 성능이 떨어진다.** 그래서 나온게 TLABs, 각각의 스레드가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가질 수 있도록 함.

## Old 영역에 대한 GC

Old 영역에 대한 GC는 JDK 7 기준으로 5가지 방식이 있다.

- Serial GC
- Parallel GC
- Parallel Old GC(Parallel Compacting GC)
- Concurrent Mark & Sweep GC( = CMS)
- G1(Garbage First) Gc

**운영 서버에서 절대 사용하면 안 되는 방식이 Serial GC 이유는 데스크톱의 CPU 코어가 하나만 있을 때 사용하기 위해서 만든 방식. 성능이 떨어짐.**

### 1. Serial GC(-XX:+UseSerialGC)

Old 영역의 GC는 mark-sweep-compact라는 알고리즘을 사용한다.

- mark : Old 영역에 살아 있는 객체를 식별(mark)
- Sweep : 힙(heap)의 앞 부분부터 확인하여 살아 있는 것만 남김.
- Compaction : 각 객체들이 연속되게 쌓이도록 힙 가장 앞부분부터 채워서 객체가 존재하는 부분과 없는 부분으로 나눔.

적은 메모리와 CPU 코어 개수가 적을 때 적합

### 2. Parallel GC(-XX:+USeParallelGC, = Throughput GC)

Serial GC와 알고리즘은 같다. but Parallel GC는 GC를 처리하는 쓰레드가 여러 개. 그렇기에 Serial GC보다 빠르게 객체 처리 가능.

메모리가 충분하고 코어의 개수가 많을 때 유리

![](/assets/img/sample/JAVA/ParallelGC.png)  

### 3. Parellel Old GC(-XX:+UseParallelOldGC)

JDK5 update 6부터 제공한 GC방식.

Parallel GC와 비교하여 Old 영역의 GC 알고리즘만 다름.

Mark-Summary-Compaction 단계를 거친다.

Summary는 앞의 Serial GC의 Sweep 보다 약간 더 복잡한 단계를 거침.

### 4. CMS GC(-XX:+UseConcMarkSweepGC)

1. inital Mark단계 : 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝냄. 따라서 멈추는 시간(stop-the-world)이 매우 짧다.
2. Concurrent Mark 단계 : 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인. **다른 스레드가 실행중인 상태에서 진행가능**
3. Remark 단계 : Concurrent Mark 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인(stop-the world).
4. Concurrent Sweep 단계 : 쓰레기를 정리하는 작업을 실행. 이 작업도 다른 스레드가 실행되고 있는 환경에서 진행.

stop-the-world 시간이 매우 짧고, 모든 애플리케이션의 응답속도가 매우 중요할 때 CMS GC를 사용, Low Latency GC라고도 부름.

하지만 단점도 존재한다.

- 다른 GC방식보다 메모리와 CPU를 많이 사용.
- Compaction 단계가 기본적으로 제공되지 않음.

따라서 조각난 메모리가 많아 Compaction을 실행하면 다른 GC의 stop-the-world시간보다 더 늘어날 수 있다. 

![](/assets/img/sample/JAVA/CMSGC.png)  

### 5. G1 GC

Young 영역, Old 영역이란 것이 없다.

![](/assets/img/sample/JAVA/G1GC.png)

바둑판의 각 영역에 객체를 할당하고 GC 실행. 그러다가 해당 영역이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행. CMS GC를 대체하기 위해 만들어짐.

- 성능 : 어떤 GC 방식보다도 빠름. JDK7에서 G1 GC 정식 제공.