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

# SocketNetwork(WindowForm)


#### 서버

##### Server windowform
<img src="C:\Users\SeongHo-PC\Documents\SeongHo-kor.github.io\_posts\Network\image\server.JPG">

```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

using System.Net.Sockets;
using System.Net;
using System.IO;

using System.Threading;

namespace WindowsFormsApp1
{

    public partial class Form1 : Form
    {

        private TcpListener tcpListener = null;
        private TcpClient tcpclinet = null;
        private BinaryWriter bw = null;
        private BinaryReader br = null;
        private NetworkStream ns = null;

        int intValue;
        float floatValue;
        string strValue;

        private void AllClose()
        {
            if (bw != null)
            {
                bw.Close();
                bw = null;
            }
            if (br != null)
            {
                br.Close();
                br = null;
            }
            if (ns != null)
            {
                ns.Close();
                ns = null;
            }
            if (tcpclinet != null)
            {
                tcpclinet.Close();
                tcpclinet = null;
            }
        }

        private int DataReceive()
        {
            intValue = br.ReadInt32();
            if (intValue == -1)
                return -1;

            floatValue = br.ReadSingle();
            strValue = br.ReadString();

            string str = intValue + "/" + floatValue + "/" + strValue;
            MessageBox.Show(str);
            return 0;
        }
        private void DataSend()
        {
            bw.Write(intValue);
            bw.Write(floatValue);
            bw.Write(strValue);
            MessageBox.Show("보냈습니다");
        }

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            tcpListener = new TcpListener(IPAddress.Any, 3000);
            tcpListener.Start();
            IPHostEntry host = Dns.GetHostEntry(Dns.GetHostName());
            for (int i = 0; i < host.AddressList.Length; i++)
            {
                if (host.AddressList[i].AddressFamily == AddressFamily.InterNetwork)
                {
                    textBox1.Text = host.AddressList[i].ToString();
                    break;
                }
            }
        }



        private void button1_Click_1(object sender, EventArgs e)
        {
            tcpclinet = tcpListener.AcceptTcpClient();
            if (tcpclinet.Connected)
            {
                //Client의 IP주소를 가져옴
                textBox2.Text = ((IPEndPoint)tcpclinet.Client.RemoteEndPoint).Address.ToString();
            }
            ns = tcpclinet.GetStream();
            bw = new BinaryWriter(ns);
            br = new BinaryReader(ns);
        }


        private void button2_Click(object sender, EventArgs e)
        {
            while (true)
            {
                if (tcpclinet.Connected)
                {
                    if (DataReceive() == -1)
                        break;

                    DataSend();
                }
                else
                {
                    AllClose();
                    break;
                }
            }
            AllClose();
        }
        private void Form1_fromClosing(object sender, FormClosingEventArgs e)
        {
            AllClose();
            tcpListener.Stop();
        }
    }
}
```

---

#### 클라이언트

##### client windowform 

<img src="C:\Users\SeongHo-PC\Documents\SeongHo-kor.github.io\_posts\Network\image\clients.JPG">


```c#
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Windows.Forms;
using System.Net.Sockets;
using System.Net;
using System.IO;

namespace WindowsFormsApp2
{
    public partial class Form1 : Form
    {
        private TcpClient tcpclinet = null;
        private BinaryWriter bw = null;
        private BinaryReader br = null;
        private NetworkStream ns = null;


        int intValue;
        float floatValue;
        string strValue;

        public Form1()
        {
            InitializeComponent();
        }

        private void Form1_Load(object sender, EventArgs e)
        {
            ;
        }

        private void button1_Click(object sender, EventArgs e)
        {
            tcpclinet = new TcpClient(textBox1.Text, 3000);
            if(tcpclinet.Connected)
            {
                ns = tcpclinet.GetStream();
                bw = new BinaryWriter(ns);
                br = new BinaryReader(ns);
                MessageBox.Show("서버 접속 성공");
            }
            else
            {
                MessageBox.Show("서버 접속 실패");
            }
        }

        private void button2_Click(object sender, EventArgs e)
        {
            bw.Write(int.Parse(textBox2.Text));
            bw.Write(float.Parse(textBox3.Text));
            bw.Write(textBox4.Text);

            intValue = br.ReadInt32();
            floatValue = br.ReadSingle();
            strValue = br.ReadString();

            string str = intValue + "/" + floatValue + "/" + strValue;
            MessageBox.Show(str);
        }
        private void Form1_FormClosing(object sender, FormClosedEventArgs e)
        {
            bw.Write(-1);
            bw.Close();
            br.Close();
            ns.Close();
            tcpclinet.Close();
        }

        private void label3_Click(object sender, EventArgs e)
        {
            ;
        }
    }
}


```

