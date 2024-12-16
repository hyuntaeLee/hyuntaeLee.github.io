---
layout: post
title: 【Linux】 Bluetooth Background
subtitle: Review
categories: Linux
banner:
  image: https://github.com/hyuntaeLee/hyuntaeLee.github.io/assets/97331148/10f87e65-fde3-4a2d-9af9-7d7b442f3ad7
tags: [linux, kernel, Review]
---

## Bluetooth

### Whats bluetooth?

블루투스(Bluetooth)는 근거리 무선 통신 기술의 하나로, 두 기기들 간에 중간 네트워킹 장비 없이 데이터를 주고받을 수 있게 해줍니다

Bluetooth는 크게 **블루투스 클래식(Bluetooth Classic)**과 **블루투스 저에너지(Bluetooth Low Energy, BLE)**로 구분된다.

> 간단하게 Bluetooth 3.0까지는 블루투스 클래식, 블루투스 4.0부터는 BLE이라고 부른다.
> 

### So, Whats the **Bluetooth Low Energy?**

**Bluetooth Low Energy**는 영어 그대로 저전력 블루투스이다. 

블루투스 LE는 두 기기 간 점대점 통신 외에도 한 기기가 동시에 무제한 수의 수신기에 데이터를 전송할 수 있는 advertise mode를 지원하며, 수만 개의 기기가 네트워크를 형성하고 네트워크 내의 모든 기기가 서로 통신할 수 있는 블루투스 메시 네트워킹의 기반이 된다.

- 4.0버전 이전의 기술들은 쌍방향 통신을 통해 서로 데이터를 주고받는다
    - 4.0버전도 여전히 가능하다.
- 하지만 단방향 통신도 가능하게 되었다는게 4.0의 특징이다.
    - 이러한 단방향 통신은 블루투스 기기가 정보를 송신할수 있으면서도 수신하기 위해 대기할 필요가 없다는 뜻이다.

### then, Linux and Bluetooth Architecture


<p align="center">
<img src="https://github.com/user-attachments/assets/fbe3017b-48ef-446f-85c3-99790b51fdc3" width = 500>
</p>
해당 이미지는 리눅스 시스템 내에서 구현된 Bluetooth LE의 아키텍처를 보여준다.

헤당 아키텍처를 보다시피, 두가지 주요 구성 요소로 나눠진다. 크게 Host와 Controller로 나눠진다.

**호스트(Host)**: 이 부분은 상위 레이어의 프로토콜과 응용 프로그램을 관리한다.

⇒이 부분은 주로 **커널 영역**에서 작동합니다. 커널은 BLE 스택의 여러 프로토콜과 프로파일을 구현하여, 하드웨어 및 운영 체제 간의 인터페이스 역할을 한다

- **(GAP, Generic Access Profile)**: 기기 탐색, 연결 설정 및 보안 설정에 대한 절차를 정의한다.
- **(GATT, Generic Attribute Profile)**: 블루투스 기기 간의 데이터 교환을 위한 서비스와 특성을 관리한다.
- **(ATT, Attribute Protocol) 및 보안 관리 프로토콜(SMP, Security Manager Protocol)**: 속성 작업과 보안 기능을 각각 관리한다.
- **(L2CAP, Logical Link Control and Adaptation Protocol)**: 데이터 캡슐화와 재조합을 용이하게 하여 상위 레이어 프로토콜과 응용 프로그램이 L2CAP 데이터 패킷을 송수신할 수 있도록 한다.

**컨트롤러(Controller)**: 이 부분은 하위 레이어 프로토콜과 물리적 라디오 측면을 처리한다.

⇒컨트롤러 부분은 **하드웨어 또는 컨트롤러 디바이스 자체** 

- **(Physical Layer)**: 원시 라디오 신호의 전송과 수신을 다룬다.
- **(Link Layer)**: 기기 연결 상태와 패킷 교환을 관리한다.
- **(Isochronous Adaptation Layer)**: 음성이나 비디오와 같은 동기화된 데이터 스트림을 지원한다.

> 호스트와 컨트롤러는 **호스트 컨트롤러 인터페이스(HCI, Host Controller Interface)**를 통해 통신하며, 이는 호스트가 컨트롤러에 접근할 수 있도록 표준화된 통신 다리를 제공한다.
> 

### architecture of Bluetooth on Linux when using BlueZ

