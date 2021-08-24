# 시작하기 전에

**Collection을 순회할때 변화가 생기면 `ConcurrentModificationException` 이 발생한다.**

<br>

# ❗문제 발견

가위 바위 보 프로그램을 테스트를 하던 중

돈을 모두 잃었을 때는 `NullPointerException` 이 뜨고

이겼을때는`ConcurrentModificationException` 이 뜨는 상황을 만나게 되었다.

<br>

## ❓문제의 원인

**`ConcurrentModificationException`** 은 난생 처음 보는 익셉션이었기 때문에 일단 무시하고 `NullPointerException` 을 해결하려고 시도했다

`NullPointerException` 이 떴다는 것은 null 인 객체의 필드나 메소드에 접근을 시도했다는 뜻이므로

Controller 클래스 안에 있는 delete 메소드에서 시작된 익셉션 이라는 것을 파악했다.

```java
// delete 메소드
// for each 문을 사용해서 리스트를 순회하면서 조건에 맞는 객체를 삭제한다
private void delete() {
	for(RSPDTO user : list) {
		if(user.getMoney() <= 0) {
			list.remove(user);
		}
	}
}
```

<br>

# 🔨어떻게 해결했나?

## ❌첫번째 방법

**delete 메소드 호출 위치를 이곳 저곳 옮겨보기**

어느 곳으로 옮겨보든 익셉션은 똑같았고 오히려 코드만 더 난잡해졌으므로 포기

<br>

## ❌두번째 방법

**delete 메소드에서 나 자신은 지우지 않기**

```java
private void delete() {
	for(RSPDTO user : list) {
		if(user.getId() == CURRENT_USER_ID) {
			if(user.getMoney() <= 0) {
				list.remove(user);
			}
		}
	}
}
```

`NullPointerException` 은 더 이상 뜨지 않았지만 `ConcurrentModificationException` 은 계속 발생하였으므로 근본적인 해결 방법은 아니었다

<br>

## ⭕세번째 방법

**코드 구조 바꾸기**

첫번째 방법도 두번째 방법도 모두 실패로 끝났기 때문에 결국 코드 구조를 바꾸기로 결정했다.

```java
public void delete(RSPDTO player) {
    if(player.getMoney() <= 0) {
        list.remove(player);
    }
}
```

for 문을 사용해서 날로 먹으려고 했던 고약한 심보 때문이었을까

for 문을 버리자 마자 익셉션이 발생하는 일 없이 프로그램을 무사히 완성하게 되었다.

<br>

# 📚그래서 그.. Concurrent... 머시기가 뭔데??

간단하게 말하자면

`ConcurrentModificationException` 은 for each문(Enhanced for loop)을 사용할 때 컬렉션 프레임워크(`List`, `Set`, `Map` 등)에 변화가 일어날때 발생하는 익셉션이다.

`for each`문을 사용하는 경우 반복문을 수행하는 동안 반복 대상이 되는 컬렉션의 변경을 허용하지 않는다.

그러므로 컬렉션을 변경할때 사용할 수 있는 몇가지 방법을 알아보자

## 1. 인덱스 기반 for 문으로 해결

```java
// list 생성
ArrayList<RSPDTO> list = new ArrayList<>();
for(int i = 0; i < 5; i++) {
    RSPDTO temp = new RSPDTO();
    temp.setId(i);
    list.add(temp);
}

// 지우기
for (int i = 0; i < list.size(); i++) {
    RSPDTO user = list.get(i);
    if (user.getId() >= 2) {
        list.remove(i); // 또는 list.remove(user);
    }
}

// 리스트 내용물 확인
System.out.print("[");
for(RSPDTO user : list) {
    System.out.print(" " + user.getId() + " ");
}
System.out.print("]\n");

// 결과
// [ 0  1  3 ]
```

하지만 이 방법은 예상하지 못한 결과를 불러오기도 하는데

