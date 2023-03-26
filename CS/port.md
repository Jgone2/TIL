# PORT

PORT는 TCP와 UDP 등이 전송 계층에서 응용 계층에서 어떤 애플리케이션을 사용하는지에 대한 정보를 포함하는데 이때 이 정보를 번호로 표현한 것이 `PORT`입니다.

포트 번호는 특정 애플리케이션의 고유의 번호이며, 이 번호를 패킷에 함께 보내면 어떤 애플리케이션의 작업을 원하는지 알 수 있습니다.

대표적으로 Spring을 실행했을 때 `localhost:8080`의 로컬주소가 생기는데 이때 `8080`이 포트번호에 해당됩니다. 이 포트번호는 ip주소의 통로번호입니다. 이미 사용중인 통로를 지나갈 수 없기에 통로번호는 중복사용이 불가능합니다.

- 같은 IP 내에서 프로세스를 구분
- `0~65535`: 할당 가능
- `0~1023`: 시스템에서 사용
  - FTP: 20, 21(TCP)
  - SSH: 22(TCP)
  - TELNET: 23(TCP)
  - SMTP: 25(TCP)
  - DNS: 53(UDP)
  - HTTP: 80(TCP)
  - HTTPS: 443(TCP)

# 📚Reference

- [CodeStates](https://www.codestates.com/course/backend-engineering?gclid=Cj0KCQjw2v-gBhC1ARIsAOQdKY3aTvRr3TyPr_tiDTQV_UDtV7N-zFjWECYE16zC9b_fA9F-x-qQ1psaAoC1EALw_wcB)