<p align="center">
<img src="https://github.com/user-attachments/assets/c47cea57-5a34-48ac-90c5-8be1260bc626" width = 700>
</p>
이 그림에서 볼 수 있듯이, BlueZ는 Bluetooth LE 스택의 **Host** 레이어를 구현하며, **Controller는** 일반적으로 컴퓨터의 일부로 통합되어 있거나 USB 블루투스 동글과 같은 주변장치 내에 구현된다. 

⇒ 라즈베리 파이와 같은 기기에서는 통합된 칩 내에 **Controller**가 존재한다

**Host** 에서의 BlueZ와 **Controller** 내 블루투스 스택의 하위 레이어 간의 통신은 Host Controller Interface (HCI)라고 하는 표준 논리 인터페이스를 통해 이루어진다.

 >응용 프로그램은 GAP 및 GATT에 기반한 프로파일 및/또는 서비스를 구현하거나, 블루투스 메시 모델을 구현하여 블루투스 메시 네트워크에서 노드로 작동한다. 
>
>리눅스 컴퓨터가 GAP/GATT 응용 프로그램이나 블루투스 메시 노드를 호스트하는지에 따라, 두 가지 BlueZ 데몬 프로세스 중 하나가 실행되어야 한다. 
     =>GAP/GATT의 경우는 bluetoothd 가 해당된다.
     ⇒블루투스 메시의 경우는 bluetoothmeshd가 해당된다. 
>
>한 대의 컴퓨터에서 GAP/GATT와 블루투스 메시 노드를 동시에 사용할 수는 없다. 선택된 BlueZ 데몬은 호스트 애플리케이션을 대신하여 모든 HCI 통신을 직렬화하고 처리합니다.


 그리고, 사진에 dbus-daemon이 있는데 이는 리눅스에서 **D-Bus**라고 하는 인터프로세스 통신(IPC) 시스템의 핵심 부분이다.

## D-Bus

