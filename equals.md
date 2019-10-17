# equlas(), hashCode(), toString() 재정의



## 1. 객체의 일치성과 객체의 논리적 동치

### 객체의 동일성

- 객체의 일치성과 같은 의미
- 객체가 **물리적으로 같은 객체**인가를 구별한다.
- 자바에서 '==' 비교연산자를 이용해서 구분한다.
  - 원시타입의 경우 '=='의 연산자는 값이 같은지를 구별한다. 하지만 나머지 참조타입은 전부 같은 객체인지를 확인하는 동일성을 묻는 연산자이다.

### 객체의 동등성(논리적 동치)

- 객체가 나타내는 값이 같은지를 구별한다.
- 자바에서는 equals()로 확인한다.

```java
String a = new String("하나");
String b = new String("하나");

System.out.println(a == b) // false
System.out.println(a.equals(b)) // true
```

- 만약 데이터를 담고 있는 클래스를 새로 만든다면, equals()와 hashcode(), toString()은 재정의 해주어야 한다.



### 참고 String의 특징

- String는 객체를 생성하는 방법에 따라 다른 메모리에 위치한다. 리터럴을 이용하는 경우 상수풀에 생성된다. 생성자를 이용하는 경우에는 힙에 저장된다. 상수풀에 저장되는 경우에 상수풀에 같은 값이 존재하면 해당 객체를 리턴하게 되어 있다. String 상수 풀은 힙영역 밖에 있었으나, 메모리 관리 방법이 없어 힙 영역안으로 들어오게 되었고 GC의 대상이다.

``` java
String a = "a";
String b = "a";
String c = new String("a");
String d = new String("a");

System.out.println( a == b ) // true - 상수풀 안에 저장되기 때문에 같은 객체이다.
System.out.println( a.equals(b)) // true "a"라는 값이 같다. 논리적 동치
System.out.println( a == c) // false - 서로 다른 객체이다.
System.out.println( a.equals(c) ) // true 논리적 동치
System.out.println( c == d ) // false - 힙 메모리 안에 서로 다른 객체로 존재
System.out.println( c.equals(d) ) // ture - 두 객체는 논리적으로 같은 값을 나타냄
```

- intern() 메소드를 이용해서 상수풀에 생성되게 할 수 있다.



## 2. equals()

>  자바에서 모든 객체의 최상위 객체인 Object 클래스에는 equals(), hashCode(), toString()의 메소드가 구현되어 있다. final 키워드가 없어, 상속받을 시 재정의 할 것을 의미한다.

- Object 클래스에서 equals() 메소드는 객체의 주소값으로 비교하게 되어있다. 때문에 데이터 클래스가 값으로 논리적 동치성을 구별해야 한다면 equals()메소드를 꼭 재정의 해야한다.

```java
    public boolean equals(Object obj) {
        return (this == obj);
    }
```

- 실제 Object 클래스의 equals() 메소드는 다음과 같이 구현되어 있다. Object 클래스 문서 해당 메소드를 재정의 하는 원칙이 있으니 꼭 확인해 보자
- 다음 원칙에 위배되지 않게 정의 되어야 한다.

``` java
1. x.equals(x) 는 언제나 true
2. x.equals(y) 값과 y.equals(x) 는 같아야 한다.
3. x.equals(y), y.equals(z)가 같다면 x.equals(z)도 같아야한다.
4. x.equals(y)를 여러번 반복해도 항상 값이 같아야한다.
```

- equals() 메소드 재정의를 잘못할 경우 리스코브 교환의 원칙에 위반 될 수 있다. ( effective java item 10 참고)
  - 구체클래스를 확장해 새로운 값(필드 변수)을 추가하면서 equals 규약을 만족 시킬 수 있는 방법은 없다. 
  - 상속 대신 컴포지션을 사용하라
  - java의 TimeStamp는 Data 클래스를 상속 받아서 작성되었다. 리스코브원칙 치환의 법칙에 따르면 TimeStamp와 Data클래스는 같은 컬렉션에서 사용될 수 있어야 하지만, 잘못된 값을 도출할수 있어 이를 막고 있다. 이는 명백히 리스코브 치환의 원칙을 위배한 예시이다.
- 다음은 String class의 equals 메소드 재정의 이다. 

```java
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
```

- 먼저 같은 객체인지 확인하라
  - 객체가 같다면 같은 값을 가지고 있다는 것이다. 불필요한 과정을 진행하지 않아도 되서 성능 개선에 도움이 된다.
