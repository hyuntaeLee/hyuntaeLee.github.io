---
layout: post
title: 【Linux】 Whats kernel?
subtitle: Review
categories: Linux
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/10f87e65-fde3-4a2d-9af9-7d7b442f3ad7
tags: [linux, kernel, Review]
---

## 1. OS란?
컴퓨터는 크게 Hardware와 Software로 구분이 된다.

소프트웨어는 **OS(Operating System)**와 응용프로그램으로 다시 구분이 된다.

그리고 또 **OS(Operating System)**는 다시 Kernel과 System program으로 다시 구분이 된다.

**OS(Operating System)는 주로 다음 두 가지 역할을 가진 소프트웨어이다.**

- **Hardware Management**
    - 운영체제는 CPU, 메모리, 저장장치, 입출력 장치 등 컴퓨터의 하드웨어 자원을 효율적으로 관리한다.
    - 이는 자원의 할당, 스케줄링, 모니터링 등을 포함한다.
- **Software와 Hardware 간 인터페이스 제공**
    - 운영체제는 응용 프로그램이 하드웨어에 접근할 수 있도록 중간 다리 역할
    - 이를 통해 응용 프로그램 개발자는 하드웨어의 복잡한 세부사항을 알 필요 없이 운영체제가 제공하는 인터페이스를 통해 하드웨어 자원을 사용할 수 있다.

**Linux는 OS이고, Linux는 주로 세가지로 구성된다.**

- **Kernel**
    - Linux의 핵심 부분이다.
- **System Library**
    - 응용 프로그램이나 시스템 유틸리티가 커널의 기능에 접근할 수 있게 해주는 특별한 함수나 프로그램이다.
- **System Utility**
    - 특화된 개별 작업을 수행하는 프로그램들이다.

## 2. Linux Kernel
### 2-1. Linux Kernel?

Linux Kernel은 OS의 핵심 구성 요소로, Hardware와 Software 사이의 중요한 인터페이스 역할을 합니다. 주요 기능은 다음과 같다.

- 컴퓨터의 하드웨어 자원 관리
- 애플리케이션 소프트웨어가 자원을 효율적으로 사용할 수 있도록 지원
- 메모리, 프로세스, 파일 시스템, 디바이스 제어 등 기본적인 시스템 기능 제공

그러나 커널은 오직 컴퓨터 자원만을 관리하며, 사용자와의 직접적인 상호작용을 지원하지 않는다.


>시스템 프로그램의 필요성<br>
>사용자와의 상호작용을 위해 OS는 Kernel 외에도 시스템 프로그램을 제공한다.
>이중 가장 대표적인 것이 쉘(Shell)이다.

### 2-2. Linux Kernel Structure

Kernel의 가장 큰 역할은 컴퓨터의 물리적(Hardware)자원과 추상화 자원을 관리 하는 것이다.



>**추상화란?**
>
>물리적으로 하나 뿐인 하드웨어를 여러 사용자들이 번갈아 사용하게 중재함으로서, 마치 한 개의 하드웨어가 여러개인 것처럼 보여지도록 하는 기술을 말한다.
>
>추상화된 환경에서 각 사용자들은 본인이 하나의 Hardware를 독점하여 활용 하는 것 처럼 느끼게 되는데, 이를 위해 Kernel은 하나의 Hardware 자원을 여러 사용자들을 위한 복수개의 추상화된 객체로 관리한다.


<b>커널이 추상화하여 관리하는 물리적 자원들과 이를 추상화한 자원을 칭하는 용어 간 대응 관계</b>

**1. CPU -> 태스크(Task)**

- **물리적 자원**: CPU (중앙처리장치)
- **추상화**: Task or Process
- **설명**:
    - Kernel은 CPU 시간을 여러 프로세스에 할당한다.
    - Task 는 실행 중인 프로그램의 인스턴스를 나타낸다.
    - 커널은 태스크 스케줄링을 통해 CPU 사용을 관리한다.

**2. 메모리 -> 페이지(Page), 세그먼트(Segment)**

- **물리적 자원**: RAM
- **추상화**: Page or Segment
- **설명**:
    - 페이지는 고정 크기의 메모리 블록
    - 세그먼트는 가변 크기의 메모리 영역
    - 가상 메모리 시스템을 통해 물리적 메모리를 효율적으로 관리한다.

