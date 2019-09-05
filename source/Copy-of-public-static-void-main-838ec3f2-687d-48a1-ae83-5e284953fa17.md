# Copy of public static void main

# [[Java] public static void main(String [] args)](https://devbox.tistory.com/entry/Java-public-static-void-mainString-args)

[https://devbox.tistory.com/entry/Java-public-static-void-mainString-args?category=574549](https://devbox.tistory.com/entry/Java-public-static-void-mainString-args?category=574549)

**public static void main(String[] args)**

- 메인 메서드는 진입점(Entry Point)을 뜻한다. 그러므로 메인 메서드의 접근자는 항상 public 이어야 한다. –> public

- 메인 메서드는 항상 정적이어야 한다. 클래스는 메모리에 로딩된 다음에 사용이 가능하다. static이 붙은 클래스나 메서드, 변수는 컴파일시 자동으로 로딩된다. 메인 메서드는 클래스 로딩 없이 호출할 수 있어야 한다. 그렇기 때문에 static을 사용한다. –> static

- void는 리턴타입이 없다는 뜻이다. 메인 메서드는 Entry Point이면서 프로그램의 끝이기도 하다. 메인으로 시작해서 메인이 끝나면 그 프로그램도 끝이다. 그러므로 리턴하는 값 자체가 불필요하다. 프로그램이 끝났는데 마지막에 어떤 값을 리턴해봤자 아무 의미가 없기 때문이다. –> void

- String[] args 는 프로그램 실행시 매개변수를 보내서 실행할 수 있다는 것을 뜻한다. 1개를 사용할수도 있고 여러개를 사용할 수도 있기 때문에 배열을 사용한다.

출처:

https://devbox.tistory.com/entry/Java-public-static-void-mainString-args?category=574549

[장인개발자를 꿈꾸는 :: 기록하는 공간]