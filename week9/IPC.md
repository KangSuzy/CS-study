
# IPC
작성자: 김다인

---

## 프로세스 간 통신
Inter-Process Communication, 줄여서 IPC로 불리는 프로세스 간 통신은 **프로세스들 간 서로 데이터를 주고받는 행위 그 자체를 가리키거나 그에 대한 방법, 경로**를 뜻한다.  
현대 운영체제에서는 하나의 프로세스가 고유 메모리 주소만 갖기 때문에 다른 프로세스 메모리에 접근할 수 없다. 독립적인 구조와 영역을 갖는 프로세스들 간 통신을 해야 하는 경우(한 프로세스가 다른 프로세스의 자원에 접근한다거나) 커널이 제공하는 IPC 설비를 통해 통신한다.  

### IPC 방식
#### 파일
대부분의 운영 체제에서 제공한다.

#### 파이프
모든 POSIX 시스템과 Windows에서 제공한다. 유닉스 계열 운영 체제에서 제공되는 병행성 메커니즘의 하나로, **두 프로세스가 생산자-소비자 모델에 따라 통신할 수 있게 해주는 원형 버퍼**이다.  
두 프로세스가 파이프로 연결되는데, **한 프로세스는 쓰고 다른 프로세스는 읽는 단방향 통신만 지원**하는 FIFO 형태 큐이다. 양쪽으로 모두 송수신하려면(전이중 통신) 2개의 파이프가 필요하다.  

* (1) 익명 파이프(anonymous pipe)
	* 매우 간단
	* 부모-자식 간 통신처럼 **서로 관련된 프로세스들**이나 통신할 프로세스를 명확히 알 수 있는 경우에만 사용
* (2) 지명 파이프(named pipe)
	* 유닉스 및 유닉스 계열의 일반 파이프의 확장격
	* 영구 프로세스가 소멸해도 계속 존재하기 때문에 사용하지 않으면 제거해야 함
	* **관련 없는 프로세스들도 공유**할 수 있지만 이름을 알아야 통신 가능

#### 메시지 큐
대부분의 운영 체제에서 제공하며, Windows 시스템의 모든 스레드에 존재한다. 입출력 방식은 지명 파이프와 같지만, 메시지 큐는 데이터 흐름이 아니라 **메모리 공간**이다. 사용할 데이터에 번호를 붙이며 여러 프로세스가 동시에 데이터를 쉽게 다룰 수 있도록 한다.  

#### 공유 메모리
모든 POSIX 시스템과 Windows에서 제공하며, **여러 프로그램이 동시에 접근해 데이터 자체를 공유할 수 있는 메모리**이다.  
프로세스가 공유 메모리 할당을 커널에 요청하면, 커널은 해당 프로세스에 메모리 공간을 할당해주고, 이후 모든 프로세스가 해당 공유 메모리에 접근할 수 있게 된다. 중개자 없이 곧바로 메모리에 접근할 수 있어서 IPC 중 가장 빠르게 동작한다.  

#### 메모리 맵
모든 POSIX 시스템과 Windows에서 제공하며, 운영체제에서 파일을 다루는 방법 중 하나이다.  
공유 메모리처럼 메모리를 공유해 주되 **메모리 맵 파일을 통해 프로세스의 가상 메모리 주소 공간에 열린 파일을 맵핑**시켜 공유하는 방식이라는 차이점이 있다. 대용량 데이터를 공유해야 할 때 주로 사용된다. 메모리와 파일 간 자료 전송은 운영체제가 자동으로 처리하므로 간단하게 사용할 수 있다.  

#### 소켓
대부분의 운영 체제에서 제공한다. 컴퓨터 네트워크를 경유하는 프로세스 간 통신의 종착점으로, 네트워크 소켓 통신을 통해 데이터를 공유하는 서버-클라이언트 방식을 사용한다. 원격에서 프로세스 간 데이터를 공유할 때 사용된다. 

IPC 통신에서 프로세스 간 데이터를 동기화하고 보호하기 위해 세마포어와 뮤텍스 락 사용을 고려할 수 있다.  

---
* [위키백과 : 프로세스 간 통신](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4_%EA%B0%84_%ED%86%B5%EC%8B%A0)
* [위키백과 : 파이프 (유닉스)](https://ko.wikipedia.org/wiki/%ED%8C%8C%EC%9D%B4%ED%94%84_(%EC%9C%A0%EB%8B%89%EC%8A%A4))
* [위키백과 : 명명된 파이프](https://ko.wikipedia.org/wiki/%EB%AA%85%EB%AA%85%EB%90%9C_%ED%8C%8C%EC%9D%B4%ED%94%84)
* [위키백과 : 메모리 맵 파일](https://ko.wikipedia.org/wiki/%EB%A9%94%EB%AA%A8%EB%A6%AC_%EB%A7%B5_%ED%8C%8C%EC%9D%BC)
* [위키백과 : 네트워크 소켓](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EC%86%8C%EC%BC%93)
* [IPC(Inter Process Communication)](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Computer%20Science/Operating%20System/IPC(Inter%20Process%20Communication).md)