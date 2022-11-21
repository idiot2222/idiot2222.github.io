---
title: equalst()와 hashCode()
date: 2022-11-21 16:30:00 +0900
categories: [Language, Java]
tags: [java, equals, hashCode]
---


<br/>
<br/>
<br/>
<br/>

### 참고

- [Baeldung - Guide to hashCode() in Java](https://www.baeldung.com/java-hashcode)

<br/>
<br/>


# 참조 타입의 값 비교

- 동일성 비교
  - == 사용
  - 둘이 같은 것인가를 비교한다.
  - 양쪽이 같은 주소값을 가지는지 비교하여 같으면 true, 다르면 false이다.
    - primitive 타입은 같은 값은 같은 주소 값을 가지기 때문에 == 비교하면 같다고 판단한다.
    - 참조 타입은 똑같은 값을 가지는 객체들이더라도 서로 다른 인스턴스를 참조하고 있으면 다르다고 판단한다.
- 동등성 비교
  - equals() 사용
  - 둘이 같은 값인가를 비교한다.
  - 양쪽이 같은 값을 가지면 true, 다른 값을 가지면 false이다.
    - 참조 타입에서 다른 인스턴스를 가지더라도 같은 값인지를 비교하기 위해 사용한다.

```java

class Babo {

    private String name;

    // constructor
}

```
```java

Babo babo1 = new Babo("bogeun");
Babo babo2 = new Babo("bogeun");

sout(babo1 == babo2);      // false;
sout(babo1.equals(babo2)); // false;

```

- 위 코드에서 두 바보는 이름이 같다.
  - 동일하냐를 묻는 == 비교에서는 false가 나왔다.
  - 동등한 값을 갖냐를 묻는 equals()에서도 false가 나왔다.
    - 이는 따로 equals()를 재정의해주지 않았기 때문이다.
    - Object의 equals() 메소드는 그저 == 비교만으로 끝이다.


<br/>
<br/>

# equals()

```java

class Babo {
    private String name;

    // constructor

    @Override
    public boolean equals(Object o) {
        if(this == 0) {
            return true;
        }
        if(o == null || o.getClass() != getClass()) {
            return false;
        }

        Babo babo = (Babo) o;
        return Objects.equals(this.name, babo.name);
    }
}

```
```java

Babo babo1 = new Babo("bogeun");
Babo babo2 = new Babo("bogeun");

sout(babo1 == babo2);      // false;
sout(babo1.equals(babo2)); // true;

```

- 이번에는 equals() 메서드를 재정의 해줬다.
  - 동일성 비교에서는 false
  - 동등성 비교에서는 true가 나왔다.

<br/>
<br/>

# hashCode()

- Object의 hashCode()는 무엇일까
- 결론부터 말하면 HashMap, HashSet 등 해시 테이블을 위해 사용되는 메서드이다.
  1. 해시 테이블은 해시 코드를 비교해서 같은 값인가를 판단하고
  2. 같다면 equals() 메서드를 통해 또 같은 값인가를 판단한다.

<br/>

```java

Babo babo1 = new Babo("bogeun");
Babo babo2 = new Babo("bogeun");

sout(babo1.equals(babo2));                  // true;
sout(babo1.hashCode() == babo2.hashCode()); // false;

```

- 인스턴스마다 고유의 해시 값을 가지기 때문에 동등한 객체더라도 다른 해시 코드를 갖는다.

<br/>

```java

Set<Babo> set = new HashSet<>();
set.add(babo1);
set.add(babo2);

sout(set.size());  // 2

```

- 그래서 해시 테이블을 사용하는 HashSet에서 babo1과 babo2를 다르다고 판단한다.
- Set은 중복된 값을 허용하지 않는 자료 구조인데
  - babo1과 babo2는 hashCode()의 값이 달라 다른 값으로 판단하여 2개가 온전히 들어갔다.

<br/>

```java

class Babo {
    // ...

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }
}

```
```java

Set<Babo> set = new HashSet<>();
set.add(babo1);
set.add(babo2);

sout(set.size());     // 1
sout(babo1 == babo2); // false

```

- 바보들의 해시 코드는 name 필드를 바탕으로 만들도록 재정의 해주었다.
- babo1과 babo2의 name은 bogeun으로 같고 따라서, 같은 해시 코드를 가지게 됐다.
- 이제는 해시 테이블이 babo1과 babo2를 온전히 같은 애로 취급하여 중복된 값이라 판단하고 babo1 하나만 들어가게 됐다.

- hashCode()가 같고 equals()가 ture니까 babo1과 babo2는 동일한 인스턴스일까?
  - 그건 아니다 서로 주소값이 다르기 때문에 여전히 동일성 비교에선 false가 나온다.


<br/>
<br/>
<br/>
<br/>
