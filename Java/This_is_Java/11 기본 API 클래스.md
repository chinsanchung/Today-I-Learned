# 11장 기본 API 클래스

## 자바 API 도큐먼트

API(Application Programming Interface)는 라이브러리라고 부르기도 하는데, 프로그램 개발에 자주 사용하는 클래스 및 인터페이스의 모음을 말한다.
API 도큐먼트는 쉽게 API 를 찾아 이용할 수 있도록 문서화한 것을 말한다. API 도큐먼트는 [HTML 페이지](http://docs.oracle.com/javase/8/docs/api/)에서 읽을 수 있다.

## java.lang 과 java.util 패키지

### java.lang 패키지

자바 프로그램의 기본적인 클래스를 담고 있는 패키지로 import 없이 클래스와 인터페이스를 사용할 수 있다.

- Object: 자바 클래스의 최상위 클래스로 사용한다.
- System: 키보드로부터 데이터를 입력받을 때, 모니터로 출력할 때, JVM 을 종료할 때, 가비지 컬렉터를 실행 요청할 때 사용한다.
- Class: 클래스를 메모리로 로딩할 때 사용한다.
- String: 문자열을 저장하고 여러 가지 정보를 얻을 때 사용한다.
- StringBuffer ,StringBuilder: 문자열을 저장하고 내부 문자열을 조작할 때 사용한다.
- Math: 수학 함수를 이용할 때 사용한다.
- Wrapper: 기본 타입의 데이터를 갖는 객체를 만들 때, 문자열을 기본 타입으로 변환할 때, 입력값 검사에 사용한다.
  - byte, short, character, integer, float, boolean, long

### java.util 패키지

컬렉션 클래스들이 대부분을 차지한다.

- Arrays: 배열을 비교, 복사, 정렬, 찾기를 할 때 사용한다.
- Calandar: 운영체제의 날짜와 시간을 얻을 때 사용한다.
- Date: 날짜와 시간 정보를 저장하는 클래스.
- Objects: 객체 비교, null 여부 등을 조사할 때 사용한다.
- StringTokenizer: 특정 문자로 구분된 문자열을 뽑아낼 때 사용한다.
- Random: 난수를 얻을 때 사용한다.

## Object 클래스

클래스를 선언할 때 extends 키워드로 다른 클래스를 상속하지 않으면 임시로 java.lang.Object 클래스를 상속하게 된다. 따라서 자바의 모든 클래스는 Object 클래스의 자식 또는 자손 클래스이다.(Object 는 자바의 최상위 부모 클래스이다.)

### 객체 비교

equals() 메소드로 두 객체가 논리적으로 동일한 객체이면 true, 아니면 false 를 리턴한다. 논리적으로 동등하다는 뜻은 객체이건 다른 객체던 상관없이 객체가 저장하고 있는 데이터가 동일한 경우를 뜻한다.

예시로 String 객체의 equals() 메소드는 재정의(오버라이딩)을 해서 번지를 비교하지 않고 문자열 비교로 변경했기 때문에 Object 의 equals() 메소드와 다르다.
Object 의 equals() 메소드는 직접 사용하지 않고 하위 클래스에서 재정의하여 논리적으로 동등 비교할 때 이용한다.

equals() 메소드를 재정의할 때는 매개값(비교할 객체)이 기존 객체와 동일 타입인지 아닌지 instancoof 연산자로 확인부터 한다.

```java
public class Member {
    public String id;
    public Member(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member) obj; // Member 타입으로 강제 타입 변환
            if (id.equals(member.id)) {
                return true;
            }
        }
        return false; // id 필드값이 다르면 false 리턴
    }
}
```

```java
public class MemberExample {
    public static void main(String[] args) {
        Member obj1 = new Member("blue");
        Member obj2 = new Member("blue");
        Member obj3 = new Member("red");

        if (obj1.equals(obj2)) {
            System.out.println("obj1 == obj2");
        } else { System.out.println("obj1 != obj2"); }

        if (obj1.equals(obj3)) {
            System.out.println("obj1 == obj3");
        } else { System.out.println("obj1 != obj3"); }
    }
}
'''
obj1 == obj2
obj1 != obj3
'''
```

### 객체 해시코드

객체 해시코드는 객체를 식별할 하나의 정수값을 말한다. Object 의 hashcode() 메소드는 객체의 메모리 번지로 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가진다.
논리적 동등 비교 시 hashCode() 를 오버라이딩할 필요가 있다.

```java
public class Key {
    public int number;

    public Key(int number) {
        this.number = number;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Key) {
            Key comapreKey = (Key) obj;
            if (this.number == compareKey.number) {
                return true;
            }
            return false;
        }
    }
}
```

이런 경우 HashMap 식별키를 Key 객체로 사용하면 저장한 값을 읽어오지 못한다. number 필드값이 같더라도 hashCode() 메소드가 리턴하는 해시코드가 다르기 때문에 다른 식별키로 인식해서이다.

```java
public class KeyExample {
    public static void main(String[] args) {
        // Key 객체를 식별키로 해서 String 값을 저장하는 HashMap 객체
        HashMap<Key, String> hashMap = new HashMap<Key, String>();

        // 식별키 "new Key(1)"으로 "길동"을 저장
        hashMap.put(new Key(1), "길동");
        // 식별키 "new Key(1)"으로 "길동"을 읽어옴
        String value = hashMap.get(new Key(1));
        System.out.println(value)// null
    }
}
```

의도대로 "길동"을 읽으려면 재정의한 hashCode() 메소드를 Key 클래스에 추가한다. 저장할 때의 "new Key(1)", 읽을 때의 "new Key(1)"는 사실 서로 다른 객체지만 HashMap 은 hashCode()의 리턴값이 같고, equals() 리턴값이 true 이기 때문에 동등 객체로 평가한다. (즉 같은 식별키로 인식한다.)

```java
public class Key {
    @Override
    public int hashCode() { return number; }
}
```

결론적으로 객체의 동등 비교를 위해서는 Object 의 equals() 메소드만 재정의하지 말고 hashCode() 메소드도 재정의해서 논리적 동등 객체라면 동일한 해시코드가 리턴되도록 해야 한다.

```java
public class Member {
    public String id;

    public Member(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (obj instanceof Member) {
            Member member = (Member) obj;
            if (id.equals(member.id)) {
                return true;
            }
        }
        return false;
    }
    @Override
    public int hashCode() {
        return id.hashCode();
    }
}
```

### 객체 문자 정보(toString())

Object 클래스의 toString() 메소드는 객체의 문자 정보(객체를 문자열로 표현한 값. 기본적으로 "클래스명@16진수해시코드"로 구성되어 있다.)을 리턴한다.

Object 하위 클래스는 toString() 메소드를 재정의해서 간결하고 유익한 정보를 리턴하도록 되어있다.

```java
public class ToStringExample {
    public static void main(String[] args) {
        Object obj1 = new Object();
        Date obj2 = new Date();
        System.out.println(obj1.toString()); // Object@1b15692
        System.out.println(obj2.toString()); // 요일 월 일 HH:MM:SS KST 연도
    }
}
```

아까 만든 SmartPhone 클래스의 toString() 메소드를 오버라이딩해서 제작회사와 운영체제를 리턴하도록 만든다.

```java
public class SmartPhone {
    private String company;
    private String os;

    public SmartPhone(String company, String os) {
        this.company = company;
        this.os = os;
    }
    @Override
    public String toString() {
        return company + ", " + os;
    }
}
```

```java
public class SmartPhoneExample {
    public static void main(String[] args) {
        SmartPhone phone = new SmartPhone("google", "android");
        String strObj = phone.toString();
        System.out.println(strObj); // google , android

        System.out.println(phone); // phone.toString() 을 자동 호출해 리턴값을 출력
    }
}
```

System.out.println 은 매개값이 기본 타입이면 해당 값을 그대로 출력하고, 객체면 그 객체의 toString() 메소드를 호출해서 리턴값을 받아 출력한다.

### 객체 복제(clone())

1. 얕은 복제

얕은 복제는 단순히 필드값을 복사해서 객체를 복제하는 것을 말한다. 필드가 기본 타입이면 값 복사를, 참조 타입이면 격체의 번지를 복사한다.
Object 의 clone() 메소드는 자신과 동일한 필드값을 가진 얕은 복제된 객체를 리턴한다. 그러기 위해선 원본 객체가 반드시 java.lang.Cloneable 인터페이스를 구현하고 있어야 한다.
만약 Cloneable 인터페이스를 명시적으로 구현하지 않으면 CloneNotSupportedException 예외가 발생한다. (그래서 clone() 메소드는 try-catch 구문이 필요하다.)

```java
public class Member implements Cloneable {
    public String id;
    public String name;
    public String password;
    public int age;
    public boolean adult;

    public Member(String id, String name, String password, int age, boolean adult) {
        this.id = id;
        this.name = name;
        this.password = password;
        this.age = age;
        this.adult = adult;
    }

    public Member getMember() {
        Member cloned = null;
        try {
            cloned = (Member) clone();
        } catch (CloneNotSupportedException e) {
            System.out.println("CloneNotSupportedException");
        }
        return cloned;
    }
}
```

```java
public class MemberExample {
    public static void main(String[] args) {
        // 원본 객체 생성
        Member original = new Member("blue", "길동", "12345", 25, true);
        // 복제 객체를 얻은 후 비밀번호 변경
        Member cloned = original.getMember();
        cloned.password = "67890";

        System.out.println("cloned.id: " + cloned.id); // blue
        System.out.println("cloned.password: " + cloned.password); // 12345

        System.out.println("original.id: " + original.id); // blue
        System.out.println("original.password: " + original.password); //67890
    }
}
```

2. 깊은 복제

얕은 복제는 참조 타입 필드를 번지만 복사하기 때문에 원본 객체의 필드와 복제 객체의 필드는 같은 객체를 참조한다. 그래서 복제 객체의 참조 객체를 바꾸면 원본 객체도 같이 바뀌는 단점이 있다.(번지를 참조했기 때문이다.)
반면 깊은 복제는 참조하고 있는 객체도 복제한다.
깊은 복제를 하려면 Object 의 clone() 메소드를 재정의해서 참조 객체를 복제하는 코드를 직접 작성해야 한다.

```java
public class Car {
    public String model;
    public Car(String model) {
        this.model = model;
    }
}
```

```java
public class Member implements Clonable {
    public String name;
    public int age;
    public int[] scores; // 참조 타입 필드
    public Car car; // 참조 타입 필드

    public Member(String name, int age, int[] scores, Car car) {
        this.name = name;
        this.age = age;
        this.scores = scores;
        this.car = car;
    }

    // clone() 메소드 재정의
    @Override
    protected Object clone() throws CloneNotSupportedException {
        // 먼저 얕은 복사로 name, age 를 복제한다.
        Member cloned = (Member) super.clone(); // Object 의 clone() 호출
        // scores 를 깊은 복제한다.
        cloned.scores = Arrays.copyOf(this.scores, this.scores.length);
        // car 를 깊은 복제한다.
        cloned.car = new Car(this.car.model);
        // 깊은 복제된 Member 객체를 리턴한다.
        return cloned;
    }
    public Member getMember() {
        Member cloned = null;
        try {
            cloned = (Member) clone();
        } catch (CloneNotSupportedException e) {
            e.printStackTrace();
        }
        return cloned;
    }
}
```

```java
public class MemberExample {
    public static void main(String[] args) {
        // 원본 객체 작성
        Member original = new Member("길동", 25, new int[] {90, 90}, new Car("소나타"));

        // 복제 객체를 얻은 후에 참조 객체의 값을 변경
        Member cloned = original.getMember();
        cloned.scores[0] = 100;
        cloned.car.model = "그랜저";

        System.out.println("cloned.name: " + cloned.name); // 길동
        System.out.println("cloned.scores: " + cloned.scores); // {100, 90}
        System.out.println("cloned.car.model: " + cloned.car.model); // 그랜저

        System.out.println("original.name: " + original.name); // 길동
        System.out.println("original.scores: " + original.scores); // {90, 90}
        System.out.println("original.car.model: " + original.car.model); // 소나타

    }
}
```

### 객체 소멸자(finalize())

참조하지 않는 배열이나 객체는 가비지 컬렉터가 힙 영역에서 자동으로 삭제하는데, 그 전에 객체의 소멸자(finalize())를 실행시킨다. 소멸자는 Object 의 finalize() 메소드로, 기본적으로 실행 내용이 없다.
만약 객체가 소며로디기 전에 마지막으로 사용한 자원(데이터 연결, 파일 등)을 닫고 싶거나, 중요한 데이터를 저장하고 싶다면 Object 의 finalize9) 메소드를 재정의하면 된다.

