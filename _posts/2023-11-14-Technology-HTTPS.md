---
layout: post
title:  " 왜 HTTPS를 사용하나요? "
categories: Technology
author: devFancy
---
* content
{:toc}

## HTTPS를 사용하는 이유

* HTTPS에서 `S`는 secure을 의미한다. 즉, 기존의 HTTP 보다 안전하다는 얘기다.

* 무엇으로부터 안전하냐면, 크게 둘로 나뉜다.

### 누군가가 도청할 수 있는 HTTP

* 먼저, [1] 내가 어떤 사이트에 보내는 **정보를 다른 누군가 훔쳐보지 못하게 한다.**

    * 네이버에 접속해서 아이디와 비밀번호를 입력하고 로그인 버튼을 누르면,

    * 이 두 정보가 인터넷을 타고, 네이버의 서버로 전송된다.

    * 그런데 그냥 `HTTP`로 보내게 되면 이 암호가 입력한 텍스트 그대로, 누구든 알아볼 수 있는 형식으로 보내진다.

    ![](/assets/img/technology/Technology-HTTPS-1.png)

    * **만약 누군가가 이 정보를 중간에 들여다보면(도청)**, 나의 네이버 아이디와 비밀번호를 알게되어 버린다.

    * 하지만, `HTTPS`는 네이버만 알아볼 수 있는 알 수 없는 텍스트로 변경해서 보내게 된다.

    ![](/assets/img/technology/Technology-HTTPS-2.png)

    * 그러면, 누군가가 들여다봐도 뭐라도 쓴 건지 알아볼 수 없게 된다.

### 신뢰할 수 없는 HTTP

* 다른 하나는, [2] 내가 접속한 사이트가 **진품인지, 신뢰할 수 있는 사이트인지 판별**해준다.

    * HTTP를 사용한 요청, 응답에서는 통신 상대를 확인하지 않는다.

    * `네이버`를 클릭해서 들어갔는데, `네이놈`이라는 **피싱 사이트**인 경우가 있다.

    * 거기에 네이버의 아이디와 비밀번호를 입력하게 되면, 나의 네이버 계정을 알게된다.

    * 돈과 관련 있는 은행같은 온라인뱅킹 사이트의 경우 더 큰일이 나게 된다.

    * `HTTPS`는 이러한 수상한 사이트를 걸러낼 수 있도록 해준다.

    * 기관으로부터 검증된 사이트만 주소에 HTTPS 사용이 허가되고, HTTP만 사용되는 사이트들은 `"안전하지 않음"`와 같은 표시가 뜨게 된다.

    ![](/assets/img/technology/Technology-HTTPS-not-secure.png)

* 요약하자면, `HTTPS`는 [1] 내가 사이트에 보내는 정보들을 제 3자가 못보게 하고, [2] 접속한 사이트가 믿을 만한 곳인지를 알려준다.

## 대칭키와 비대칭키

* 위에서 설명한 두 보안 기능이 어떤 원리로 구현되는지 알아보기 이전에 대칭키와 비대칭키 개념부터 알아보자.

### 대칭키

* `대칭키`는 동일한 키로 `암호화`와 `복호화`를 같이할 수 있는 방식의 암호화 기법을 의미한다.

* 암호화와 복호화를 위해 양쪽이 같은 키를 가져야 한다는 점에서 이 키를 `대칭키`라고 한다.

> 암호화: 어떤 정보를 외부에 노출시키지 않기 위해 변형하는 것을 의미한다.
>
> 복호화: 반대로 암호화된 데이터를 원본으로 복원하는 것을 의미한다.

![](/assets/img/technology/Technology-HTTPS-3.png)

* 네이버에 로그인하는 상황을 가정해보면, 내가 로그인할 때 아이디와 비밀번호를 **대칭키**로 이용해서 `암호화`하고, 

* 네이버에서는 이를 `복호화`해서 인식할 수 있다. 

* 그러면 중간에 누가 이걸 훔쳐보더라도 알아볼 수 없게 된다.

* 그런데 대칭키는 **큰 단점**이 있다.

* 어떻게 이 동일한 키를 애초에 양쪽이 공유하느냐는 것이다.

* 결국 한 번은 한쪽에서 다른 한쪽으로 `대칭키`를 전달해야 하는데, 이 과정에서 제 3자로부터 `대칭키`가 탈취된다면, 제 3자도 정보를 복호화해서 알 수 있게 된다.

* 이러한 한계점을 보완하기 위해 1970년대 수학자들에 의해 개발되어 나온 암호화 방식이 `비대칭키`(공개키)다. (개발자들 사이에서는 `공개키`라고 부른다)

### 비대칭키(공개키)

* 비대칭키는 키가 두개가 있다.

