---
layout: post
title: Socket Communication
---

# 소켓 통신

## JAVA에서 소켓 통신을 사용하는 이유
자바에서 소켓 통신은 C 또는 C++언어로 구현된 프로젝트와의 통신에 많이 사용됩니다.  
이유는 JAVA와 C의 데이터 개념이 다르기 때문이다. C에서는 구조체를 사용하는데 반해서 Java에는 구조체가 없다.  
이처럼 JAVA의 Object구조를 C에서 이해하지 못하고 C의 구조체를 JAVA에서 이해하지 못하기 때문에 서로 통신을 위해서 Byte단위로 정보를 주고받아야 한다.

## HTTP통신과 Socket통신의 차이점
__- 단방향 통신인 HTTP 통신__
HTTP 통신은 Client의 요청(Request)이 있을 때만 서버가 응답(Response)하여 해당 정보를 전송하고 곧바로 연결을 종료하는 방식입니다. Client가 요청을 보내는 경우에만 Server가 응답하는 단방향 통신으로 반대로 Server가 Client에게 요청을 보낼 수는 없다.

__- 양방향 통신인 Socket 통신__
Server와 Client가 특정 Port를 통해 실시간으로 양방향 통신을 하는 방식이다. HTTP 통신과는 다르게 Server와 Client가 특정 Port를 통해 연결되어 있어서 실시간으로 양방향 통신을 할 수 있다.  
실시간 채팅, 게임 등과 같이 즉각적으로 정보를 주고받는 경우에 사용된다.

## Socket 통신 흐름
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlRcp2%2FbtrrEUFw62k%2F5TBTR9g3jnvOWB3gDL0d31%2Fimg.jpg)
소켓은 응용프로그램에서 TCP/IP를 이용하는 창구 역할을 하며, 두 프로그램이 네트워크를 통해 서로 통신을 수행할 수 있도록 양쪽에서 생성되는 링크의 단자이다. 두 소켓이 연결되면 서로 다른 프로그램이 서로 데이털르 전달할 수 있게 된다.  
이러한 Socket 통신은 일련의 규칙이 정해져 있다.
1. 먼저 기다리는 측을 Server라고 하며, Server에서는 Port를 열고 Client의 접속을 기다린다.
1. 그리고 접속하는 측을 Client라고 하며, Server의 IP와 Port에 접속하여 통신이 연결된다.
1. Server와 Client 간의 통신은 Send, Receive의 형태로 주고받는다.
1. 그리고 통신이 끝나면 `close()`로 접속을 끊는다.

## 코드를 통해 자세하게 알아보기

### Server
데이터 송수신은 Blocking 방식으로 동작한다. 때문에 메인 스레드로만 구성하면 데이터를 올바르게 수신하는데 문제가 생길 수 있어 Multi-Thread 서버로 구현하였다.

Server 코드에서는 'ServerSocket'과 'Socket' 두 가지 소켓을 볼 수 있다.  
ServerSocket은 서버 프로그램에서 사용하는 소켓으로 클라이언트가 연결해오는 것을 기다린다.  
클라이언트가 연결해 올 때마다 요청은 Queue에 쌓이고, 각각의 클라이언트 연결에 `accept()`하여 요청을 Queue에서 꺼내고 Socket 객체가 리턴된다.  
이렇게 리턴되는 Socket을 활용하여 클라이언트와 데이터를 주고받으며, 예시와 같은 멀티스레드 환경에서는 Socket을 생성한 스레드에 주어서 클라이언트와 데이터를 주고받는다.
```java
public class TcpServerExample {

    public static int tcpServerPort = 9999;

    public static void main(String[] args) {
        new TcpServerExample(tcpServerPort);
    }

    public TcpServerExample(int portNo) {
        tcpServerPort = portNo;
        try {
            // ServerSocket 생성
            ServerSocket serverSocket = new ServerSocket();
            serverSocket.bind(new InetSocketAddress(tcpServerPort));
            System.out.println("Starting tcp Server: " + tcpServerPort);
            System.out.println("[ Waiting ]\n");
            while (true) {
                // socket -> bind -> listen socket 클래스 내부에 구현되어 있음
                Socket socket = serverSocket.accept();
                System.out.println("Connected " + socket.getLocalPort() + " Port, From " + socket.getRemoteSocketAddress().toString() + "\n");
                // Thread
                Server tcpServer = new Server(socket);
                tcpServer.start();
            }
        } catch (IOException io) {
            io.getStackTrace();
        }
    }

    public class Server extends Thread {
        private Socket socket;

        public Server(Socket socket) {
            this.socket = socket;
        }

        public void run() {
            try {
                while (true) {
                    // Socket에서 가져온 출력스트림
                    OutputStream os = this.socket.getOutputStream();
                    DataOutputStream dos = new DataOutputStream(os);

                    // Socket에서 가져온 입력스트림
                    InputStream is = this.socket.getInputStream();
                    DataInputStream dis = new DataInputStream(is);

                    // read int
                    int recieveLength = dis.readInt();

                    // receive bytes
                    byte receiveByte[] = new byte[recieveLength];
                    dis.readFully(receiveByte, 0, recieveLength);
                    String receiveMessage = new String(receiveByte);
                    System.out.println("receiveMessage : " + receiveMessage);
                    System.out.println("[ Data Receive Success ]\n");

                    // send bytes
                    String sendMessage = "서버에서 보내는 데이터";
                    byte[] sendBytes = sendMessage.getBytes("UTF-8");
                    int sendLength = sendBytes.length;
                    dos.writeInt(sendLength);
                    dos.write(sendBytes, 0, sendLength);
                    dos.flush();

                    System.out.println("sendMessage : " + sendMessage);
                    System.out.println("[ Data Send Success ]");
                }
            } catch (EOFException e) {
                // readInt()를 호출했을 때 더 이상 읽을 내용이 없으면 EOFException이 발생한다.
            } catch (IOException e) {
                e.printStackTrace();
            } finally {
                try {
                    if (this.socket != null) {
                        System.out.print("\n[ Socket closed ] ");
                        System.out.println("Disconnected :" + this.socket.getInetAddress().getHostAddress() + ":"
                                + this.socket.getPort());
                        this.socket.close();
                    }
                } catch (Exception e) {
                }
            }
        }
    }
}
```