```java
public class Counter {
    private int no;
    public Counter(int no) {
        this.no = no;
    }
    @Override
    protected void finalize() throws Throwable {
        System.out.println(no + "번 객체의 finalize() 실행");
    }
}
```

```java
public class FinalizeExample {
    public static void main(String[] args) {
        Counter counter = null;
        for (int i = 1; i <= 50; i++) {
            counter = new Counter(i);
            // Counter 객체를 쓰레기로 만듦
            counter = null;
            // 가비니 컬렉터의 실행을 요청함.
            System.gc();
        }
    }
}
```

실행 결과를 보면 무작위로, 그리고 메모리의 상태를 보고 전체가 아닌 일부만 소멸시킨다. 가비지 컬렉터는 메모리가 부족할 때나 CPU 가 한가할 떄 JVM 에 의해 자동 실행된다. 그렇기 때문에 finalize() 메소드가 호출되는 시점은 명확하지 않다.
프로그램이 종료될 때 즉시 자원을 해제하거나 즉시 데이터를 최종 저장해야 한다면, 일반 메소드를 작성한 후 프로그램이 종료될 때 명시적으로 finalize() 메소드를 호출하는 것이 좋다.

## Objects 클래스

java.util.Objects 클래스는 객체 비교, 해시코드 생성, null 여부 확인, 객체 문자열 리턴 등의 연산을 수행하는 정적 메소드들로 구성된 유틸리티 클래스이다.