* A키로 암호화하면 B키로 복호화할 수 있고, B키로 암호화하면 A키로 복호화하는 방식이다.

> 이후부터는 공개키(=비대칭키)라고 부르는게 더 이해하기 쉬울 것 같아서 `공개키`로 부르겠습니다.

* 이 방식에 착안해서 두개의 키 중 하나를 공개키, 다른 하나를 비공개 키로 지정한다.

* 아래 그림을 통해 자세히 살펴보면 다음과 같다.

![](/assets/img/technology/Technology-HTTPS-4.png)

* 네이버 서버는 이 두개의 키들 중 하나는 비밀로 보관하고(개인키), 다른 하나를 대중에게 공유하는 공개키를 제공한다고 가정했을 때,

* 우리가 비밀번호를 암호화해서 네이버에 보내는 과정에서, 중간에 누군가가 가로채도 **같은 `공개키`로는 해당 암호문을 풀 수가 없다.**

* 이 암호문을 볼 수 있는 건 **`비공개키(개인키)`** 를 가진 네이버 회사만 가능하다. 

* 즉, **`비공개키(개인키)`가 없으면 `복호화`가 불가능하기 때문에 대칭키 방식의 단점을 극복**할 수 있다.

* 이 원리로 이제 사용자는 개인 정보들을 안심하고 네이버에 보낼 수 있다.

> 그런데, 이 사이트가 네이버라는 걸 어떻게 증명하나요?

* 네이버에서 우리에게 보내는 정보들은 그 일부가 이 **네이버의 `개인키`로 암호화**가 되어 있다.

* 우리가 네이버의 `공개키`로 풀어서 알아볼 수 있는 건 **네이버의 `개인키`로 암호화된 정보들** 뿐이다.

* **신뢰할 수 있는 기관**에게 네이버의 공개키만 검증해주면, 이 기준으로 안전하게 네이버를 이용할 수 있다.

    * 여기서 신뢰할 수 있는 기관은 민간 기업으로 `CA`(Certificate Authority)라고 부른다.

* 하지만 공개키 방식의 문제점은 대칭키 방식보다 암호화 연산시간이 더 소요되어 비용이 크다.

* 결론적으로 대칭키 방식의 장점이 공개키 방식의 단점이 되고, 대칭키 방식의 단점이 공개키 방식의 장점이 된다.

* 그래서 `SSL`은 **각 방식이 가진 단점 때문에 하나의 방식만 채택하지 않고 두 방식 모두 적절히 섞어서 사용**한다.

> SSL(Secure Socket Layer): Netscape Communications Corporation에서 서버와 웹 브라우저간의 보안을 위해 만든 프로토콜로, 공개키 방식과 대칭키 방식을 혼합해서 사용한다.
>
> SSL은 서버와 브라우저 사이에 안전하게 암호화된 연결을 만들 수 있게 도와주고, 서버와 브라우저가 민감한 정보를 주고받을 때 해당 정보가 도난당하는 것을 막아준다.
>
> 참고로 SSL 3.0버전은 IETF에서 표준으로 제정되어 TLS라는 이름을 갖게 되었다.

## SSL 통신 과정

`SSL`은 공개키 방식과 대칭키 방식을 적절히 혼합해서 사용하는데, 이를 보다 더 구체적으로 정리하면, 

1. `SSL`은 **공개키 방식으로 대칭키를 안전하게 전달**한다.

2. 그리고 **이 대칭키를 활용**해서 암호화와 복호화를 하고 서버와 브라우저간 통신을 한다.

3. 통신이 끝나면 종료한다.

이렇게 됨으로써 대칭키를 중간에 탈취당하지 않고, 공개키 방식보다 빠르게 통신할 수 있게 된다.

SSL 통신 과정을 살펴보면 아래와 같은 세가지 과정으로 진행된다.

> `HandShake` -> 통신 -> 통신 종료

앞서 이야기한 것처럼 HTTP는 통신 상대를 확인하지 않는다. 클라이언트가 서버로부터 받은 공개키가 진짜인지 가짜인지 판별하기 위해 `CA`(Certicifate Authority)라는 제 3자를 통해 가능하다.

`CA`는 정말 엄격한 인증 과정을 거쳐야 될 수 있다고 한다. 이 과정을 조금 더 자세히 알아보자. 

![](/assets/img/technology/Technology-HTTPS-5.png)

[1] 먼저 서버는 `CA`에게 자신의 `공개키`를 건넨다. 

  * 그러면 CA는 서버의 공개키를 `CA의 개인키`로 암호화하여 서명한다.

[2] 이렇게 암호화된 것을 **`공개키 인증서(Public Key Certificate)`** 라고 한다. 서버는 이 `공개키 인증서`를 클라이언트에게 보낸다.

### HandShake(악수)