D-Bus는 동일한 시스템에서 실행 중인 여러 프로세스 간의 통신을 가능하게 한다. BlueZ의 경우 바로 위에 있는 [사진](https://www.notion.so/background-e97298a1f4f045e29e9dc0ad8b5cad97?pvs=21)을 보다시피, Bluetooth 데몬(bluetoothd)과 작성한 애플리케이션 간의 통신에 사용된다.

D-Bus의 주요 장점 중 하나는 대부분의 프로그래밍 언어가 D-Bus에 대한 바인딩을 제공하기 때문에, BlueZ API는 언어에 구애받지 않는다는 것이다. D-Bus 바인딩을 사용할 때는 다음 네 가지 정보를 알아야 한다

1. **어느 버스(system 또는 session)에 서비스가 있는지**
2. **서비스의 버스 이름**
3. **메소드와 속성의 오브젝트 경로**
4. **메소드, 속성 또는 신호의 인터페이스**

D-Bus에서 사용되는 정보 계층 구조는 특정 요소에 접근하기 위해 고유하게 식별하는 데 사용된다.

```
System Bus -> Bus Name -> Object Path -> Interface -> Method
```

해당 구조는 D-Bus 상에서 메소드를 호출할 때 사용하는 종류의 콜 스택과 비슷하다고 생각하면 될거같다.

> BlueZ API에 접근하려면 이 계층 구조를 따라 버스, 버스 이름, 객체 경로, 인터페이스, 메소드를 차례로 지정해야 한다. 이 정보들을 정확히 알고 있어야 D-Bus 바인딩을 통해 BlueZ API를 호출할 수 있다.
> 

### Buses

두 종류의 버스들이 존재한다.

1. 사용자 로그인 세션을 위한 `session bus`
2. 시스템 서비스들에 접근을 제공하는 `system bus` 
    1. 시스템 서비스와 사용자 애플리케이션 간의 통신을 가능하게 한다.
    2. **BlueZ 및 bluetoothd 접속을 위해 우리는 항상 시스템 버스를 사용한다.**

### Bus Name

나의 컴퓨터에서 사용 가능한 모든 시스템 D-Bus 이름을 보기 위해 아래 명령어를 실행해본다.

```bash
$ busctl list
```

확인해 보면, 아래에 org.bluez가 있다. 이것은 BlueZ bluetoothd 정보에 사용됩니다.

### Object Path

`Object Path`는 파일 시스템 경로처럼 보이지만, 실제로는 D-Bus 상의 고유 객체들을 식별하는 식별자입니다. 예를 들어, 리눅스 시스템에서 기본 블루투스 어댑터의 경로는 일반적으로 `/org/bluez/hci0` 이다.

```bash
l@ubuntu:~/Desktop$ busctl tree org.bluez
└─/org
  └─/org/bluez
    └─/org/bluez/hci0
```

### Interface

인터페이스를 메소드와 신호의 명명된 그룹으로 생각해라. D-Bus는 간단한 네임스페이스 문자열로 인터페이스를 식별한다다. 

하나의 객체 경로에 여러 인터페이스가 있을 수 있다. 예를 들어, `/org/bluez/hci0`에는 다음과 같은 인터페이스가 있을 수 있다.

```bash
org.bluez.Adapter1
org.bluez.GattManager1
org.bluez.LEAdvertisingManager1
org.bluez.Media1
org.bluez.NetworkServer1
org.freedesktop.DBus.Introspectable
org.freedesktop.DBus.Properties
```

해당 객체에 무엇이 있는지는 introspection을 통해 알아낼 수 있다. 아래 명령어를 통해 확인해보자.

```bash
$ busctl introspect org.bluez /org/bluez/hci0
NAME                                TYPE      SIGNATURE RESULT/VALUE           >
org.bluez.Adapter1                  interface -         -                      >
.GetDiscoveryFilters                method    -         as                     >
.RemoveDevice                       method    o         -                      >
.SetDiscoveryFilter                 method    a{sv}     -                      >
.StartDiscovery                     method    -         -                      >
.StopDiscovery                      method    -         -                      >
.Address                            property  s         "58:1C:F8:F3:E1:10"    >
.AddressType                        property  s         "public"               >
.Alias                              property  s         "ubuntu"               >
.Class                              property  u         786432                 >
.Discoverable                       property  b         false                  >
.DiscoverableTimeout                property  u         180                    >
.Discovering                        property  b         false                  >
.Modalias                           property  s         "usb:v1D6Bp0246d0535"  >
.Name                               property  s         "ubuntu"               >
.Pairable                           property  b         false                  >
.PairableTimeout                    property  u         0                      >
.Powered                            property  b         true                   >
.UUIDs                              property  as        7 "0000110a-0000-1000-8>
org.bluez.Media1                    interface -         -                      >
.RegisterApplication                method    oa{sv}    -                      >
.RegisterEndpoint                   method    oa{sv}    -                      >
.RegisterPlayer                     method    oa{sv}    -                      >
.UnregisterApplication              method    o         -                      >
.UnregisterEndpoint                 method    o         -                      >
.UnregisterPlayer                   method    o         -                      >
org.bluez.NetworkServer1            interface -         -                      >
.Register                           method    ss        -                      >
.Unregister                         method    s         -                      >
org.freedesktop.DBus.Introspectable interface -         -                      >
.Introspect                         method    -         s                      >
org.freedesktop.DBus.Properties     interface -         -                      >
.Get                                method    ss        v                      >
.GetAll                             method    s         a{sv}                  >
.Set                                method    ssv       -                      >
.PropertiesChanged                  signal    sa{sv}as  -  
```

### reference
* [BleedingTooth: Linux Bluetooth Zero-Click Remote Code Execution](https://google.github.io/security-research/pocs/linux/bleedingtooth/writeup.html#introduction)
* [저전력 블루투스 BLE(Bluetooth Low Energy)란?](https://blog.naver.com/ycpiglet/222630970457)
* [블루투스 프로토콜 스택(Bluetooth Protocol Stack)](https://blog.naver.com/ilikebigmac/221535338595)
* [Understanding Bluetooth Technology for Linux - Linux.com](https://www.linux.com/news/understanding-bluetooth-technology-for-linux/)
* [Introducing: The Bluetooth Low Energy Primer Bluetooth® Technology Website](https://www.bluetooth.com/blog/introducing-the-bluetooth-low-energy-primer/)
* [The Bluetooth Technology for Linux Developers Study Guide Bluetooth® Technology Website](https://www.bluetooth.com/blog/the-bluetooth-for-linux-developers-study-guide/)
* [Bluetooth overview - stm32mpu](https://wiki.stmicroelectronics.cn/stm32mpu/wiki/Bluetooth_overview)
* [D-Bus and Bluez](https://ukbaz.github.io/howto/python_gio_1.html)