### 객체 비교 compare(T a, T b, Comparator<T> C)

`Objects.compare(T a, T b, Comparator<T> C)` 메소드는 두 객체 a, b 를 비교자(Comparator)로 비교해서 int 값을 리턴한다. T 는 제네릭으로 비교할 객체 타입을 나타낸다. a 가 b 보다 작으면 음수, 크면 양수를 리턴하도록 구현 클래스를 만들어야 한다.

```java
class StudentComparator implements Comparator<Student> {
    @Override
    public int compare(Student a, Student b) {
        if (a.sno < b.sno) return -1;
        else if (a.sno == b.sno) return 0;
        else return 1;

        // 위 코드를 한 줄로 대체할 수 있다.
        // return Integer.compare(a.sno, b.sno);
    }
}
```

```java
public class CompareExample {
    public static void main(String[] args) {
        Student s1 = new Student(1);
        Student s2 = new Student(1);
        Student s3 = new Student(2);

        int result = Object.comapre(s1, s2, new StudentComparator());
        result = Object.compare(s1, s3, new StudentComparator());
    }

    static class Student {
        int sno;
        Student(int sno) {
            this.sno = sno;
        }
    }
    static class StudentComparator implements Comparator<Student> {
        @Override
        public int compare(Student o1, Student o2) {
            return Integer.compare(o1.sno, o2.sno);
        }
    }
}
```