`SSL 통신`은 데이터를 주고 받기 이전에 어떻게 데이터를 암호화할지, 믿을 만한 서버인지 등에 대한 과정을 확인한다.

[3] 클라이언트가 서버에 접속한다. 이 단계를 `Client Hello` 라고 한다. 이 단계에서 주고 받은 정보는 아래와 같다.

  * 클라이언트 측에서 생성한 랜덤 데이터

  * 클라이언트가 지원하는 암호화 방식들 ⇒ 클라이언트가 가능한 암호화 방식을 서버에 알려주기 위함

[4] 서버는 `Client Hello` 에 대한 응답으로 `Server Hello` 를 하게 된다.(이때, 클라이언트에게 `공개키 인증서` 전달) 이 단계에서 주고 받은 정보는 아래와 같다.

  * 서버 측에서 생성한 랜덤 데이터

  * 서버가 선택한 클라이언트의 암호화 방식 ⇒ 선택한 암호화 방식을 클라이언트에게 알려주기 위함

[5] `클라이언트`는 `서버의 인증서`가 `CA`에 의해 발급된 것인지 확인한다. 이때, **브라우저에 내장된 `CA리스트와 CA 공개키`를 사용해서 인증서를 복호화**한다.

   * 성공적으로 인증서가 복호화 됬다면, 서버가 전달한 `공개키 인증서`가 `CA의 개인키`로 암호화된 문서임이 보증된 것이다.

   * **즉, 올바른 서버임을 신뢰 할수 있게된다.**

  * 서버를 신뢰할수 있으므로 클라이언트는 `서버가 생성한 랜덤 데이터`와 `클라이언트가 생성한 랜덤 데이터`를 조합하여 `pre master secret`이라는 키를 생성한다.

  * 이때, `공개키 방식`을 사용한다.

[6] `서버의 공개키`를 전달받은 클라이언트는 `pre master secret`를 암호화해서 서버로 전송한다.

  * 서버는 자신의 비공개키를 통해 `pre master secret`를 복호화한다.

  * 이를 통해, 클라이언트와 서버는 안전하게 같은 `pre master secret`를 가진다.

[7] 서버와 클라이언트가 공개키 방식을 통해 `pre master secret`를 `master secret`라는 `대칭키`를 생성한다.

  * 클라이언트는 서버로부터 받은 공개키를 이용하여 자신의 대칭키를 암호화한다.

  * 클라이언트는 이렇게 암호화된 대칭키를 서버에게 전달한다.

  * 서버는 전달받은 암호화된 대칭키를 자신의 비공개키(개인키)를 통해 복호화하고, 이 복호화로 인해 클라이언트로부터 대칭키를 얻을 수 있게 된다.

* 그리고 클라이언트와 서버는 `HandShake`가 종료되었음을 서로에게 알린다.

### 통신

* 이 대칭키를 통해 실제로 클라이언트와 서버가 암호화된 메시지를 주고받을 수 있게 된다.

    다른 말로 표현하면, `master secret`라는 `대칭키`를 통해 클라이언트와 서버는 데이터를 암호화/복호화하면서 주고 받게 된다.

### 통신 종료

* 데이터의 전송이 끝나면 `SSL 통신`이 끝났음을 서로에게 알려준다.

* 그리고 사용한 대칭키인 `master secret`은 폐기한다.

## 마치며

* 지난 8-9월에 굿프렌즈 프로젝트에서 백엔드와 프론트엔드 서버에 HTTPS를 설정하는 작업을 진행했었다.

* HTTPS의 중요성과 그에 기반한 지식들을 복습하기 위해 이 글을 작성하게 되었다.

* 처음에는 완전히 이해가 안됐지만, 계속 보다보니 이해가 점점 되어가는 내 자신에게 뿌듯함을 안겨줬다.

* HTTPS에 대한 더 깊은 지식은 나중에 이어 학습하고 정리하자.

## Reference

* [HTTPS가 뭐고 왜 쓰나요? (Feat. 대칭키 vs. 비대칭키)](https://www.youtube.com/watch?v=H6lpFRpyl14&t=2s&ab_channel=%EC%96%84%ED%8C%8D%ED%95%9C%EC%BD%94%EB%94%A9%EC%82%AC%EC%A0%84)

* [[10분 테코톡] 🍭 다니의 HTTPS](https://www.youtube.com/watch?v=wPdH7lJ8jf0&ab_channel=%EC%9A%B0%EC%95%84%ED%95%9C%ED%85%8C%ED%81%AC)

* [HTTPS 간단히 알아보기](https://hudi.blog/https/)

* [생활코딩 - SSL 통신과정](https://www.youtube.com/watch?v=8R0FUF_t_zk&t=178s&ab_channel=%EC%83%9D%ED%99%9C%EC%BD%94%EB%94%A9)