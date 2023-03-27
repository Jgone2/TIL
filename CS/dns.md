# 1. 도메인 네임(Domain Name)

- 도메인 네임은 숫자 **IP 주소로 매핑되는 텍스트 문자열**
- 클라이언트 소프트웨어에서 웹 사이트에 액세스 하는데 사용됨
- **'google.com'이나 'naver.com'** 처럼 특정 웹 사이트에 접속하기 위해 입력하는 텍스트
  ![Domain Name](https://geekcrunchhosting.com/wp-content/uploads/2019/12/domain-name-structure-diagram.png)<br />
  > 💡 IP 주소: 컴퓨터 네트워크에서 장치들이 서로 인식하고 통신하기 위해서 사용하는 특수한 번호<br />
  >
  > - IPv4: IP version 4로 0~255사이의 10진수 네개를 사용하며 .로 구별한다.(192.0.2.1)
  > - IPv6: IPv4에서 주소가 부족해짐에 따라 버전6에서는 주소길이가 32비트에서 128비트로 대폭 확장되었으며, 두 자리의 16진수 여덟개를 사용하고 :기호로 구분한다.(00:00:00:00:00:00:00:00)

<br />

# 2. 도메인 네임의 체계

![DNS Server 체계](https://www.kisa.or.kr/img/dns_line.gif)
<br />
도메인은 트리 구조형태를 하고있다. 최상위에는 루트(root)가 위치하고 바로 아래 단계에 최상위 도매인인 TLD(Top Level Domain)이 존재한다. 최상위 도메인은 국가코드와 일반코드로 나뉘며, 그 하위에 도메인 네임이나 기타 도메인 문자열이 존재한다.<br />
이 체계는 DNS작동원리와 연관성이 있으므로 눈여겨 볼 필요가 있다.
<br /><br />

# 3. DNS(Domain Name System)

- DNS 서비스는 **도메인 네임을 IP주소로 변환**해 서버연결을 제어하는 역할
- 222.122.195.6과같은 긴 숫자 IP 주소가 아닌 www.naver.com 과 같이 사람이 암기하기 편한 문자 주소로 웹 사이트를 탐색할 수 있도록 도와준다.
  <br /><br />

# 4. DNS 서버

웹페이지를 로드하기 위해서 사람이 도메인 주소를 입력하게 되면 클라이언트는 **IP주소를 찾아서 값을 반환**해야한다. 그러나 전 세계의 모든 IP주소를 한 개의 서버에서 유지/관리할 수 없다.<br />
따라서 IP주소는 여러 대의 DNS서버에 나눠서 저장되어있다. 그렇기 때문에, 우리는 IP 주소를 찾기 위해 **탐색하는 과정을 필요**로 한다.<br /><br />

# 5. DNS 서버의 종류

### 01. Local DNS(Resolver, Recursive DNS)

Local DNS는 Resolver혹은 Recursive DNS라고도 쓰이며 웹 브라우저 등의 애플리케이션을 통해 **클라이언트 PC로부터 쿼리(Query)를 받아 DNS서버로 전달하고, 응답받은 정보를 클라이언트 PC로 반환**하는 역할을 수행<br />
또한 탐색한 IP주소를 탐색할 때마다 재탐색하면 비효율적이기 때문에, DNS 조회 초기에 **응답받은 레코드를 일정기간(TTL, Time to Live) 캐싱**하여 효율적으로 요청작업을 수행할 수 있도록 한다.

### 02. Root DNS

Root DNS는 도메인 네임을 IP 주소로 변환하기 위한 첫 단계다. TLD서버에 대한 주소정보를 가지고 있다.

### 03. TLD DNS

도메인 네임 체계에서 최상위 도메인인 TLD 서버다. 각 TLD문자를 가지고 있는 하위의 Authoritative DNS에 대한 주소정보를 가지고 있다.

### 04. Authoritative DNS(Name Server DNS)

**실제 도메인 네임과 IP주소의 관계가 저장**되어 있는 서버다. 일반적으로 네임 서버(Name Server)라고도 불리며 Local DNS에서 요청한 쿼리의 종착점이다.<br />
요청한 레코드에 대한 액세스 권한이 있기 때문에, 요청한 IP 주소를 Local DNS에 응답하는 역할을 수행한다.
<br /><br />

# 6. DNS 작동 원리

![DNS 작동원리](https://www.netmanias.com/ko/?m=attach&no=1997)<br />

1. 클라이언트 PC에서 Local DNS에 'www.naver.com'이라는 hostname에 대한 IP주소를 물어봄
2. 'www.naver.com'에 관한 IP 주소가 캐싱되어 있다면 곧 바로 클라이언트 PC에 값을 반환하고, 캐싱되어있지 않다면 먼저 root DNS에게 'www.naver.com'에 대한 IP 주소를 아는지 물어봄
3. root DNS는 'www.naver.com'에 대한 IP 주소를 모르기 때문에 TLD DNS인 COM DNS에게 물어보라고 응답
4. com DNS는 '.com과 관련된 도메인'을 관리하는 서버다. 이제 Local DNS는 com DNS에게가서 IP주소를 물어봄
5. com DNS 역시 IP주소를 모르기 때문에 name server DNS를 알려주며 여기에 가서 물어보라고 응답
6. Local DNS는 'www.naver.com'을 직접 관리하는 name server DNS에 가서 'www.naver.com'에 관한 IP 주소 여부를 물어봄
7. name server DNS는 'www.naver.com'도메인을 직접 관리하기 때문에 IP 주소를 보유하고 있고, 도메인 네임에 대한 IP 주소를 응답해 줌
8. 응답받은 Local DNS는 'www.naver.com'에 대한 IP 주소를 캐싱하고 IP 주소를 클라이언트 PC에게 반환해줌
   > 💡 이와 같이 Local DNS가 root DNS부터 차례대로 탐색 후 값을 도출하는 과정을 Recursive Query(재귀 쿼리)라고 한다.

<br />

# 📚Reference

- [AWS - DNS란 무엇입니까?](https://aws.amazon.com/ko/route53/what-is-dns/)
- [Cloudflare - DNS란 무엇입니까?](https://www.cloudflare.com/ko-kr/learning/dns/what-is-dns/)
- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQjw2v-gBhC1ARIsAOQdKY2Hj9THw6YromW_L-Dmtq7eQ_z47MDN-chkF-_YKrqCkJEhtPOnmngaAlumEALw_wcB)
- [HwanShell - DNS에 대한 설명](https://hwan-shell.tistory.com/320)
- [KISA - DNS](https://www.kisa.or.kr/business/address/address3_sub1.jsp)
- [DNS의 구성요소 및 기본 동작 원리](https://itsandtravels.blogspot.com/2018/11/dnsdomain-name-system-dns.html)
- [NETMANIAS - DNS 기본 동작 설명](https://www.netmanias.com/ko/?m=view&id=blog&no=5353)
- [DNS 개념 및 동작 원리](https://ja-gamma.tistory.com/entry/DNS%EA%B0%9C%EB%85%90%EB%8F%99%EC%9E%91%EC%9B%90%EB%A6%AC)
- [DNS, 네임서버 개념정리](https://gentlysallim.com/dns%EB%9E%80-%EB%AD%90%EA%B3%A0-%EB%84%A4%EC%9E%84%EC%84%9C%EB%B2%84%EB%9E%80-%EB%AD%94%EC%A7%80-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC/)