### 동등 비교 equals() , deepEquals()

`Objects.equals(Object a, Object b)` 메소드는 두 객체의 동등을 비교하는데 다음과 같은 결과를 리턴한다.

- not null, not null => a.equals(b) 의 리턴값
- null, not null => false
- not null, null => false
- null, null => true

`Objects.deepEquals(Object a, Object b)` 메소드도 두 객체의 동등을 비교하는데, a 와 b 가 서로 다른 배열이거나 항목 값이 모두 같으면 true 를 리턴한다.(`Arrays.deepEquals(Object[] a, Object[] b` 메소드와 동일하다.)

- not null (not array), not null (not array) => a.equals(b) 의 리턴값
- not null (array), not null (array) => Arrays.deepEquals(a, b) 의 리턴값
- not null, null => false
- null, not null => false
- null, null => true

```java
public class EqualsAndDeepEqualsExample {
    public static void main(String[] args) {
        Integer o1 = 1000;
        Integer o2 = 1000;
        System.out.println(Objects.equals(o1, o2)); // true
        System.out.println(Objects.equals(o1, null)); // false
        System.out.println(Objects.equals(null, o2)); // false
        System.out.println(Objects.equals(null, null)); // true
        System.out.println(Objects.deepEquals(o1, o2) + "\n"); // true

        Integer[] arr1 = {1, 2};
        Integer[] arr2 = {1, 2};
        System.out.println(Objects.equals(arr1, arr2)); // false
        System.out.println(Objects.deepEquals(arr1, arr2)); // true
        System.out.println(Arrays.deepEquals(arr1, arr2)); // true
        System.out.println(Objects.deepEquals(null, arr2)); // false
        System.out.println(Objects.deepEquals(arr1, null)); // false
        System.out.println(Objects.deepEquals(null, null)); // true
    }
}
```

