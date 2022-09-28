---
title: Exception
date: 2022-09-28 17:30:00 +0900
categories: [Language, Java]
tags: [java, exception]
---



<br/>
<br/>
<br/>
<br/>

# Checked & Unchecked

![img-description](assets/img/posting/java/checked_unchecked_exception.png)
_Checked Exception & Unchecked Exception_

- 자바의 예외 처리는 두 가지로 구별할 수 있다. 위의 그림을 보면 쉽게 구분할 수 있다.
  - Checked Exception
  - Unchecked Exception
- 둘의 차이는 예외가 발생할 수 있는 상황과 관련이 있다.
  - Checked Exception은 예상이 가능하다.
    - 이미지 파일을 가져올거야, 근데 없으면 defaulst image로 대체해. => 예측과 복구가 이뤄짐
  - Unchecked Exception은 예측이 어렵다.
    - 프로그램이 돌아가던 중에 메모리가 부족하네? => 예측할 수 없어 언제 catch하고 복구할지 정해둘 수가 없다.
    - 메소드 인자로 null이 들어왔네? => 얘는 예측이 가능한 경우는 복구할 수 있지만, 너무 광범위하게 일어남.

- 둘은 서로 다른 작성 룰이 있다.

<br/>
<br/>

## Checked Exception

- Checked Exception을 throws 할 수 있는 메서드를 작성한 경우,
  - 메서드 시그니처에 throws 절을 사용해서 예외를 선언해야 한다.
    - Checked Exception을 던지는데 throws 절을 선언하지 않으면, 컴파일 에러가 뜬다.
    - 이 부분 때문에 Checked 라는 확인 되었다는 이름이 붙음.
- 내가 작성한 메서드에서 사용된 다른 메서드가 Checked Exception을 throw 할 수 있는 경우
  - 내가 작성한 메서드가 Checked Exception을 throw 할 가능성이 없어도, thorws절로 선언해줘야 한다.
  - 또는, try/catch 문으로 exception 처리를 해줘야 한다.

<br/>

- Checked Exception은 두 가지 선택지가 있다.
  - 내가 여기서 예외를 잡거나, (try/catch)
  - 나를 호출하는 너한테 떠넘기거나. (throws)
- 이러나 저러나 Checked Exception은 예외 처리가 필수이다.

<br/>

```java
// Checked Exception을 던질 가능성이 있는 경우.
public void test1() throws CheckedException {
    throws new CheckedException();
}

// Checked Exception이 발생해도 예외 처리를 한 경우.
public void test2() {
    try {
        throws new CheckedException();
    } catch(Exception e) {
        // 예외 처리
    }
}

// 여러 개의 Checked Exception이 발생할 수 있는 경우.
public void test3() throws CheckedException, BaboCheckedException {
    throwChecked();
    throwBabo();
}

// 여러 개의 Checked Exception이 발생할 수 있지만, 예외 처리로 잡아준 경우.
// 바보만 잡아줬으니 CheckedException 얘는 throws로 선언해줘야 한다.
public void test4() throws CheckedException {
    throwChecked();

    try {
        throwBabo();
    } catch(BaboException e) {
        // 예외 처리
    }
}
```
- Checked Exception을 throw 할 가능성이 있는 경우에는 메서드 시그니처에 thorws 절로 어떤 CheckException을 throw 할 수 있는지 명시 해줘야 한다.
- Checked Exception을 throw 할 가능성은 있지만, 빠르게 처리해버리면 throws 절로 명시하지 않아도 된다.
- 만약 해당 메서드가 여러 종류의 Checked Exception을 throw 할 가능성이 있다면, 여러 개 넣어주면 된다.
  역시나 여러 개여도 try/catch로 예외 처리를 해주면 명시하지 않아도 된다.

<br/>
<br/>


## Unchecked Exception

- Unchecked Exception가 throw 되더라도 throws문을 작성할 필요가 없다.
  - Unchecked Exception은 어디에서도 일어날 수 있기 때문에, 따로 예측할 필요가 없다.
- Unchecked Exception은 try/catch로 잡을 수도 있다.
  - Unchecked Exception 역시 예측만 할 수 있다면 잡을 수도 있다.
  - 예를 들어, NullPointException을 처리하는 경우가 가장 많을 것이다.
- Unchecked Exception은 잡지 못할 수도 있다.
  - Unchecked Exception은 프로그래밍 내에서는 해결하지 못하는 경우가 있다.
  - 예를 들어, OutOfMemoryError의 경우 예측하고 catch 해봤자, 프로그래밍 레벨에서 해결할 수 있는게 없다.
  - 그렇지만 try/catch로 로깅 정도는 가능하다고 한다.




<br/>
<br/>
<br/>
<br/>