**3. 디스크 -> 파일(File)**

- **물리적 자원**: 하드 디스크, SSD 등의 저장 장치
- **추상화**: File
- **설명**:
    - 파일은 데이터의 논리적 단위
    - 파일 시스템을 통해 데이터의 저장, 검색, 관리를 수행한다.
    - 디렉토리 구조를 통해 파일을 체계적으로 조직화한다.

**4. 네트워크 -> 소켓(Socket)**

- **물리적 자원**: 네트워크 인터페이스 카드(NIC)
- **추상화**: Socket
- **설명**:
    - 소켓은 네트워크 통신의 엔드포인트
    - 애플리케이션이 네트워크를 통해 데이터를 송수신할 수 있게 한다.
    - TCP/IP 등의 프로토콜을 통해 네트워크 통신을 관리한다.

Kernel은 위에서 설명한 물리 자원과 추상 자원들을 관리(manage)한다.<br>
**따라서 Linux Kernel의 구성요소는 여러가지 자원들을 관리하는 관리자(manager)들이다.**
아래와 같이 크게 5가지 관리자가 있다.

- **Task Manager**
    - 물리적 자원인 CPU를 추상적 자원인 Task로 제공
- **Memory Manager**
    - 물리적 자원인 메모리를 추상적 자원인 Page or Segment로 제공
- **File System Manager**
    - 물리적 자원인 디스크를 추상적 자원인 File로 제공
- **Network Manager**
    - 물리적 자원인 네트워크 장치를 추상적 자원인 소켓으로 제공
- **Device Driver Manager**
    - 각종 외부 장치(프린터, GPU 등)에대한 접근

### 2-3 **Why the kernel manage these resources?**

이유는 사용자가 물리적 하드웨어에 접근하고 사용할 수 있도록 매개하기 위해서이다. 

인간이 직접 하드웨어를 접근하고 사용하기는 어렵기 때문에, 커널이 중간에서 이를 돕는 역할을 한다.

 즉, 커널은 사용자와 하드웨어 사이의 인터페이스 역할을 수행한다. 

이를 통해 사용자는 복잡한 하드웨어 구조를 이해하지 않고도 컴퓨터 시스템을 효과적으로 사용할 수 있게 된다.

<p align="center">
<img src="https://github.com/user-attachments/assets/2d2ba1cb-e797-4966-8775-084c4e72bcb6" width = 800>
</p>
위에서 설명한 kernel의 구성 요소들인 Resources Manager들이 존재하는 공간을  Kenerl Space라고한다.

Kernel Space위에 사용자로 여겨지는 Task(그림에선 process)들이 존재하는 User Space가 있다.(프로그램 파일이 결국 task가 된다.)

그리고 Kernel Space와 User Space 사이에는 System Call Interface가 있다.

User Space의 Task들이 커널이 관리하는 자원에 접근해야할 필요가 있으면, System Call Interface를 통해 Kernel Space의 Resources Manager에게 요청이 전달 된다.
그리고 이 Kernel의 각 Resources Manager는 사용자 요청에 맞게 알맞는 하드웨어에 사용자 명령을 전달하고 작업을 수행한다.

**그래서 Kernel이란?
⇒ 사용자가 System Call를 통해 컴퓨터 Resource을 사용할 수 있게해주는 Resources Manager이다.**

## 3. 유저 영역(user land)과 커널 영역(kernel land)

**메모리 구분**

- **유저 영역**: 프로그램 실행을 위한 메모리 공간 (stack, heap, bss, data, text 영역)
- **커널 영역**: 운영체제 실행에 필요한 메모리 공간

**접근 권한**

- **커널 모드**: CPU가 모든 메모리 영역에 접근 가능, 모든 명령어 실행 가능
- **유저 모드**: 자신의 메모리 영역에만 접근 가능, 제한된 명령어만 실행 가능

**구분의 목적**

1. 중요 시스템 자원 보호
2. 사용자 간 데이터 침범 방지
3. 시스템 안정성 및 보안 강화

**모드 전환**

- I/O 작업 등 특정 작업 수행 시 유저 모드에서 커널 모드로 전환 필요

이러한 구분을 통해 운영체제는 시스템 자원을 안전하게 관리하고, 사용자 간 데이터 보호를 실현한다.

## 4. System call

### 4-1. System call handle