리스트는 **i 번째 원소**가 삭제되면 **i+1 번째 원소**가 그 자리에 오게되고, 리스트의 길이가 1만큼 짦아지기 때문이다.

## 2. 이터레이터로 해결

[이터레이터(Iterator)란?](https://thefif19wlsvy.tistory.com/41)

[Iterator<E> 인터페이스](http://tcpschool.com/java/java_collectionFramework_iterator)

자바의 `Iterator` 인터페이스는 `remove` 메소드를 제공하고 있는데, 자바 공식 문서에서는 `remove` 메소드를 사용하는 것이 컬렉션을 순회하면서 원소를 삭제할 수 있는 유일하게 안전한 방법이라고 가이드 하고 있다고 한다.

```java
// iterator() = iterator 인터페이스를 구현한 클래스의 인스턴스를 반환하는 메소드
// hasNext() : 읽어올 요소가 남아있는지 확인하는 메소드. 요소가 있으면 true, 없으면 false
// next() : 다음 데이터를 반환.
// remove() : next()로 읽어온 요소를 삭제

// list 생성
ArrayList<RSPDTO> list = new ArrayList<>();
for(int i = 0; i < 5; i++) {
    RSPDTO temp = new RSPDTO();
    temp.setId(i);
    list.add(temp);
}

// 지우기
for (Iterator<RSPDTO> iter = list.iterator(); iter.hasNext(); ) {
    RSPDTO user = iter.next();
    if (user.getId() >= 2) {
        iter.remove();
    }
}

// 리스트 내용물 확인
System.out.print("[");
for(RSPDTO user : list) {
    System.out.print(" " + user.getId() + " ");
}
System.out.print("]\n");

// 결과
// [ 0  1 ]
```

## 3. 자바 8에서 추가된 방법

`Collection` 인터페이스에 `removeIf` 메소드가 추가되었으므로 다음과 같은 한 줄의 코드만 적어주면 깔끔하게 변경할 수 있다

[removeIf 메소드 사용예제](https://codechacha.com/ko/java-collections-arraylist-removeif/)

```java
// list 생성
ArrayList<RSPDTO> list = new ArrayList<>();
for(int i = 0; i < 5; i++) {
    RSPDTO temp = new RSPDTO();
    temp.setId(i);
    list.add(temp);
}

// 지우기 (list안의 객체들을 순회하면서 RSPDTO 객체의 id 값을 비교해서 객체를 제거한다.)
list.removeIf(user -> user.getId() >= 2);

// 리스트 내용물 확인
System.out.print("[");
for(RSPDTO user : list) {
    System.out.print(" " + user.getId() + " ");
}
System.out.print("]\n");

// 결과
// [ 0  1 ]
```

# 결론

이번 예외 상황을 겪으며 다시 한번 설계의 중요성과 `for each`문을 사용하느냐 아니냐는 단순한 취향 차이의 문제가 아니라는 것을 알게 되었다

그리고 컬렉션을 순회하며 변경할 일이 생긴다면 특이사항이 없는 이상

컬렉션을 변경하는 안전한 방법인 **이터레이터**를 사용하도록 하자

# ➕추가적으로 만난 예외 상황들

## 성적표 프로그램

- **`InputMismatchException`**
  - 지정한 변수 타입과 다른 타입을 입력받았을 때 발생하는 예외
- **`NumberFormatException`**
  - 숫자 외의 형태를 숫자로 변환하려고 할때 발생하는 예외

---

**ConcurrentModificationException 참고한 글**

[List 순회 중 만난 ConcurrentModificationException과 컬렉션 불변성](https://m.blog.naver.com/tmondev/220393974518)

[[자바] 컬렉션에서 원소 삭제하기 (ConcurrentModificationException 피하면서)](https://www.daleseo.com/how-to-remove-from-list-in-java/)

**예외처리에 대한 기본적인 예제 참고한 글**

[예외처리](https://ande226.tistory.com/24)