- 매개 변수로 들어온 Object 객체가 같은 String객체인지 확인한다.
  - instanceof 연산자의 경우 첫번째 피연산자가 null일 경우 무조건 false를 반환하게 되어있다.
- 대상 객체를 알맞은 형번환을 하라
- 필수 필드들이 일치하는지를 판단하여 값을 도출 하라



## 3. HashCode()

> equals 메소드를 재정의 하거든 hashcode 또한 재정의 하라

- equals를 재정의 했다면 hashcode 또한 재정의 해야한다. 재정의 하지 않을 경우 해시코드 규약을 어기게 되어 hash를 사용하는 컬렉션의 원소로 사용할 수 없다. (Effective java 아이템 11)

```java
1. 애플리케이션이 실행되는 동안 항상 일관된 값을 반환해야 한다. 
2. equals()가 두 객체가 같다고 판단 했다면, 두 객체의 hashCode가 같아야 한다.
3. equals()가 두 객체가 서로 다르다고 판단했더라고, 두 객체의 hashcode()가 서로다른 값을 반환할 필요는 없다. 단, 다른 객체에 대해서는 다른 값을 반환해야 해시테이블의 성능이 좋아진다.
```

### hash

 해시는 일정한 함수를 이용해서 나온 값을 의미한다. 이러한 해시 값을 가지고 데이터를 저장한다면, 해당 데이터에 O(log1)의 접근이 가능하다. 같은 대상에 대해서는 항상 같은 값이 나와야 한다. 하지만 다른 값이 같은 해시 값이 나올수도 있다. 다른 값에 대하여 다른 해시값이 나오도록 해시알고리즘을 정의해야 더 좋은 성능 나온다.

 만약 해시 값의 충돌이 생긴다면, 값을 저장하는 버킷에 링크드 리스트를 이용해서 저장할 수 있다. 이를 체이닝 기법이라고 한다. 만약 n개의 데이터의 해시값이 모두 같다면 하나의 버킷에 모두 저장되기 때문에 효율이 떨어진다. 다른 방법은 충돌이 일어났을 때 다른 해시 값이 나오도록 하는 것이다. 이를 개방주소법이라한다. 선형적인 방법으로 다음버킷에 저장하거나 제곱의 숫자로 떨어진 버킷에 저장할 수 있다. 아니면 두번째 해시함수를 만들어 새로운 해시값이 나오게 할 수도 있다.

- HashMap이나 HashSet 같은 해시를 사용하는 컬렉션에 새로 정의한 데이터 클래스를 사용하고 싶다면 꼭 hashCode()를 재정의 해야한다.  HashMap에서도 동치를 확인할 때 먼저 hashcode를 확인하여 다르면 동치확인을 시도 하지 않도록 구현되어 있기 때문이다.
- HashCode를 생성하는 규칙은 공개되지 않게 해야한다. 사용자가 결과값에만 의존하게 해야지 나중에 더 나은 생성규칙이 나왔을 때 이를 수정할 수 있다. 현재 Java는 많은 라이브러리에서 hashcode 생성 규칙을 알리고 있는데 이는 잘못된 행동이다.



- 다음 String의 해시코드 이다. 

  ```java
      public int hashCode() {
          int h = hash;
          if (h == 0 && value.length > 0) {
              char val[] = value;
  
              for (int i = 0; i < value.length; i++) {
                  h = 31 * h + val[i];
              }
              hash = h;
          }
          return h;
      }
  ```

  - hash 값은 lazyinit 되어 있다. 
  - 만약 hash 값이 0 이라 처음 불렸다면, 해시값을 구하는 과정에 들어간다. 
  - String의 캐릭터 숫자만큼 for문을 돌려 hash code를 만든다.
  - val[0]\*31^l + val[1]\*31^(l-1) + val[2]\*31^(l-2) ... ... + val[n-1]\*31^1 + val[n]의 식이 생긴다.
  - 31이라는 숫자는 소수이기 때문에 사용되었다. 31이라는 숫자가 일반적으로 사용된다.
  - hash 코드를 31의 다항식으로 만든 이유는 string의 각자리 숫자를 반영하기 위한 것이다. 만약 자릿수와 관련없이 해시코드를 만든다면 "abc" 와 "cab"는 같은 해시코드를 만들지도 모른다. 

  