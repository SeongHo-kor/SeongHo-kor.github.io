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

1. 정보 클라스
- IPAddress
- DNS
- IPHOSTEntry
- IPEndPoint

---

#### code 

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
            IPAddress ip = IPAddress.Parse("192.168.0.107");
            byte [] ipbytes = ip.GetAddressBytes();
            IPAddress ipv6 = ip.MapToIpv6(); // IPv4를 IPv6로 매핑


            // DNS (DNS Class)
            // IP주소를 외우기 힘들기 때문에 호스트 명과 도메인 명을 사용한다.
            // 인터넷 호스트 명, 도메인 명 정보 얻기
            IPHostEntry hostEntry = Dns.GetHostEntry("www.google.com");
            foreach(IPAddress ip in hostEntry.AddressList){
                console.WriteLine(ip);
            }
            
            // 로컬 호스트명 정보 얻기
            string hostname = Dns.GetHostName();
            IPHostEntry localhost = Dns.GetHostEntry(hostname);

            // IP로 호스트명 알아내기
            // 사용 예) 스펨메시지 차단 - 어떤 IP주소로 온 메일을 호스트명, 도메인명으로 바꿔 타당한지 확인
            // Reverse Dns(rDns)가 동작하기 위해서는 Dns 설정에 해당 서버를 PTR레코드에 별도로 등록해야함
            IPAddress ip = IPaddress.Parse("74.125.28.99");
            IPHostEntry hostEntry = Dns.GetHostEntry(ip);
            Console.WriteLine(hostEntry.HostName);

            // TCP, UDP통신을 하기 위해 IP주소와 Port번호 필요 -> IPEndPoint Classs를 사용
            IPAddress ip = IPAddress.Parse("74.125.28.99");
            IPEndPoint ep = new IPEndPoint(ip,80);
            Console.WriteLine(ep.Tostring());
        }
    }
}
```

---

#### 출력

```sh
74.125.28.103
74.125.28.106
74.125.28.99
74.125.28.105
74.125.28.147
74.125.28.104

www.google.com

74.125.28.99:80
```