### 해시코드 생성 hash(), hashCode()

`Objects.hash(Object... values)` 메소드는 매개값으로 배열을 생성한 후 `Arrays.hashCode(Object[])`메소드를 호출해서 해시코드를 얻고 이 값을 리턴한다.
이 메소드는 클래스가 hashCode() 메소드를 재정의해서 리턴값을 생성하기 위해 사용하면 좋다.
클래스가 여러 가지 필드를 가지고 있을 때 이 필드를로부터 해시코드를 생성하면 동일한 필드값을 가진 객체는 동일한 해시코드를 가질 수 있다.

`Objects.hash(Object o)` 메소드는 매개값으로 주어진 객체의 해시코드를 리턴하는데, o.hashCode() 의 리턴값과 동일하다. 다만 매개값이 null 이면 0을 리턴한다.

```java
public class HashCodeExample {
    public static void main(String[] args) {
        Student s1 = new Student(1, "aa");
        Student s2 = new Student(1, "aa");
        System.out.println(s1.hashCode()); // 54151054
        System.out.println(Objects.hashCode(s2)); // 54151054
    }

    static class Student {
        int sno;
        String name;
        Student(int sno, String name) {
            this.sno = sno;
            this.name = name;
        }

        @Override
        public int hashCode() {
            return Objects.hash(sno, name);
        }
    }
}
```

### 널 여부 조사

`Objects.isNull(Object obj)`는 매개값이 null 이면 true 를 리턴한다. `Objects.nonNull(Object obj)`는 매개값이 not null 이면 true 를 리턴한다. `Objects.requireNonNull()`은 다음 세 가지로 오버로딩되어 있다.

- `requireNonNull(T obj)` : not null => obj , null => NullPointerException
- `requireNonNull(T obj, String message)` : not null => obj , null => NullPointerException(message)
- `requireNonNull(T obj, Supplier<String> msgSupplier)` : not null => obj , null => NullPointerException(msgSupplier.get())

```java
public class NullExample {
    public static void main(String[] args) {
        String str1 = "name";
        String str2 = null;

        System.out.println(Objects.requireNonNull(str1)); // name

        try {
            String name = Objects.requireNonNull(str2);
        } catch (Exception e) {
            System.out.println(e.getMessage()); // null
        }

        try {
            String name = Objects.requireNonNull(str2, "no name");
        } catch (Exception e) {
            System.out.println(e.getMessage()); // no name
        }

        try {
            String name = Objects.requireNonNull(str2, () -> "ramda: no name");
        } catch (Exception e) {
            System.out.println(e.getMessage()); // ramda: no name
        }
    }
}
```

