---
layout: post
title: C# Ping
subtitle: Pinging a Network Directory
image: /img/CSharp.png
tags: [C#, Ping, Network Directory, NetworkInformation, new Ping(), 서버 연동, 핑, ]
---

내가 느끼기에 C++/C# 같은 언어로 프로그램을 짜서 네트워크 폴더에 접근 하는 것은 고려해야 될 것들을 엄청 많이 늘려준다.
Credentials.. Connectivity.. UNC.. 등등등 

일단 가장 기본적이면서도 가장 중요한건 Connectivity가 아닐까? 
연결이 되어야 뭘 하던 하겠지?

구글에서 찾아보면 StackOverflow/Reddit에서 몇몇 사람들은 Directory.Exists 아니면 File.Exists로 파일/폴더에 접근을 시도해서 접근이 가능하면 연결 OK, 안되면 NO. 

이렇게 판단 하는 사람들도 종종 볼 수 있다.

어찌보면 가장 간단한 방법일 수도 있으나.. 내가 직접 시도해본 결과, 느리다. 

빠르게 확인해서 빠르게 다른 대안을 찾아야 되는 것 아닌가? 

그럴때는 위 방법이 적합하지 않는 것은 너무나도 뻔하다. (제대로 테스트 해봤다면...)

따라서, 다른 대안은 무엇이 있을까? 

시작 -> 실행 -> cmd -> ping "네트워크 주소" 

위와 같이 진행해보면 알 수있지 않을까? 

핑이 가면 연결할 수 있는거고 (맞는 네트워크 자격이 있다면!)

핑이 안가면 자격이고 모고 연결할 수 없는거다.

그래서 아래의 함수를 구현해보았다. 

~~~
using System.Net.NetworkInformation;
using System.Text;
using System.Net;
 
private bool Check_Network_Status(string Server_Address)
{
	try
	{
		Ping ping = new Ping();
		PingOptions options = new PingOptions();
		options.DontFragment = true;
		string data = "hello";
		byte[] buffer = ASCIIEncoding.ASCII.GetBytes(data);
		int timeout = 120;
		PingReply reply = ping.Send(IPAddress.Parse(Server_Address), timeout, buffer, options);
		if (reply.Status == IPStatus.Success)
			return true;
		else
			return false;
	}
	catch (Exception ex) 
	{ 
		//Exception 처리
	}
	return false;
}
~~~

핑 기다릴 시간은 아래 시간을 조정해주면 되며

{: .box-note}
int timeout = "시간";

핑을 보내서 답변이 정상적으로 오면 return true를

답변이 오지 않으면 return false를 리턴해준다. 

*이 함수를 부르기전에 네트워크 폴더에 연결 하는 것을 잊지 말자*

-WooJinSI