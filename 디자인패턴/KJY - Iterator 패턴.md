<aside>
💡 무엇인가 많이 모여있을 때 이를 순서대로 가리키며 전체를 검색하고 처리를 반복하는 패턴

</aside>

- for문의 변수 i를 추상화한 것

- 무엇인가 많이 모여있는 것 중에서 하나씩 끄집어내어 열거하면서 전체를 처리하는 일을 하는 경우에 적용

## UML

### UML

![Iterator.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0d4772d3-7a81-48be-b9d3-888f10601699/Iterator.png)

### 시퀀스 다이어그램

## 역할

### Iterator(반복자)

- 원소를 하나씩 끄집어낼 때 사용할 공통된 메소드를 선언한 인터페이스
- hasNext()와 next() 메소드 선언
- Iterator Interface

### ConcreteIterator Class(구체적인 반복자)

- Iterator 인터페이스를 실제로 구현하는 클래스
- BookShlefIterator

### Aggregate Interface(집합체)

- Iterator를 만들어내는 인터페이스를 제공
- iterator() : 내가 가지고 있는 각 원소들을 차례로 검색해 줄 사람을 만들어내는 메소드
- BookShelf() : 내가 가지고 있는 각 원소

### ConcreteAggregate Class(구체적인 집합체)

- Aggregate 인터페이슬 구현하는 클래스
- 

## 심층 이해

### 왜 Iterator 패턴이 유용한가?

- 원소를 끄집어내는 방법이 집합체 구현과 무관
    - 집합체가 원소를 어떻게 유지하고 있든지 상관없이, 집합체의 원소를 차례로 끄집어내고자 하면, iterator의 hasNext()와 next() 메소드를 사용하면 됨
- 하나를 수정했을 때, 그것으로 인해 수정해할 부분이 적음
- 가능한 한 추상 클래스나 인터페이스를 자주 사용하여 구체적인 클래스를 줄임
- Aggregate와 Iterator의 대응 관계
    - ex) BookShelf()의 getBookFrom()의 이름을 getBookAt()으로 바꾸면, BookShelfIterator의 next() 내부도 수정해야 함
- 여러 종류의 iterator 생성 가능
    - ex) 역방향

## 예제(책-책장)

### Iterable<E> Interface

- 집합체를 나타내는 인터페이스
- Iterator() : 집합체에 대응하는 iterator 한 개를 생성하는데 사용될 메소드
    - 어떤 집합체의 원소를 하나씩 열거하거나 조사하고자 할 때 이 메소드를 사용해서 Iterator 인터페이스를 구현한 클래스의 인스턴스를 한 개 얻어옴

```java
public interface Iterable<E> {
	public abstract Iterator<E> iterator();
}
```

### Iterator<E> Interface

- 하나씩 열거하면서 스캔을 실행하는 인터페이스
- 집합체의 원소를 하나한 끄집어내는 루프 변수와 같은 역할
- hasNext() : 다음 원소가 존재하는지 조사할 때 사용하는 메소드
- next() : 다음 원소를 얻어올 때 사용하는 메소드

```java
public interface Iterator<E> {
	public abstract abstract hasNext();
	public abstract E next();
}
```

### Book Class

- 책을 나타내는 클래스
- name : 책이름
- getName() : 책의 이름을 얻어올 때 호출하는 메소드

```java
public class Book{
	private String name;

	public Book(String name){
		this.name = name;
	}

	public String getName(){
		return name;
	}
}
```

### BookShelf Class

- 책장을 나타내는 클래스 = 집합체
- Aggregate 인터페이스를 구현
    - iterator() + 다른메소드()
- books : book의 배열
    - 생성자(BookShelf() 호출 시 크기 지정됨)
    - private
- appendBook() : 책 한 권 추가 메소드
- getLength() : 책장의 책의 개수를 반환하는 메소드
- iterator() : 책장의 책 하나하나를 끄집어내는 일을 하는 BookShelfIterator를 생성하는 메소드

```java
import java.util.Iterator;

public calss BookShlef implements Iterable<Book>{
	private Book[] books;
	private int last = 0;

	public BookShelf(int maxsize){
		this.books = new Book[maxsize];
	}

	public Book getBookAt(int index){
		return books[index];
	}

	public void appendBook(Book book){
		this.books[last] = book;
		last++;
	}

	public int getLength(){
		return last;
	}

	@Override
	public Iterator<Book> iterator(){
		return new BookShelfIterator(this);
	}
}
```

### BookShelfIterator

- 책장에 있는 책들을 하나씩 끄집어내는 일을 하는 클래스
- Iterator 인터페이스를 구현
- BookShelf : BookShelfIterator가 검색할 책장을 가리키는 변수
- index : 책장에서의 현재 책을 가리키는 변수
- hasNext() : 다음 책이 있으면 true, 없으면 false를 반환
- next() : 현재 가리키고 있는 책을 반환하고, 다음 책을 가리키는 메소드

```java
import java.util.Iterator;
improt java.util.NoSuchElementException;

public class BookShelfIterator implements Iterator<Book>{
	private BookShelf bookshlef;
	private int index;

	public BookShlefIterator(BookShelf bookshelf){
		this.bookshelf = bookshelf;
		this.index = 0;
	}

	@Override
	public boolean hasNext(){
		if(index < bookshelf.getLength()){
			return true;
		}else{
			return false;
		}
	}

	@Override
	public Book next(){
		if(!hasNext()){
			throw new NoSuchElementException();
		}
		Book book = bookShelf.getBookAt(index);
		index++;
		return book;
	}
}
```

### Main

- 
- 

```java
public class Main{
	public static void main(String[] args){
		BookShelf bookShelf = new BookShelf(4);
		bookShelf.appendBook(new Book("Bible));

		//명시적으로 Iterator를 사용하는 방법
		Iterator<Book> it = bookShelf.iterator();
		while(it.hasNext()){
			Book book = it.next();
			System.out.println(book.getName());
		}
		System.out.println();

		//확장 for문을 사용하는 방법
		for(Book book : bookShelf){
			System.out.println(book.getName());
		}
		System.out.println();
	}
}
```