<p align="center">
<img src="https://github.com/user-attachments/assets/9588db60-7a01-4a15-b907-8798dd587c48" width = 600>
</p>

1. **유저 프로그램이 시스템 콜 호출**
    - 유저 프로그램이 printf()와 같은 라이브러리 함수를 호출하면, 해당 함수는 I/O 작업을 위해 시스템 콜을 요청한다.
2. **Trap 발생 및 준비 작업**
    - 유저 모드에서 실행 중인 프로그램은 Trap에 걸리며, Wrapper Routine이 System call number 등 필요한 파라미터를 준비한다.
3. **모드 전환: User mode → Kernel mode**
    - chmodk 명령으로 CPU의 모드 비트를 변경하여 커널 모드로 전환하고, 프로그램은 커널 영역으로 이동한다.
4. **Trap Handler 실행**
    - 하드웨어가 sys_call()이라는 커널의 Trap Handler를 실행하며, 커널 내 assembly 함수가 동작한다.
5. **현재 상태 저장**
    - 유저 프로그램의 현재 상태를 저장하여, 작업 완료 후 원래 상태로 복귀할 수 있도록 준비한다.
6. **System call number 확인**
    - System call number를 커널의 System call table과 대조하여 올바른 함수인지 확인한다.
7. **System call function 실행**
    - System call table에서 해당 함수의 주소를 가져와 작업을 수행한다 (예: write() 함수 실행).
8. **User mode로 복귀**
    - 작업 완료 후 CPU 모드를 커널 모드에서 유저 모드로 전환하고, 저장된 상태로 복귀하여 프로그램을 계속 실행한다.

### 4-2. System call Wrapper routine

<p align="center">
<img src="https://github.com/user-attachments/assets/553fe645-d4ec-49bc-852a-7bc7df44edbd" width = 500>
</p>

**아키텍처별 시스템 콜 구현 차이**

- x86 아키텍처: **`int $0x80`** 명령어 사용
- x64 아키텍처: **`syscall`** 명령어 사용

**시스템 콜 호출 과정**

1. 트랩(Trap) 발생
    - **`int $0x80`** 또는 **`syscall`** 명령어 실행 시 트랩 발생
    - 커널 모드로 전환되며 제어권이 커널에게 넘어감
2. 시스템 콜 함수 실행
    - 커널은 eax(rax) 레지스터의 시스템 콜 번호를 확인
    - syscall_table에서 해당 번호에 대응하는 함수 주소를 찾아 실행

**시스템 콜 번호 할당**

시스템 콜 번호(예: 0번 open, 1번 close 등)는 컴파일러와 커널 간의 약속

컴파일러 제작 회사가 결정하며, 커널과 호환성 유지 필요

Compile time and Runtime

1. Compile time
    - 라이브러리 함수(예: print)가 시스템 콜 래퍼(예: write)를 포함
    - 래퍼 함수에서 해당 시스템 콜의 번호 설정
    - 컴파일러가 System call number결정 (예: write가 7번)
2. Runtime
    - **`int $0x80`** 또는 **`syscall`** 명령어 실행 시 트랩 발생
    - 커널이 설정된 시스템 콜 번호를 사용하여 해당 함수 호출



## reference
* [[Linux Kernel] 커널의 개념과 리눅스 커널의 구조](https://5equal0.tistory.com/entry/Linux-Kernel-%EC%BB%A4%EB%84%90%EC%9D%98-%EA%B0%9C%EB%85%90%EA%B3%BC-%EC%BB%A4%EB%84%90%EC%9D%98-%EA%B5%AC%EC%A1%B0)
* [Linux 커널 OS, 커널이란?](https://www.devkuma.com/docs/linux/kernel/basic1/)
* [[linux kernel] (1) - 커널(kernel)이란?](https://jiravvit.tistory.com/entry/linux-kernel-1-%EC%BB%A4%EB%84%90kernel%EC%9D%B4%EB%9E%80-1)
* [[Linux] Linux Kernel Compile (리눅스 커널 컴파일)](https://junshim.github.io/linux%20kernel%20study/Linux_Kernel_Compile/)
* [[Linux Kernel] System call](https://velog.io/@whattsup_kim/Linux-Kernel-System-call)
* [kernel of linux - system call(1)](https://jeongzero.oopy.io/ce68cf2c-4c14-431b-950f-e32eeae42e30)