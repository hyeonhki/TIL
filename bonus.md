#### - 주간 추가 공부
- [ ] C언어의 특징 : hyulim님 말씀. 메모리 접근 정리! 슬랙에 저장!
- [x] %p 구현과정에 있어서의 궁금증
  - void *을 unsigned int 로 받는 것
  - void *? 자료형에 대한 근본적 지식
- [x] NULL과 0의 차이
- [ ] 재귀함수
- [ ] ft_bewhat, be_zero에서 void 포인터를 사용한 함수구현ㅍ
  - 왜 이차원 포인터를 사용하지 않았는데 내용이 바뀌었나?
  - 인덱스를 사용하지 않고 함수를 구현할 시 문제가 생긴 내용
- [ ] char * 와 char []의 차이
  > https://skyul.tistory.com/28
- [x] unsigned long, unsigned int 자료형 byte와 이로인해 생기는 문제
- [x] 다중 free 만들기
- [x] fsanitize 메모리 누수 잡기
- [ ] heap BOF
- [x] 문자열의 널, printf, malloc
- [x] 댕글링 포인터
- [ ] write와 printf 교차사용 : stdout
___
### - 상세 학습 내용
- %uxX 는 printf 출력대상 자료형에 unsigned int라고 명시되어 있음.
  - int와 long의 경우, 표현가능한 수의 범위와 길이는 같지만 운영체제에 따라 비트의 차이가 있다.

  |운영체제|16비트|32비트|64비트|
  |:-:|-|-|:-:|
  |int|16bits|32bits|32bits(4bytes)|
  |long|32bit|32bits|64bits(8bytes)|
  - UNIX 운영 체제에서 int와 long의 메모리크기가 4,8 바이트로 다름.
  - void *의 크기는 8 바이트로 unsinged int를 사용하게 되면 길이가 잘릴 수 있다.


- %p 구현과정 중 unsigned long을 사용하는 이유
  - unsigned int 와 다를게 없어 보이는 데 왜일까?
  - ptf 테스트 과정 중 unsigned int 사용시 오류 발생

- void *
  - 자료형이 정해지지 않은 범용 포인터로 어떤 자료형이든 저장 가능
  - 주소만 저장할 수 있고, 값에 접근하거나 변경할 수 없다.
  - 자료형이 정해지지 않았기 때문에 역참조는 불가능하고 형변환이 필요하다.

- NULL과 0의 차이
  - NULL
    - NULL은 헤더파일에 정의된 매크로 'null pointer constant'로 컴파일에 의해 (void *)0으로 정의된다.
    - 널은 포인터 변수 초기화 시에 사용하며, 0 주소를 의미한다.
    - char *ptr = NULL 과 char *ptr = 0 은 같은 의미이다. 하지만, int n = NULL과 int n = 0의 의미는 다르다. 주소값 0과 정수 0은 다르기 때문
    - printf를 통해 사이즈를 출력해보면 메모리의 크기도 다르게 나온다. void *는 8byte, 0은 4byte
  ```c
  #include <stdio.h>
  int main()
  {
   printf("NULL : %d, 0 : %d", sizeof(NULL), sizoef(0)); // NULL : 8, 0 : 4
   return (0):
  }
  ```
    - 무분별한 혼용으로 일어날 수 있는 문제?
      - 주소값을 반환값으로 받는 상황에서 엄밀히 따지면 0과 큰 차이가 있다고 한다.

- 빈 문자열과 널의 차이
  - (1) string a = null;
    - 객체가 힙에서 아무것도 참조하지 않음
  - (2) string a = "";
    - 객체가 힙에서 길이가 0인 고유한 문자열을 나타내는 것

- 댕글링 포인터
  - premanture free(조숙한 해제, 너무 급한 해제)
  - 포인터가 여전히 해제된 메모리 영역을 가리키고 있는 것을 뜻한다.
  - free 이후에 null 포인터 지정을 통해 해결할 수 있다.
  - 동적할당된 buf가 존재할 경우
    - free(buf) => 할당받은 메모리를 해제하는 것
    - buf = 0 => 할당받은 메모리주소를 담고있는 buf 변수가 0값을 갖게되는 것으로 <span style="color:red">할당받은 메모리주소를 잃어버린 것이 되어 해제가 불가능해진다 = __MEMORY LEAK__</span>
    ```c
    int main()
    {
      char *a;

      a = strdup("abcdef");
  	  printf("a : %s\n", a); // a : abcdef
  	  free(a);
  	  printf("a : %s\n", a); // a : abcdef
  	  a = 0;
  	  printf("a : %s\n", a); // a : 0
  	  return (0);
    }
    ```

### - 참조
> https://dojang.io/mod/page/view.php?id=278(void *)
> https://noirstar.tistory.com/16 (NULL과 0의 차이)
> 슬랙 : dlee {NULL은 (void *)0 으로 정의되어 있는 것으로 아는데 일상적인 사용에서는 0과 혼용이 가능하지만 주소값을 반환값으로 받는 상황에서 물리넷처럼 엄밀히 따진다면 0 과 큰 차이가 있으니까요.} (NULL과 0의 차이)