### 객체 문자 정보

`Objects.toString()`은 객체의 문자 정보를 리턴하는데, 다음 두 가지로 오버로딩되어 있다.

- `toString(Object o)` : not null => o.toString() , null => "null"
- `toString(Object o, String nullDefault)` : not null => o.toString() , null => nullDefault

```java
public class ToStringExample {
    public static void main(String[] args) {
        String str1 = "name";
        String str2 = null;

        System.out.println(Objects.toString(str1)); // name
        System.out.println(Objects.toString(str2)); // null
        System.out.println(Objects.toString(str2, "no name")); // no name
    }
}
```

## System 클래스

### 프로그램 종료 (exit())

System 클래스의 exit() 는 현재 실행하고 있는 프로세스를 강제로 종료한다. 정상 종료일 경우 매개값을 0으로, 비정상 종료일 때는 0 이외의 값을 준다. 특정 값으로 종료하려면 자바의 보안 관리자를 직접 설정해서 종료 상태값을 확인해야 한다.
System.exit() 를 실행하면 checkExit() 메소드가 자동 호출되는데, 이 메소드에서 종료 상태값을 조사해서 특정 값이 입력되지 않으면 SecurityException 을 발생시켜 System.exit() 를 호출한 곳에서 예외 처리를 하도록 해준다.

```java
public class ExitExample {
    public static void main(String[] args) {
        System.setSecurityManager(new SecurityManager() {
            @Override
            public void checkExit(int status) {
                if (status != 5) { throw new SecurityException(); }
            }
        });

        for (int i = 0; i < 10; i++) {
            System.out.println(i);
            try {
                // JVM 종료 요청
                System.exit(i);
            } catch (SecurityException e) {}
        }
    }
}
```

### 가비지 컬렉터 실행 (gc())

JVM 은 메모리가 부족할 때 또는 CPU 가 한가할 때 가비지 컬렉터를 실행시켜 사용하지 않는 객체를 자동으로 제거한다. System.gc(); 메소드를 실행하면 빠른 시간 내로 실행하도록 요청을 할 수 있다.(곧바로 가비지 컬렉터를 띄우진 않는다.)

```java
public class GcExample {
    public static void main(String[] args) {
        Employee emp;

        emp = new Employee(1);
        emp = null;
        emp = new Emplyee(2);
        emp = new Employee(3);
        // new Employee(1), new Emplyee(2) 은 쓰레기가 됐다.
        System.out.print("emp 가 최종적으로 참조하는 번호: " + emp.eno); // 3
        System.gc(); // 가비지 컬렉터의 실행을 요청함
    }
}

class Employee {
    public int eno;
    public Employee(int eno) {
        this.eno = eno;
        System.out.println(eno + "가 메모리에 생성됨");
    }
    public void finalize() {
        System.out.println(eno + "가 메모리에서 제거됨");
    }
}
```

### 현재 시각 읽기 (currentTimeMilis(), nanoTime())

currentTimeMillis(), nanoTime() 메소드는 컴퓨터의 시계로부터 현재 시간을 읽어서 밀리세컨드 단위와 나노세컨드 단위의 long 값을 리턴한다. 주로 프로그램의 실행 소요 시간을 측정하는 데 사용한다.

### 시스템 프로퍼티 읽기 (getProperty())

시스템 프로퍼티는 JVM 이 시작할 때 자동으로 설정되는 시스템의 속성값이다. 시스템 프로퍼티는 키와 값으로 구성되어 있다. System.getProperty("키 이름") 으로 불러올 수 있다. 아래는 키와 그에 따른 설명을 나열한 것이다.

- java.version : 자바의 버전
- java.home : 사용하는 JRE 의 파일 경로
- os.name : Operating System name
- file.separator : File separator ("/" on UNIX)
- user.name : 사용자의 이름
- user.home : 사용자의 홈 디렉토리
- user.dir : 사용자가 현재 작업 중인 디렉토리 경로

