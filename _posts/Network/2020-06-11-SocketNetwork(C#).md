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
# SocketNetwork using C#

### Network class 분류

1. 정보 클라스
- IPAddress
- DNS
- IPHOSTEntry
- IPEndPoint

2. 연결 클라스
- TcpListener
- TcpClient
- UdpClient

3. 전송 클라스
- NetworkStream
- StreamWriter/StreamReader
- BinaryWriter/BinaryReader


####Ex1 정보클래스

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Net;
using System.Net.Sockets;
using System.IO;

namespace InfoClass
{
    class Program
    {
        static void Main(string[] args)
        {

            //IPAddress.parse(ip주소) -> Ip 주소를 Long형으로 만들기
            IPAddress ip = IPAddress.Parse("127.0.0.1");

            //TcpListener(IPAddress ip,int port), Client로 부터 연결 대기
            TcpListener tcpListener = new TcpListener(ip, 13);

            //기다리는 Client 주소 출력
            Console.WriteLine("{0}", tcpListener.LocalEndpoint.ToString());

            // www.naver.com의 도메인이 나타내는 ip주소를 모두 출력
            IPAddress[] IP = Dns.GetHostAddresses("www.naver.com");
            foreach (IPAddress HostIP in IP)
            {
                Console.WriteLine("{0}", HostIP);
            }

            // IPHostEntry -> Domain명 IP주소를 저장하는 Container
            IPHostEntry HostInfo = Dns.GetHostEntry("www.naver.com");
            foreach (IPAddress ip_DNS in HostInfo.AddressList)
            {
                Console.WriteLine("{0}", ip_DNS);
            }
            Console.WriteLine("{0}", HostInfo.HostName);

            // IPEndPoint -> 목적지 ip주소와 Port번호 저장
            IPAddress IPInfo = IPAddress.Parse("127.0.0.1");
            int Port = 13;
            IPEndPoint EndPointInfo = new IPEndPoint(IPInfo, Port);
            Console.WriteLine("ip: {0} port: {1}", EndPointInfo.Address, EndPointInfo.Port);
            Console.WriteLine(EndPointInfo.ToString());
        }
    }
}
```

####Ex2 연결클래스

```c#
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Net;
using System.Net.Sockets;
using System.IO;

namespace ConnectClass
{
    class Program
    {
        static void Main(string[] args)
        {
            
            TcpListener tcpListener = new TcpListener(IPAddress.Parse("192.168.0.107"),7);
            tcpListener.Start();
            Console.WriteLine("대기 상태 시작");
            
            TcpClient tcpClient = tcpListener.AcceptTcpClient();
            
            tcpClient.Close();
            tcpListener.Stop();
        }
    }
}
```

