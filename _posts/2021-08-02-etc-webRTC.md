---
layout: post
title: webRTC
categories: etc
tags: [web]
date: 2021-08-02 15:32 +0800
# toc: true
---

# WebRTC란?

WebRTC(Web Real-Time Communications)란, 웹 어플리케이션(최근에는 Android 및 IOS도 지원) 및 사이트들이 별도의 소프트웨어 없이 음성, 영상 미디어 혹은 텍스트, 파일 같은 데이터를 브라우져끼리 주고 받을 수 있게 만든 기술입니다. WebRTC로 구성된 프로그램들은 별도의 플러그인이나 소프트웨어 없이 P2P 화상회의 및 데이터 공유를 합니다.

![](https://miro.medium.com/max/1400/1*Lhsz8eckhNrXDehMo2hQyA.png)

WebRTC 기술은 P2P 통신에 최적화가 되어 있습니다.  
WebRTC에 사용되는 기술은 여러 가지가 있지만 크게 3가지의 클래스에 의해서 실시간 데이터 교환이 일어납니다.

- MediaStream — 카메라와 마이크 등의 데이터 스트림 접근
- RTCPeerConnection — 암호화 및 대역폭 관리 및 오디오, 비디오의 연결
- RTCDataChannel — 일반적인 데이터의 P2P 통신

이 3가지의 객체를 통해서 데이터 교환이 이뤄지며 RTCPeerConnection들이 적절하게 데이터를 교환할 수 있게 처리해 주는 과정을 시그널링(Signaling)이라고 합니다.

위 그림은 시그널링 하는 과정을 나타내는 것이며, PeerConnection은 두 명의 유저가 스트림을 주고 받는 것이므로 연결을 요청하는 콜러(Caller)와 연결을 받는 콜리(Callee)가 존재합니다.

> 왜 콜러(Caller)를 말하면 중국어로 “콜라”가 생각날까…?

위 그림은 시그널링 하는 과정을 나타낸 것이며, PeerConnection은 두 명의 유저가 스트림을 주고 받는 것이고 연결을 요청한 콜러(Caller)와 연결을 받는 콜리(Callee)가 존재합니다. 콜러(Caller)와 콜리(Callee)가 통신을 하기 위해서는 중간 역할을 해 주는 서버가 필요하고 서버를 통해서**SessionDescription**을 서로 주고 받아야 한다.

# 간단한 용어

다른 개발 용어와 완전히 다른 개발 용어들이 나옵니다.  
평소에 개발을 하면서 이해하기 어려운 용어들을 정리 해 보았습니다.

## Stun Server , Turn Server

Web RTC는 P2P에 최적화 되어 있습니다.  
즉 Peer들간의 공인 네트워크 주소(IP)를 알아하고 데이터 교환을 해야하는데, 실제 개개인의 컴퓨터는 방화벽등 여러가지 보호장치들이 존재하고 있습니다. 그래서 Peer들간의 연결이 쉽지 않습니다, 이렇게 서로간의 연결을 위한 정보를 공유하여 P2P 통신을 가능하게 해주는 것이 Stun/Turn Server 입니다.

[https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/](https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/) 을 읽어 보시면 도움이 많이 됩니다.

## STUN (Session Traversal Utilities for NAT )

**Session Traversal Utilities for NAT (STUN)**는 당신의 공재적 주소(Public Address)를 발견하거나 Peer 간의 직접 연결을 막는 등의 라우터의 제한을 결정하는 프로토콜 입니다. 클라이언트는 인터넷을 통해 클라이언트의 공개 주소와 라우터의 NAT 뒤에 있는 클라이언트가 접근 가능 한지에 대한 답변을 위한 요청을 STUN 서버에 보냅니다.

![](https://miro.medium.com/max/518/1*80Z67TRcEZnqHj3dWSi2cg.png)

WebRTC 프로토콜 소개 — Mozilla

## NAT(Network Address Translation)

**Network Address Translation (NAT)**는 단말에 공개 IP 주소를 할당하기 위해 사용됩니다. 라우터는 공개 IP 주소를 갖고 있고 모든 단말들을 라우터에 연결되어 있으며 비공개 IP 주소(Private IP Address)를 갖고 있습니다.  
요청은 단말의 비공개 주소로부터 라우터의 공개 주소와 유일한 포트를 기반으로 번역될 것 입니다. 이러한 경유로 각각의 단말이 유일한 공개 IP 없이 인터넷 상에 검색 될 수 있는 방법입니다.

어떠한 라우터들은 네트워크에 연결할수 있는 제한을 갖고 있습니다. 따라서 STUN서버에 의해 공개 IP주소를 발견한다고 해도 모두가 연결을 할수 있다는 것은 아닙니다. 이를 위해 TURN이 필요합니다.

## TURN(Traversal Using Relays around NAT)

몇몇의 라우터들은 Symmetric NAT이라고 불리우는 제한을 위한 NAT을 채용하고 있습니다. 이 말은 peer들이 오직 이전에 연결한 적 있는 연결들만 허용한다는 것입니다.

**Traversal Using Relays around NAT (TURN)** 은 TURN 서버와 연결하고 모든 정보를 그 서버에 전달하는 것으로 Symmetric NAT 제한을 우회하는 것을 의미합니다.  
이를 위해 TURN 서버와 연결을 한 후 모든 peer들에게 저 서버에 모든 패킷을 보내고 다시 나에게 전달해달라고 해야 합니다. 이것은 명백히 오버헤드가 발생하므로 이 방법은 다른 대안이 없을 경우만 사용하게 됩니다.

![](https://miro.medium.com/max/590/1*WSa3buqCC42Jc4Qygi9jXw.png)

WebRTC 프로토콜 소개 — Mozilla

## SDP (Session Description Protocol)

**세션 기술 프로토콜(Session Description Protocol,SDP)**은 스트리밍 미디어의 초기화 인수를 기술하기 위한 포맷입니다. 본 규격은 IETF(국제 인터넷 표준화 기구)의 RFC 4566로 규격이 공식화 되어 있습니다.  
실제로 WebRTC를 이용하여서 SDP format에 맞춰져 음성, 영상 데이터를 교환하고 있습니다.  
해상도나 형식, 코덱, 암호화등의 멀티미디어 컨텐츠의 연결을 설명하기 위한 표준입니다.

## Interactive Connectivity Establishment (ICE)

**Interactive Connectivity Establishment (ICE)**는 브라우저가 Peer를 통한 연결이 가능하도록 하게 해 주는 프레임워크입니다. Peer A ➡️ Peer B까지 단순하게 연결하는 것으로는 작동하지 않는 것에 대한 이유는 많이 있습니다.  
연결을 시도하는 방화벽을 통과 해야하기도 하고, 단말에 Public IP가 없다면 유일한 주소 값을 할당해야할 필요도 있으며 라우터가 Peer 간의 직접 연결을 허용하지 않을 때에는 데이터를 릴레이 해야할 경우도 있습니다.  
ICE는 이러한 작업을 수행하기 위해서 위해서 설명하였던 STUN, TURN Server 두개 다 혹은 하나의 서버를 사용합니다.

## RTMP(Real Time Messaging Protocol)

**리얼 타임 메시징 프로토콜**(Real Time Messaging Protocol, **RTMP**)은 어도비 시스템즈의 독점 컴퓨터 통신 규약 입니다.  
RTMP는 오디오, 비디오 및 기타 데이터를 인터넷을 통해 스트리밍할 때 쓰이고 있습니다.  
RTMP는 어도비 플래시 플레이어와 서버 사이의 통신에 이용됩니다.

# 참고

- [https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API/Protocols](https://developer.mozilla.org/ko/docs/Web/API/WebRTC_API/Protocols)
- [https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/](https://www.html5rocks.com/ko/tutorials/webrtc/infrastructure/)
- [https://juneyr.dev/webrtc-basics](https://juneyr.dev/webrtc-basics)
- [https://ko.wikipedia.org/wiki/%EC%84%B8%EC%85%98*%EA%B8%B0%EC%88%A0*%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C](https://ko.wikipedia.org/wiki/%EC%84%B8%EC%85%98_%EA%B8%B0%EC%88%A0_%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)
- [https://medium.com/@hyun.sang/webrtc-webrtc%EB%9E%80-43df68cbe511](https://medium.com/@hyun.sang/webrtc-webrtc%EB%9E%80-43df68cbe511)
