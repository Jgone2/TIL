# 예외처리(Exception handling)

프로그램을 작성할 때 문법에 맞지 않게 작성하고 컴파일하거나 또는 어떤 원인에 의해서 예상치 못한 오류가 발생할 수 있습니다. 이렇게 시스템이 동작하는 도중에 예상하지 못한 곳에서 오작동하는 것을 오류/에러라고 하고 이것을 `오류`와 `예외`로 나눠서 볼 수 있습니다.

# 1. 오류(error)와 예외(exception)

먼저 `오류(error)`는 시스템 레벨에서 프로그램에 문제가 발생해 실행중인 프로그램이 종료되는 것을 말합니다. 실행 중 발생하는 오류에 대해서는 예측하고 대비할 수 없습니다.

`예외(exception)`는 오류와 마찬가지로 실행중인 프로그램이 비정상적으로 종료되지만 다음과 같은 예측 가능하여 처리할 수 있도록 설계할 수 있는 것을 말합니다.

- 사용자의 입력오류
- 네트워크 연결 끊김
- 메모리 부족

# 📚 Reference

- [Java의 정석](https://product.kyobobook.co.kr/detail/S000001550352)
- [TCP School](http://tcpschool.com/java/intro)
- [CodeStates](https://www.codestates.com/course/backend-engineering)
