# 6주차 과제 : 상속 

## 자바 상속의 특징
상속이란, 부모 클래스의 멤버(필드, 메소드)를 자식 클래스에게 물려주는 것으로 이미 잘 만들어진 클래스를 재사용하여 새로운 클래스를 만들어 코드의 중복을 줄이는 방법을 말한다. 

부모 클래스의 모든 멤버를 물려받는 건 아니고, 부모 클래스에서 private 접근 제어자를 갖는 멤버는 제외된다. 그 외의 경우는 모두 상속의 대상이 된다. 

```
class 자식클래스 extends 부모클래스 {
    // 필드
    // 생성자
    // 메소드 
}
```
위의 형식처럼 extends 키워드를 이용하여 상속을 할 수 있다. 

다른 언어와 달리 자바는 다중 상속을 허용하지 않는다. 즉 여러 개의 부모 클래스를 상속할 수 없고 단 하나의 부모 클래스만 상속할 수 있다. 

## super 키워드

자바에서 자식 객체를 생성하면 먼저 부모 객체가 생성되고 나서 자식 객체가 생성된다. 
```
class Phone{

}

class FolderblePhone extends Phone{

}
```
FolderblePhone 클래스가 Phone 클래스를 상속하는 클래스라고 해보자. 

```
FolderblePhone folderblePhone = new FolderblePhone(); 
```
위 코드는 FolderblePhone 객체만 생성하는 것으로 보이지만 내부적으로는 부모인 Phone 객체가 먼저 생성되고, FolderblePhone 객체가 생성된다. 

모든 객체는 생성자를 호출해야만 생성되는데 부모의 생성자는 자식의 생성자 안에서 호출된다. 
명시적으로 생성자를 만들지 않았다면 컴파일러가 자동으로 자식의 생성자 첫줄에 super(); 라고 super 키워드를 이용해 부모의 생성자를 호출한다. 
```
public FolderblePhone(){
    super();
}
```
super(); 은 다음과 같은 부모의 기본 생성자를 호출한다. 여기서 super는 this와 비슷한 키워드이며 부모 객체를 참조하는 데 사용한다. 
```
public Phone(){
}
```

직접 자식 생성자를 선언하고 호출하고 싶다면 마찬가지로 생성자 첫줄에 super(arg1, arg2,...); 로 부모의 생성자를 명시적으로 호출하고 진행하면 된다. 

## 메소드 오버라이딩

자식 클래스는 부모 클래스로부터 상속받는 메소드 중 어떤 메소드는 사용하기 불편할 수 있다. 이런 메소드들은 조금씩 수정해서 사용해야 한다. 이럴 때 쓰는게 메소드 오버라이딩(메소드 재정의)이다. 

메소드 오버라이딩은 자식 클래스에서 상속받은 메소드를 다시 정의해서 쓰는 것을 말한다. 오버라이딩을 하면 자식 객체에서 메소드를 호출 시, 오버라이딩된 자식 메소드가 호출된다. 

오버라이딩할 때는 규칙이 몇가지 있다. 
- 부모의 메소드와 동일한 메소드 시그니처(리턴 타입, 메소드 이름, 매개변수 리스트)를 가져야 한다. 
- 더 좁은 범위의 접근 제어자를 사용할 수 없다. 
- 새로운 예외(Exception)을 throw 할 수 없다. (나중에 배우는 내용)

```
class Phone{
    public void print(){
        System.out.println("hello");
    }
}

class FolderblePhone() extends Phone{
    @Override
    public void print(){
        System.out.prinln("hello folderblephone!!!!");
    }
}
```

위와 같이 메소드 오버라이딩을 할 수가 있다. 자식 메소드의 @Override 어노테이션은 생략할 수 있으나 이것이 있으면 메소드가 제대로 오버라이딩 되는지 컴파일러가 체크해주기 때문에 개발자의 실수를 줄일 수 있다. 

## 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

메소드 디스패치(Method Dispatch)란 어떤 메소드를 호출할지 결정하여 실제로 실행시키는 과정을 말한다. 

다이나믹 메소드 디스패치는 인터페이스 혹은 추상 클래스에서 오버라이드 받은 추상 메소드를 호출하고 실행하는 경우를 말한다. 
```
public class Car {
  public void speedUp() {
    System.out.println("Car speedUp!!!");
  }
}

public class SportsCar extends Car {
  @Override
  public void speedUp() {
    System.out.println("SportsCar speedUp!!!");
  }
}

public class Main {

  public static void main(String[] args) {
    Car a = new Car(); // Car 참조, Car 객체
    Car b = new SportsCar(); // Car 참조, SportsCar 객체

    a.speedUp(); // Car 클래스에 정의된 메서드 실행
    b.speedUp(); // SportsCar 클래스에 정의된 메서드가 실행됨(다이나믹 메소드 디스패치)
  }
}
```
이 결과는 다음과 같이 출력된다. 
```
Car speedUp!!!
SportsCar speedUp!!!
```
이런 식으로 변수 a, b 둘 다 참조 타입은 Car 지만, JVM이 객체 타입을 확인하고 그 객체에 정의된 메소드를 실행한다. 

## 추상 클래스

> 추상 클래스란 추상 메소드를 한 개라도 가지고 있는 클래스를 말한다. 

여기서 추상 메소드란? 
- 구현부를 아직 가지지 않은, 선언부만 가진 메소드 
```
public abstract class Car {
  public abstract void move();
}
```
위 코드와 같이 선언부로만 이루어진 메소드를 추상 메소드라 한다. 

추상 클래스는 위 코드에서 잠시 봤다시피 abstract 키워드를 사용해서 표시한다. 추상 메소드에도 앞에 abstract 키워드를 붙인다. 


추상 클래스를 상속받는 클래스는 필수적인 추상 메소드의 구현 의무를 가진다. 또한 자바는 단일 상속만을 지원하기 때문에 추상 클래스를 상속하면, 다른 클래스를 상속할 수 없다. 






### 추상 클래스를 사용하는 이유 
1. 실체 클래스들의 공통된 필드와 메소드의 이름을 통일할 목적 

2. 실체 클래스를 작성할 때 시간을 절약 


## final 키워드



## Object 클래스