```java
Socket socket = ServerSocket.accept();
```
`accept()`를 통해 Socket 객체를 가지고 오는 부분에서 내부적으로는 bind → Listen 과정이 실행된다.

### Client

```java
public class TcpClientExample {

    public static void main(String[] args) {

        Socket socket = null;

        try {
            // Server와 통신하기 위한 Socket
            socket = new Socket();
            System.out.println("\n[ Request ... ]");
            // Server 접속
            socket.connect(new InetSocketAddress("localhost", 9999));
            System.out.println("\n[ Success ... ]");

            byte[] bytes = null;
            String message = null;
            // Socket에서 가져온 출력스트림
            OutputStream os = socket.getOutputStream();
            DataOutputStream dos = new DataOutputStream(os);

            // send bytes
            message = "클라이언트에서 보내는 데이터";
            bytes = message.getBytes("UTF-8");

            dos.writeInt(bytes.length);
            dos.write(bytes, 0, bytes.length);
            dos.flush();

            System.out.println("\n[ Data Send Success ]\n" + message);

            // Socket에서 가져온 입력스트림
            InputStream is = socket.getInputStream();
            DataInputStream dis = new DataInputStream(is);

            // read int
            int receiveLength = dis.readInt();

            // receive bytes
            if (receiveLength > 0) {
                byte receiveByte[] = new byte[receiveLength];
                dis.readFully(receiveByte, 0, receiveLength);

                message = new String(receiveByte);
                System.out.println("\n[ Data Receive Success ]\n" + message);
            }

            // OutputStream, InputStream close
            os.close();
            is.close();

            // Socket 종료
            socket.close();
            System.out.println("\n[ Socket closed ]\n");

        } catch (Exception e) {
            e.printStackTrace();
        }

        if (!socket.isClosed()) {
            try {
                socket.close();
                System.out.println("\n[ Socket closed ]\n");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

```java
Socket socket = new Socket("localhost", 포트번호);
Socket socket = new Socket(new InetSocketAddress("localhost", 포트번호));
```
Client 에서 Socket을 사용하기 위한 두가지 방법이다. 연결하려는 외부 서버의 IP주소 대신 도메인 이름을 알고 있을때에는 InetSocketAddress 클래스를 사용할 수 있다.

#### InputStream read() method
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbqmzyJ%2FbtrrxYJv0pq%2FUhrKNEc0oeFp6lMZ6kJni0%2Fimg.png)
데이터를 받기 위해 InputStream의 `read()`메서드를 호출하면 상대방이 데이터를 보내기 전까지 블로킹이 된다. `read()`메서드가 블로킹 해제되고 리턴되는 경우는 다음과 같다.
1. 상대방이 데이터를 보냈을 때 (리턴 값: 읽은 바이트 수)
1. 상대방이 정상적으로 Socket의 `close()` 메서드를 호출했을 때 (리턴 값: -1)
1. 상대방이 비정상적으로 종료했을 때 (IOExeption 발생)

## InputStream, OutputStream close() 여부
자바에서 InputStream, OutputStream 같은 자원은 사용하고나서 해제하지 않으면 메모리 누수 및 특정 프로그램의 독점으로 인해 해당 객체가 올바르게 동작하지 않을 수 있다. 때문에 `close()`메소드를 통해 자원을 해제해야 한다.

예시 코드를 보면 Server에서는 `close()`하는 부분이 없고, Client에서는 있다.
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbSwETI%2Fbtrrun36Bi0%2FbyEuAehYYDNL6oxA1VKeZK%2Fimg.png)
사실 Client 코드에서도 `close()`하는 부분이 없더라도 동작하는데 아무런 문제가 없다. 이유는 Socket 클래스의 `close()`메서드를 통해 알 수 있다.  
___Closing this socket will alse close the socket's InputStream and OutputStream.___

반대로 InputStream, OutputStream이 `close()`되면 Socket도 `close()`되기 때문에 주의해야 한다.

#### 출처
[자바 소켓 통신(Socket)을 사용하는 이유와 동작 원리 및 코드](https://wildeveloperetrain.tistory.com/122)