### 환경 변수 읽기 (getenv())

환경 변수는 운영체제에서 이름(Name) 과 값(Value)으로 관리되는 문자열 정보로, 자바에서는 System.getenv() 메소드로 환경 변수의 값을 가져온다. 매개값으로 환경 변수의 이름을 입력한다.

## Class 클래스

### Class 객체 얻기 (getClass(), forName())

프로그램에서 Class 객체를 얻으려면 Object 클래스의 getClass() 메소드를 이용하면 된다. Object 는 최상위 클래스이므로 모든 클래스에서 obj.getClass() 으로 메소드를 호출할 수 있다. 다만 이 메소드는 해당 클래스로 객체를 생성했을 때만 사용할 수 있다.
객체를 생성하지 않고 정적 메소드 forName() 으로 직접 Class 객체를 얻을 수 있다. 클래스 전체 이름(패키지가 포함된 이름)을 매개값으로 Class 객체를 리턴한다.

```java
public class ClassExample {
    public static void main(String[] args) {
        Car car = new Car();
        Class clazz1 = car.getClass();
        System.out.println(class1.getName()); // sec06.exam01_class.Car
        System.out.println(class1.getSimpleName()); // Car
        System.out.println(class1.getPackage().getName()); // sec06.exam01_class

        try {
            Class clazz2 = Class.forName("sec06.exam01_class.Car");
            System.out.println(clazz2.getName()); // sec06.exam01_class.Car
            System.out.println(clazz2.getSimpleName()); // Car
            System.out.println(clazz2.getPackage().getName()); // sec06.exam01_class
        } catch (ClassNotFoundException e) { e.printStackTrace(); }
    }
}
```

### 리플렉션 (getDeclaredContructors(), getDeclaredFields(), getDeclaredMethods())

Class 객체를 이용하면 클래스의 생성자, 필드, 메소드의 정보를 각각 getDeclaredContructors(), getDeclaredFields(), getDeclaredMethods() 메소드를 이용해 알아낼 수 있다.
이 세 메소드는 각각 Constructor 배열, Field 배열, Method 배열을 리턴한다.
getDeclaredFields(), getDeclaredMethods() 는 클래스에 선언된 멤버만 가져오고 상속된 멤버는 가져오지 않는다. 만약 상속된 멤버도 얻으려면 getFields(), getMethods() 를 이용해야 한다. (단 public 멤버만 가져온다.)

```java
Class clazz = Class.forName("sec06.exam01_class.Car");

Field[] fields = clazz.getDeclaredFields();
for (Field field : fields) {
    // 필드 타입, 필드 이름
    System.out.println(field.getType().getSimpleName() + ", " + field.getName());
}

// 메소드 이름과 매개변수를 출력
Method[] methods = clazz.getDeclaredMethods();
for (Method method : methods) {
    System.out.println(method.getName());
    Class[] parameters = method.getParameterTypes();
    printParameters(parameters); // 매개변수 정보를 출력
}


private static void printParameters(Class[] parameters) {
    for (int i = 0; i < parameters.length; i++) {
        System.out.print(parameters[i].getName());
    }
}
```

### 동적 객체 생성 (newInstance())

Class 객체로 new 연산자가 없어도 동적으로 객체를 생성할 수 있다. 이 방법은 코드 작성 시가 아닌, 런타임 시에 클래스 이름을 결정하는 경우에 유용하다.
Class.forName() 메소드로 Class 객체를 얻은 후 newInstance() 메소드를 호출하면 Object 타입의 객체를 얻을 수 있다. 이 객체는 기본 생성자를 호출하며, 만약 매개변수가 있는 생성자를 호출하려면 리플렉션으로 생성자 객체를 얻어 newInstance() 메소드를 호출하면 된다.
newInstance() 은 InstantiationException 예외(해당 클래스가 추상 클래스이거나 인터페이스일 경우), IllegalAccessException 예외(클래스나 생성자가 접근 제한자 떄문에 접근할 수 없을 때)가 있기 떄문에 예외 처리 코드가 필요하다.
