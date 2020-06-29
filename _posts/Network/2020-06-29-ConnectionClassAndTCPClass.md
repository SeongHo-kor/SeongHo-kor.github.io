---
title : "Network"
category :
    - Network
tag :
    - SocketNetwork
author_profile : true
sidebar_main : true
use_math : true
headr:
    teaser : 
---

# SocketNetwork
#### 사용한 클라스

1. 연결 클라스
- TcpListener
- TcpClient
- UdpClient

2. 전송 클라스
- NetworkStream
- StreamWriter/StreamReader
- BinaryWriter/BinaryReader

---

## 연결 클래스, 전송 클래스를 이용한 TCP Network Code

#### Server

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Net;
using System.IO;
using System.Net.Sockets;


namespace Server
{
    class Program
    {
        static void Main(string[] args)
        {
            // TCP Server에서 ip주소와 Port를 열고 Listening을 해야한다.
            // TcpListener 생성자에 ip 주소와 포트번호를 지정
            // IPAddress.Any -> 0.0.0.0으로 로컬 머신의 모든 ip주소를 뜻함
            TcpListener tcpListener = new TcpListener(IPAddress.Any, 8000);
            tcpListener.Start();
            Console.WriteLine("Server start!");
            Console.WriteLine("Waiting Client....");

            // Client로 부터 요청이 들어오면 요청을 수용하고 
            // 서버에서 새 TcpClient 객체를 생성하여 리턴한다.
            TcpClient tcpClient = tcpListener.AcceptTcpClient();
            Console.WriteLine("Client is connected");

            // Data를 주고받을 Stream을 생성
            NetworkStream ns = tcpClient.GetStream();

            // 데이터를 보내기 위한 Stream
            StreamWriter sw = new StreamWriter(ns);

            // 데이터를 읽어오기 위한 Stream
            StreamReader sr = new StreamReader(ns);

            // 서버와 연결이 성공했다는 메시지를 클라이언트에게 전송
            string StartMsg = "Server Connect Success!";
            sw.WriteLine(StartMsg);
            sw.Flush();


            // Exit 메시지가 올때까지 클라이어트로 부터 데이터를 읽어와서 출력
            while (true)
            {
                string strMsg = sr.ReadLine();
                if (strMsg == "exit") break;

                Console.WriteLine(strMsg);
                sw.WriteLine(strMsg);
                sw.Flush();
            }

            // Exit 메시지 이후 사용했던 객체 모두 닫음
            sw.Close();
            sr.Close();
            ns.Close();
            tcpClient.Close();
            tcpListener.Stop();
            Console.WriteLine("Connected is finish");

        }
    }
}

```

---

#### Client

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Net;
using System.IO;
using System.Net.Sockets;

namespace Clients
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                // Tcp 클라이언트가 서버랑 연결하기 위해서 ip주소와 port번호가 필요
                // TcpClient 생성장에 ip주소와 port번호를 입력하면 자동으로 Tcp Connection 연결
                TcpClient tcpClient = new TcpClient("192.168.0.107", 8000);
                Console.WriteLine("Client Start!");
            
                // Data를 주고받을 Stream을 생성
                NetworkStream ns = tcpClient.GetStream();

                // 데이터를 보내기 위한 Stream
                StreamWriter sw = new StreamWriter(ns);

                // 데이터를 읽어오기 위한 Stream
                StreamReader sr = new StreamReader(ns);
                
                // 서버와 연결되었는지 확인하는 법
                // 1. tcpClient.connection() 메서드로 명시적을로 연결됨을 확인
                // 2. 서버로 부터 연결 성공 메시지를 받는다
                string StrRecvMsg = sr.ReadLine();
                Console.WriteLine(StrRecvMsg);

                // Exit 메시지를 입력하기 전까지 서버에 메시지를 보냄
                while (true)
                {
                    string StrSendMsg = Console.ReadLine();
                    sw.WriteLine(StrSendMsg);
                    sw.Flush();
                    if (StrSendMsg == "Exit") break;
                    StrRecvMsg = sr.ReadLine();
                    Console.WriteLine(StrRecvMsg);
                }

                // Exit 메시지 이후 사용했던 객체 모두 닫음
                sr.Close();
                sw.Close();
                ns.Close();
                tcpClient.Close();
                Console.WriteLine("Connected is finish");
            }
            catch (SocketException e)
            {
                Console.WriteLine(e.ToString());
            }
        }
    }
}

```

#### 출력

#### Server

```sh
Server Start!
Waiting Client...

// Connection 성공

Client is connected

// 이후 데이터 주고 받는 Echo 서버로서의 역할 수행
Hello
Exit

// 연결 종료 메시지
Connection is finished
```

#### Client

```sh
Client Start!

// 서버랑 연결이 완료 되었을 때 서버로 부터 메시지 받음
Server Connect Success!

// 이후 데이터 주고 받는 Echo 클라이언트로 역할 수행
// 명령어로 Exit 전송하면 연결 종료
Hello
Hello
Exit
Exit

// 연결 종료 메시지
Connection is finished
```


