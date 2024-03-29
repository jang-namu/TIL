# 자료구조
* 이하 설명은 모두 c언어를 통해 작성 및 구현되었다.
* 자료구조의 모든 연산은 함수 내부에서 구조체를 변경하기 위해 구조체 포인터를 받는다.

## 2. 순환, Recursion
* 순환이란, 자기 자신을 호출하여 문제를 해결하는 프로그램.
* 순환 호출의 내부적인 구현
    1. 함수가 자기를 다시 호출하는 것은 다른 함수를 호출하는 것과 동일하다.
    2. 즉, 복귀주소가 시스템 스택에 저장되고 호출되는 함수를 위한 매개변수와 지연변수를 스택으로부터 할당받는다.
    3. 위와 같은 함수를 위한 시스템 스택에서의 공간을 활성레코드(activation record)라고 한다.
    4. 준비가 끝나면 호출된 함수의 시작 위치로 점프하여 수행한다.
    5. 호출된 함수가 끝나면 시스템 스택에서 복귀주소를 추출하여 호출한 함수로 되돌아간다.
    * 함수호출마다 새로운 지역변수를 만들지 못하면 이전 호출과 구분할 수 없으므로 순환 호출이 불가하다.
* 순환 알고리즘의 구조
    * 순환 알고리즘은 자기자신을 순환호출하는 부분과 순환호출을 멈추는 부분으로 구성된다.
    * 분할정복방법(divide and conquer) : 문제의 일부를 해결할 다음, 나머지 문제에 대해 순화호출한다. 
    * 따라서 순환호출이 일어날 때마다 문제의 크기가 작아진다.
* 반복(iteration)과 순환의 관계
    * 기본적으로 반복과 순환은 문제해결능력이 같다.
    * 순환호출이 끝에서 일어나는 꼬리순환(tail recursion)의 경우, 반복으로 쉽게 바꾸어 쓸 수 있다.
    * 순환의 경우 몇몇 알고리즘에서 명확하고 간결하게 나타낼 수 있다는 장점이 있다.
    * 순환은 함수호출로 인한 오버헤드로 일반적인 경우 반복을 사용하는 것이 빠르다.
* 대표적인 순환 알고리즘
    * 순환이 반복보다 빠른 대표적인 예로, 거듭제곱값 계산이 있다. 순환은 O(log(n)), 반복은 O(n)으로 순환이 더 빠르다.
    * 피보나치의 경우 순환을 사용하면 자연스러운 알고리즘 작성이 가능하지만, 속도가 매우 느려지는 대표적인 예이다.

- - -
    * 반복적인 형태로 바꾸기 어려운 순환
     1. return n* factorial(n-1);
     2. return factorial(n-1) *n;
     * 1과 같이 순환 호출이 순환 함수의 맨 끝에서 이루어지는 것을 꼬리 순환이라고 한다. 이는 쉽게 반복으로 변환이 가능하다.
     * 2와 같은 머리순환의 경우나 하노이탑 문제처럼 여러 군데에서 자기 자신을 호출하는 경우는 쉽게 반복으로 바꿀 수 없다.
     * 따라서 동일한 알고리즘을 머리순환과 꼬리순환 모두로 작성할 수 있다면 당연히 꼬리 순환으로 작성해야 한다.
- - -
* 책에 설명에 오류가있어, 위 내용을 수정한다.
* [What is tail recursion?](https://stackoverflow.com/questions/33923/what-is-tail-recursion?newreg=9204514db4b647e2a3f00c669ebd6e2d)
* Head recursion은 위에 1, 2의 경우를 모두 포함한다. 즉, Head recursion은 순환호출이 일어나면서 시스템 스택에 함수가 종료되지 않고 쌓인 후, 하나 씩 해결되며 종료된다.
* Tail recursion의 경우, 함수가 순환호출을 할 때 자기가 맡은 일을 끝내고 다음 함수를 호출한다. return에 더 이상의 계산식이 없이 함수호출 부분만이 존재한다.

    * Head Recursion
    ```c
    function recsum(x) {
        if (x === 0) {
            return 0;
        } else {
         return x + recsum(x - 1);
        }
    }
    ```

    * Tail Recursion
    ```c
    function tailrecsum(x, running_total = 0) {
     if (x === 0) {
         return running_total;
     } else {
         return tailrecsum(x - 1, running_total + x);
     }
    }
    ```

## 3. 배열, 구조체, 포인터
* 배열(array)
    * 동일한 타입에 데이터를 저장
    * 많은 자료구조들이 배열을 사용하여 구현된다.
* 배열 ADT(Abstract Data Type)
    * 생각해보면 배열은 <인덱스, 값> 쌍의 집합이라고 볼 수 있다.
    * 배열은 주어진 인덱스에 값을 저장하는 set, 값을 추출하는 get 연산 등을 수행한다.
    * 컴파일러는 배열에 메모리를 연속된 위치에 할당한다. 인덱스 0이 기본주소(base)가 되고 다음 주소를 구하기 위해선 1씩 더한다.(base+1*sizeof(int))
* 2차원 배열
    * c언어에서 2차원 배열은 'int list[3][5];'의 형식으로 선언된다.
    * 즉, 배열에 배열을 만들어서 2차원 배열을 구현한다. 위의 예시에선 크기가 3인 배열을 만들고 이 배열의 요소로 크기가 5인 배열을 생성하여 추가한다.

* 구조체(structure)
* 타입이 다른 데이터를 묶는 방법, 일반적으로 struct 키워드 이용
    ```c
    struct 구조체태그 {
         항목1; 
         항목2;
    }; 
    ```   
* typedef 구조체 정의
    * 아래 예에서는 student가 새로운 데이터 타입의 이름이 된다.
        ```c
        typedef struct studentTag {
             char name[10];
             int age;
        } studnet;
        ```
    * 구조체는 중괄호를 사용하여 선언 시에 초기화 할 수 있다.
        1. student s = { "Jang", 23 };

* 배열의 응용 : 다항식
* 배열을 이용해서 수학에서 나오는 다항식을 나타내는 두가지의 자료구조를 생각할 수 있다.
    * 첫번째 방법 : 모든 항의 계수를 배열에 저장하고 최고차항을 degree 변수에 저장
        * 다항식 구조체는 'int degree'와 'float coef[MAX_DEGREE+1]'을 필드로 갖는다.
        * 간단하지만, 대부분의 항의 계수가 0인 희소 다항식의 경우 공간 낭비가 심하다.
        * 덧셈이나 뺄셈 시 같은 차수의 계수를 쉽게 찾을 수 있다.
    * 두번째 방법 : 0이 아닌 항만을 하나의 구조체 배열에 저장한다. 즉, 각 항을 (계수, 차수)의 구조체 형식으로 만들고 배열에 넣는다.
        * 'float coef'와 'int expon' 필드를 갖는 구조체의 배열이다.
        * 많은 다항식을 저장할 수 있다.
        * 하나의 다항식이 시작되고 끝나는 인덱스를 관리해야 한다.
        * 다항식의 연산을 구현하는 것이 어려워진다. 또한, 다항식에 따라서는 첫번째 방법보다 공간을 더 많이 필요로 할 수 있다.

* 배열의 응용 : 희소행렬
* 배열을 이용해서 행렬을 나타낼 수 있다.
    * 첫번째 방법 : 이차원 배열을 사용한다.
        * 가장 자연스러운 방법이다.
        * 배열을 나타내고, 확인 및 계산하기에 가장 간편하다.
        * 다항식과 마찬가지로 희소행렬의 경우 공간 낭비가 심하다.
        * 'int matric[rows][columns]'과 같이 선언한다.
    * 두번째 방법 : 값이 0이 아닌 위치만 포함하는 구조체 배열을 포함하는 구조체로 만든다.
        * 구조체 배열은 다음과 같은 모습이다.
        ```c
        typedef struct {
            int row;
            int column;
            int value;
        } element;
        ```
        * 위에 element배열을 요소로 갖는 matrix 구조체를 만든다.
        ```c
        typedef struct matrix {
            element data[MAX_TERMS];
            int rows;
            int columns;
            int terms;
        } matrix;
        ```
        * 이렇게 두번째 방법은 0이 많은 행렬에서 공간 효율적이지만, 계산이 복잡해진다는 단점이 있다.

* 포인터 : 주소를 갖는 변수
* 모든 변수는 메모리 공간에 저장되며, 메모리의 각 바이트에는 주소가 매겨져있다.
    ```c
    int a = 100;
    int *p;
    p = &a;
    ```
    1. 메모리공간에 변수 a를 선언함과 동시에 100으로 초기화한다.
    2. int형를 가리키는 포인터 변수 p를 선언한다.
    3. p의 값으로 a의 주소를 대입한다.
* 포인터와 관련된 연산자
    * "&" : 주소 연산자, 변수의 주소를 추출하는 연산자
    * "*" : 간접참조 연산자, 포인터가 가리키는 장소에 값을 저장하는 연산자
* 널 포인터
    * 널 포인터는 어떤 객체도 가리키는 않는 포인터이다. 
    * **포인터를 사용하기 전에는 반드시 널 포인터인지 검사해야한다.**
    * 잘못된 포인터를 가지고 메모리를 변경하는 경우 치명적인 결과를 불러올 수 있는데,
    * 널 포인터에 참조하려고 하면 오류가 발생하기 때문에 쉽게 파악이 가능하다.
    * 따라서 포인터가 아무것도 가리키고 있지 않을 때는 항상 널 포인터 상태롤 만들어 두는 것이 좋다.
* 배열과 포인터
    * 포인터를 사용하면 함수에 값을 전달해서, 내부에서 매개변수 값을 변경할 수 있다.
    * 다만, 배열을 포인터를 사용하지 않아도 값이 변경 가능하다. 이는 **배열의 이름이 배열의 첫번째 원소를 가리키는 포인터이기 때문이다.**
    * 다시말해 배열을 선언하고 초기화하면, 배열의 이름에는 배열의 첫번째 원소의 주소가 저장된다.
    * 일반적으로 변수가 call by value로 함수에 전달될 때, 변수 값에 대한 복사가 일어난다.
    * c언어에서는 배열의 이름이 포인터이기 때문에, 배열에 대한 복사가 일어나지 않아 더 빠르고 공간 효율적인 프로그램 작성이 가능하다.
    * **'int arr[10];'과 같이 정적할당 되는 배열은 스택영역에 생성된다.**   
* 동적메모리 할당, Dynamic memory allocation
* 프로그램 작성 당시에는 입력의 크기를 알 수 없는 경우가 많다. 이에따라 발생하는 문제들을 해결하기 위해 C언어에서는
* 필요한만큼의 메모리를 운영체제로부터 할당받아 사용하고, 사용이 끝나면 시스템에 반납하는 기능이 있다. 이를 동적 메모리 할당이라고 한다.
* **동적 메모리는 메모리공간 중 '힙(heap)영역'에 할당된다.** 힙은 운영체가자 사용되지 않는 메모리 공간을 모아놓은 공간이다.
    ```c
    int *p;
    p = (int *)malloc(sizeof(int));     # 동적 메모리 할당
    *p = 1000;          # 동적 메모리 사용
    free(p);            # 동적 메모리 반납
    ```
    1. malloc()은 size바이트 만큼의 메모리 블록을 할당한다. sizeof 키워드는 변수나 타입의 크기를 숫자로 반환한다.
    2. malloc()은 동적메모리 블럭의 시작주소를 반환한다. 반환되는 주소는 'void *'타입이므로 형변환 해서 사용한다.
    3. 동적 메모리는 포인터로만 사용가능하다. free()함수는 할당된 메모리 블록은 운영체제에 반납한다. 
    4. 메모리 반환을 위해선 할당받은 포인터 값을 기억해야한다. malloc() 함수가 반환했던 포인터 값을 잊어버리면 반환할 수 없다..
    5. **malloc은 메모리를 할당할 수 없으면 NULL을 반환한다. 따라서 malloc()의 반환값은 항상 NULL검사를 해야한다.**
    
    * 정수 10개를 저장할 수 있는 동적메모리 할당
    ```c
    #define SIZE 10
    int *p;
    p = (int *)malloc(SIZE * sizeof(int));
    if (p == NULL) {
        fprintf(stderr, "할당할 수 있는 메모리가 없습니다.");
        exit(1);
    }
    for (int i=0; i<SIZE; i++) {
        p[i] = i;
    }
    for (int i=.; i<SIZE; i++) {
        printf("%d ", p[i]);
    }
    free(p)
    ```
* 구조체와 포인터
    * 구조체도 물론, 구조체 포인터를 선언하고 이를 통해 구조체 멤버에 접근할 수 있다.
    * '*'와 '.'연산자를 이용하여 구조체 멤버에 접근할 수도 있지만, 일반적으로 더 편리한 '->'를 자주 사용한다.
    * C에서는 자주 함수의 매개변수로 구조체가 전달된다. 구조체 자제를 함수로 전달하는 경우,
    * 구조체가 복사되어 전달되기 때문에, 큰 구조체의 경우에는 구조체 포인터를 전달하는 것이 좋다.


## 4. 스택
* 스택이란?
    * 스택은 후입선출(LIFO), 최상단에서만 데이터의 입출력이 일어나는 자료구조를 말한다.
    * 스택에서 입출력이 일어나는 부분을 top, 이와 대응되는 부분을 bottom, 스택에 저장되는 요소를 element라고 한다.
    * 스택은 출력이 입력의 역순으로 이루어져야 할 경우 유용하게 사용된다. 예로, 워드프로세서의 되돌리기 기능. 시스템 스택을 이용한 함수 호출 등이 있다.
        * 시스템 스택을 이용한 함수 호출
            * 함수는 호출된 역순으로 되돌아간다. 이를 위해 스택에 복귀할 주소를 저장한다.
            * 시스템 스택에는 함수가 호출될 때마다 활성레코드가 만들어지며 여기에 복귀주소가 저장된다.
            * 활성레코드에는 프로그램카운터 뿐 아니라 함수 호출시 매개변수와 함수 안에서 선언된 지역 변수들이 같이 생성된다.
* 스택 ADT 
    * 스택의 push()와 pop()을 구현하기 위해, is_full() is_empty() 함수를 먼저 작성하는 것을 잊지말자
    * peek() 연산은 pop()과 비슷한데, 유일하게 다른점으로 peek()은 데이터를 삭제하지 않는다.
    * 스택을 **구현하는 방법은 1.배열을 이용한 방법 2.연결리스트를 이용한 방법**이 있다.
    * 배열을 이용하면 간단하고 성능이 좋지만 길이가 고정, 연결리스트는 복잡하지만 가변적이다.
* 스택 구현
    * 복잡한 데이터를 저장하기 위해, **구조체 element를 갖는 스택을 기본으로 한다.**
    * C에서 모든 자료구조는 다음과 같이 구조체로 정의하고, 이를 위한 연산에는 구조체의 포인터를 인자로 전달한다.
        ```c
        typedef struct {
            element data[MAX_STACK_SIZE];
            int top;
        } StackType;
        ```
    * 포인터로 전달하는 이유
        1. 그냥 구조체를 전달하게 되면 call by value에 의해 구조체가 복사되어 전달된다. => 함수호출로 인한 오버헤드 증가
        2. 포인터로 '주소'를 전달해야 값을 바꿀 수 있음.
    * 동적 배열 스택
        * 위는 정적 배열, 컴파일 시간에 크기가 결정되고 이를 위해선 배열의 최대크기를 미리 알아야한다.
        * C에서는 malloc()을 이용해 동적으로 메모리를 할당받을 수 있다. realloc()은 현재 내용은 유지하면서 주어진 크기로 동적메모리를 다시 할당한다.
        ```c
        typedef struct {
            element *data;
            int capacity;       // 스택의 용량
            int top;
        } StackType;
        
        void init_stack(StackType *s) {
            s->capacity = 1;
            s->top = -1;
            s->data = (element *)malloc(s->capacity * sizeof(element));     // 실제로는 null검사를 해야한다.
        }

        void push(StackType *s, element item) {
            if (is_full(s)) {
                s->capacity *= 2;
                s->data = (element *)realloc(s->data, s->capacity * sizeof(element));
            }
            s->data[++(s->top)] = item;
        }
        ```
* 스택 자료구조 응용
    1. 괄호 검사
    2. equation 계산, 중위 -> 후위표기 변환, 후위표기된 수식의 계산
    3. 미로 문제(maze solving problem)


## 5. 큐, queue
* 선입선출(FIFO), 뒤(후단, rear)에서 데이터 추가, 앞(전단, front)에서 하나씩 추출. **데이터의 입출력이 일어나는 부분이 다름**
* **즉, 큐는 데이터가 들어온 순서대로 나간다.** 스택과 마찬가지로 **큐도 배열과 연결리스트를 이용하여 구현할 수 있다.**
* 스택에 push()와 pop()이 있다면, queue에는 enqueue와 dequeue가 있다.
    * 큐에서 데이터를 삽입하고, 삭제하는 연산은 O(1)의 시간복잡도를 갖는다.
* enqueue는 큐에 후단(rear)에 새로운 요소를 추가하고, dequeue는 전단(front)에서 요소를 추출하고 삭제한다.
* 큐는 현실세계 시뮬레이션에 많이 사용. 은행 대기줄, **인터넷에서 전송되는 데이터 패킷 등을 모델링하는데 사용한다.**
* 큐는 버퍼의 역할로 컴퓨터와 주변장치 사이에도 많이 사용된다.
* 선형큐(linear queue)
    * 일차 배열과, front, rear 변수 필요. front, rear은 초기 -1에서 시작하여 각각 enqueeu, dequeue가 일어날 때 하나씩 증가.
    * 문제점 : 선형큐에서 front와 rear은 계속해서 증가만한다. 언젠가는 배열에 끝에 도달하게 되고 이미 지나간 빈공간은 낭비된다.
    * 모든 요소를 주기적으로 왼쪽으로 모을 수도 있다. 다만 이 방법도 복잡하고 비효율적이다.
* 원형큐(Circular queue)
    * 선형큐에 문제를 해결. 배열을 단방향의 선형이 아닌 원형으로 이해하는 방법이다.
    * front와 rear이 배열의 끝에 다다르면 다음에 증가되는 값은 0이 되도록 만든다.
    * **원형 큐에서는 front와 rear의 값이 0부터 시작되며, front는 항상 큐의 첫번째 요소의 하나 앞을, rear는 마지막 요소를 가리킨다.**
        * 초기상태에서 enqueue()를 시행하면 인덱스 1부터 값이 저장된다. enqueue, dequeue 모두 전위연산자로 구현
    * **front와 rear의 값이 같으면 원형큐가 비어있음을 나타낸다.** 
        * 위와 같이 나타내기 위해(포화상태와 공백상태를 구분하기 위해) 원형큐에서 하나의 자리는 항상 비워둔다.
        * 따라서 front == rear는 공백상태이며 front가 rear보다 하나 앞에 있으면 포화상태가 된다. 
        * 요소의 개수를 카운팅하는 count변수를 추가로 만들어 한자리를 비우지 않게 구현할 수도 있다.
    * **배열을 원형으로 회전시키기 위해, 모듈로(modulo)연산을 이용한다.**
        ```c
        void enqueue(QueueType *q, element item) {
            if (is_full(q)) {
                fprintf(stderr, "큐가 가득참");
                exit(1);
            }
            q->rear = (q->rear + 1) % MAX_QUEUE_SIZE;
            q->queue[q->rear] = item;
        }
        ```
* 버퍼, 큐의 응용
    * (데이터) **생산자-소비자 프로세스 : 큐를 버퍼로 사용**
    * **CPU 스케줄링** : 운영체제는 실행 가능한 프로세스를 저장하거나 이벤트를 기다리는 프로세스를 저장하기 위해 큐를 사용한다.
* 덱, deque(double-ended queue)
    * 큐의 **front와 rear에서 모두 삽입과 삭제가 가능한 큐를 deque이라고 한다.**
    * 역시 중간에서의 삽입과 삭제는 허용되지 않는다. 마찬가지로 배열과 연결리스트를 이용해 구현이 가능하다.
    * 덱은 원형큐와 비슷해서 좀만 확장하면 쉽게 구현이 가능하다.
        * 원형큐에서 delete_rear(), add_front(), get_rear()의 연산을 추가한다.
        * front에서 추가, rear에서 삭제가 일어날 때, 각각 front와 rear를 감소시켜야 한다. 음수가 되는 경우 MAX_SIZE만큼 더 해줘야한다.
        ```c
        void add_front(DequeType *q, element val) {
            // 포화상태이면...
            q->data[q->front] = val;
            q->front = (q->front -1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
        }
        
        element delete_rear(DequeType *q) {
            int prev = q->rear;
            // 공백상태이면..
            q->rear = (q->rear - 1 + MAX_QUEUE_SIZE) % MAX_QUEUE_SIZE;
            return q->data[prev];
        }
        ```


## 6. 연결 리스트 I
* 리스트(list)의 항목들은 순서 또는 위치를 가진다.
    * 집합의 원소들은 순서가 없고, 리스트는 순서가 있다는 점에서 둘은 다르다.
    * 스택과 큐도 넓게 보면 리스트의 일종이다.
* 리스트의 기본적인 연산은 다음과 같다.
    * 리스트에 원소 삽입(삽입 연산)
    * 리스트에서 항목 삭제(삭제 연산)
    * 리스트에서 특정한 항목 탐색(탐색 연산)
* 리스트 ADT
    * 리스트 객체 : n개의 element형으로 구성된 '순서 있는' 모임, 선형 자료구조
    * 리스트의 삽입연산은 처음, 끝, 중간 모두에서 가능하다.
    * 삭제 연산은 특정위치의 요소를 제거하고, 모든 원소를 삭제하는 clear() 함수도 제공한다.
    * 탐색 연산은 pos 위치의 요소를 반환한다.
    * 그 외, 리스트의 길이, empty, full과 같은 상태를 나타내는 연산 들을 제공한다.
* 리스트의 구현
    * 리스트의 구현에는 두가지 방법이 있다.
        1. 배열을 이용한 구현
            * 간단하고 빠르지만, 크기가 고정된다.
            * **리스트의 중간에 새로운 데이터를 삽입하거나 삭제하기 위해서는 기존 데이터들을 이동시켜야 한다.**
        2. 연결리스트를 이용한 구현
            * 연결리스트는 크기가 제한되지 않고, 중간에서 쉽게 삽입하거나 삭제할 수 있는 유연한 리스트를 구현 가능하다.
            * 단점으로는 임의의 항목을 추출하려고 할 때는 배열을 사용하는 방법보다 시간이 많이 걸린다.
    * 배열을 이용한 구현은 거의 사용하지 않는다.
* 배열로 구현된 리스트
    * 배열을 이용하여 리스트를 구현하면, 순차적인 메모리 공간이 할당된다. 이것을 리스트의 **순차적 표현**(sequential representation)이라고 한다.
    * 배열로 리스트를 구현할 때, 주의할 점은 리스트의 삽입, 삭제 연산이 일어날 때이다.
    * **문제점 : 리스트의 마지막이 아닌 부분에서 삽입, 삭제 연산이 일어날 경우 각 행동이 실행된 위치를 기준으로 앞 또는 뒤의 모든 원소들을 한칸씩 밀어줘야 한다.**
* 연결 리스트(linked list)
    * 연결리스트는 동적으로 크기가 변할 수 있고 삭제나 삽입시에 데이터를 이동할 필요가 없다.
    * 이를 **연결된 표현(linked representation)**이라고 하며, 이는 포인터를 사용하여 데이터들을 연결하는 것을 말한다.
        * 연결된 표현은 리스트뿐만 아니라, 트리, 그래프, 스택, 큐 등 다양한 자료형의 구현에 사용된다.
    * 연결 리스트의 데이터들은 메인 메모리상에서 따로 떨어져있다. 떨어진 각각의 데이터를 포인터를 이용하여 연결한다.
    * linked list는 데이터의 추가, 삭제가 자유롭다. linked list끼리는 첫 번째 데이터를 확인함으로써 구분할 수 있다.
    * 단점으론 포인터를 추가로 저장하기 때문에 메모리를 많이먹고, i번째 데이터를 찾기 위해서는 앞에서부터 순차적으로 접근해야한다.
* 연결 리스트의 구조
    * 연결 리스트는 데이터를 저장하는 노드(node)들의 집합이다.
    * node = data field + link field
    * 연결 리스트의 대표, 첫번째 노드를 가리키는 변수를 Head Pointer라고 한다.
    * 마지막 노드의 링크는 NULL로 초기화한다.
    * 노드에는 데이터와 링크가 있어야한다. 따라서 c에서는 구조체로 정의된다.
* 연결 리스트의 종류
    1. 단순(Singly) 연결 리스트 : 하나의 방향으로 연결된 리스트, 마지막 노드의 링크필드는 NULL로 초기화. chain이라고도 함.
    2. 원형(Cicular) 연결 리스트 : 마지막 노드의 링크가 첫번째 노드의 포인터를 가지고 있다.
    3. 이중(doubly) 연결 리스트 : 각 노드는 앞, 뒤 2개의 링크를 갖는다.
* 단순 연결 리스트(Singly linked list)
    * 노드가 하나의 링크만을 갖는다. 
    * 마지막 노드의 링크는 NULL
    * 노드의 정의
        * '**자기 참조 구조체**'를 이용한다. => 자기 자신을 참조하는 포인터를 포함하는 구조체.
        * element 타입의 데이터 필드와 포인터가 저장되는 링크 필드로 정의
        * link 필드는 ListNode를 가리키는 포인터로 정의, 다음 노드의 주소를 저장한다.
        ```c
        typedef int element;
        typedef struct {
            element data;
            struct ListNode *link;  // 자기 참조 구조체
        } ListNode;
        ```
    * 노드의 생성
        * 연결 리스트의 모든 데이터의 접근하기 위해, 헤드 포인터가 필요하다.
        * 노드가 없는 단순 연결리스트는 다음과 같이 생성한다.
            ```c
            ListNode *head = NULL;
            ```
        * 즉, 리스트의 공백검사는 헤드 포인터가 NULL인지 확인한다.
        * 연결리스트는 필요할 때마다 노드를 동적 할당하여 생성한다. 
            ```c
            head = (ListNode *)malloc(sizeof(ListNode));
            head->data =10; // 노드에 데이터를 저장한다.
            head->link = NULL;
            ```
    * 노드의 연결
        * 노드의 연결은 간단하게, 새로운 노드를 동적으로 생성 및 데이터를 저장하고 이전 노드의 링크에 생성한 노드의 주소를 넣는다.
            ```c
            ListNode *p; 
            p = (ListNode *)malloc(sizeof(ListNode));
            p->data = 20;
            p->link = NULL;

            head->link = p; // head에 p노드를 연결한다.
            ```
    * 단순 연결 리스트의 연산 구현
        * 연결 리스트에는 대표적으로 다섯가지 연산이 있다.
            1. insert_first() : 맨앞에 데이터 추가
            2. insert() : 중간 위치에 데이터 추가
            3. delete_first() : 맨앞에 데이터 삭제
            4. delete() : 중간 위치에 데이터 삭제
            5. print_list : 연결 리스트를 순회하며 출력

    * 다항식 : 연결 리스트 응용
        * 단순 연결 리스트로 다항식을 표현하려면, 계수, 지수, 링크로 필드가 세개인 구조체 노드를 선언한다.
            ```c
            typedef struct ListNode {
                int coef:
                int expon;
                struct ListNode *link;
            } ListNode;
            ```
        * 포인터를 이용하여 다항식의 연산을 처리하는 함수를 만들 수 있다.
        * 두 다항식을 저장한 연결리스트의 헤드 포인터가 A, B일 때 각각 p, q 포인터로 순회하며
            1. p.expon == q.expon
                * 두 계수를 더해 0이 아니면 새로운 항을 만들어 다항식 C에 추가한다. p와 q 모두 다음 항으로 이동한다.
            2. p.expon > q.expon
                * p의 항을 새로운 항으로 복사하여 다항식 C에 추가한다. p만 다음 항으로 이동한다.
            3. p.expon < q.expon
                * q의 항을 새로운 항으로 복사하여 다항식 C에 추가한다. q만 다음 항으로 이동한다.
            * 위 과정을 p나 q가 NULL이 될 때 까지 반복한다. 하나가 NULL이 되면 나머지를 전부 C로 가져온다.
    
    * 헤더 노드
        * 연결 리스트를 좀 더 효율적으로 다루기 위하여, head 뿐 아니라 tail(마지막 노드를 가리킴)도 저장하는 특수한 노드를 운용할 수 있다.
        * 많은 경우 연결 리스트의 노드의 갯수를 세는 size 변수도 같이 구현한다.
        * 헤더 노드를 이용하면 연결 리스트의 마지막에 노드를 추가할 때, 여러 편리함이 있다.
        * 헤더 노드를 ListType이라는 구조체로 만들어 사용한다.
            ```c
            typedef struct ListType {
                int size;
                ListNode *head;
                ListNode *tail;
            } ListType;
            ```
        * 헤더노드를 운용하기 위해선, 헤더 노드를 동적으로 생성하고 초기화해야한다.
            ```c
            ListType* create() {
                ListType *plist = (ListType *)malloc(sizeof(ListType));
                plist->size = 0;
                plist->head = plist->tail = NULL;
                return plist;
            }
            ```
        

## 7. 연결 리스트 II
* 원형 연결 리스트(Circular linked list)
    * 원형 연결 리스트는 단순 연결 리스트에서 마지막 노드의 링크가 첫 노드를 가리키는 형태이다.
    * 따라서 모든 노드가 연결되어, 원형 연결 리스트는 어떤 노드에서든 모든 노드를 탐색할 수 있다.
    * 원형 연결 리스트를 변형하여 Head 포인터가 마지막 노드를 가리키게 한다면, 전단과 후단에 삽입과 삭제를 상수시간에 해결할 수 있다.
    * 이 경우, 마지막 노드는 Head포인터가, 첫 노드는 Head->link가 가리킨다.
    * 원형 연결 리스트의 사용
        1. 운영체제의 프로그램 스케줄링
            * 실핼중인 모든 응용 프로그램을 원형 연결 리스트에 저장하고
            * 운영체제가 원형 연결 리스트에 있는 프로그램 실행을 위해 고정된 시간 슬롯을 제공한다.
        2. 원형 큐의 구현
            * front와 rear 두 개의 포인터를 갖는 원형 연결 리스트로 크기가 자유자재인 원형 큐를 구현할 수 있다.

* 이중 연결 리스트(Double linked list)
    * 단순, 원형 연결 리스트는 선행 노드를 찾기가 어렵다.
    * 이중 연결 리스트는 이런 단점을 극복하고자, llink, rlink 각각 선행, 후속 노드에 대한 정보를 갖는다.
    * 대개, 이중 연결 리스트와 원형 연결 리스트를 합친 형태를 많이 사용한며, 헤드 노드(head node)라는 특별한 노드를 추가한 형태이다.
    * 헤드 노드는 데이터를 가지고 있지 않고 처음과 끝 노드를 가리킨다. 이는 삽입과 삭제 알고리즘을 간편하게 만드는 트릭이다.
    * 이중 연결 리스트에서 포인터 p가 임의의 노드라고 할 때, p = p->llink->rlink = p->rlink->llink의 관계가 항상 성립한다.
    * 헤드 노드는 포인터 변수가 아닌, 구조체 변수이다. 이중 연결 리스트를 사용하기 전에 반드시 초기화 해야 한다.
    * 구현
        ```c
        typedef int element;
        typedef struct DListNode {  // 이중 연결 노드 타입
            element data;
            struct DListNode *llink;
            struct DListNode *rlink;
        } DListNode;
        ```
    * 연산
        1. 삽입 연산
            ```c
            void dinsert(DListNode *before, element data) {
                DListNode *newnode = (DListNode *)malloc(sizeof(DListNode));
                newnode->data = data;
                newnode->llink = before;
                newnode->rlink = before->rlink;
                before->rlink->llink = newnode;
                before->rlink = newnode;
            }
            ```
        2. 삭제 연산
            ```c
            void ddelete(DListNode *head, DListNode *removed) {
                if (head == removed) return;
                removed->llink->rlink = removed->rlink;
                removed->rlink->llink = removed->llink;
                free(removed);
            }
            ```

* 연결된 스택(linked stack)
    * 스택을 연결 리스트로 구현하면 크기에 제한을 받지 않는다. 즉, 동적할당을 받을 수 있다면 크기가 무한정 늘어난다.
    * 반면, 삽입, 삭제 연산에 동적 메모리 할당이나 해제를 해야 하므로 시간이 좀 더 걸린다.
    * 연결된 스택은 단순 연결 리스트로 충분히 구현 가능하다. **top을 정수가 아닌 노드를 가리키는 포인터로 선언**
    * 공백 상태는 top 포인터가 NULL인 경우.
    * 구현
        ```c
        typedef struct StackNode {
            element data;
            struct StackNode *link;
        } StackNode;

        typedef struct {
            StackNode *top;
        } LinkedStackType;  // 관련된 데이터는 top 포인터 뿐이지만 일관성을 위해 구조체로 작성
        ```
    * 연산
        1. push()
            ```c
            void push(LinkedStackType *s, element item) {
                StackNode *newnode = (StackNode *)malloc(sizeof(StackNode));
                newnode->data = item;
                newnode->link = s->top;
                s->top = newnode;
            }
            ```
        2. pop()
            ```c
            element pop(LinkedStackType *s) {
                if (is_empty(s)) {
                    fprintf(stderr, "스택이 비어있음\n");
                    exit(1);
                }
                StackNode *removed = s->top;
                element value = removed->data;
                s->top = removed->link;
                free(removed);
                return value;
            }
            ```

* 연결된 큐(linked queue)
    * 크기가 제한되지 않는 반면, 링크 필드 때문에 메모리 공간을 더 많이 사용
    * 단순 연결 리스트에 2개의 포인터를 추가, front와 rear가 처음과 끝 노드를 가리킨다.
    * front는 삭제 연산과, rear는 삽입 연산과 관련이 있다. 공백 상태의 경우 front와 rear는 NULL이 된다.
    * 구현
        ```c
        typedef struct QueueNode {
            element data;
            struct QueueNode *link;
        } QueueNode;

        typedef struct {
            QueueNode *front, *rear;
        } LinkedQueueType;
        ```
    * 연산
        1. enqueue()
            ``` c
            void enqueue(LinkedQueueType *s, element data) {
                QueueNode *newnode = (QueueNode *)malloc(sizeof(QueueNode));
                newnode->data = data;
                newnode->link = NULL;
                if (is_empty(s)) {
                s->front = s->rear = newnode;
                }
                s->rear->link = newnode;
                s->rear = newnode;
            }
            ```
        2. dequeue()
            ```c
            element dequeu(LinkedQueueType *s) {
                if (is_empty(s)) {
                    frpintf(stderr, "큐가 비어있습니다.\n");
                    exit(1);
                }
                QueueNode *removed = s->front;
                element value = removed->data;
                s->front = removed->link;
                if (s->front == NULL) {
                    s->rear = NULL;
                }
                free(removed);
                return value;
            }
            ```


## 8. 트리(tree)
* 트리는 부모-자식 관계의 노드들로 이루어진 계층적인 구조의 자료구조이다.
    * 트리에서 가장 높은 요소를 root, 나머지 노드들을 서브트리(subtree)라고 부른다.
    * subtree 내부에서도 root와 subtree의 관계로 나눌 수 있다
    * 트리는 root와 subtree는 간선(edge)으로 이어져 있다.
    * 노드들 간에는 부모-자식 관계, 형제 관계, 조상-자손 관계가 있다.
    * 자식이 없는 노드를 단말 노드(leaf node, terminal node), 자식이 있는 노드를 비단말 노드(nonterminal node)라고 한다.
    * 차수는 한 노드가 가지는 자식 수를 의미하며, 레벨(level)은 루트가 1부터 시작해서 내려갈수록 숫자가 커진다.
    * 트리의 차수는 트리가 가진 노드의 차수 중 가장 큰 값이며 높이(height)는 최대 레벨과 같다. 트리의 집합은 포레스트(forest)라고 한다.
    * 결정 트리(decision tree) : 인간의 의사 결정 구조를 표현하는 방법 중 하나. 인공지능 문제에서 자주 사용
* 이진 트리(binary tree) : 가장 많이 쓰이는 트리.
    * 모든 노드가 2개의 서브트리를 가지고 있다. 서브트리는 공집합일 수 있다.
    * 즉, 이진트리의 모든 노드는 최대 2개의 자식 노드가 존재한다.(모든 노드의 차수가 2 이하)
    * 서브트리 간에 순서가 존재한다. 왼쪽과 오른쪽의 서브 트리는 서로 구별된다.
    * 이진 트리의 정의
        1. 공집합이거나
        2. 루트와 왼쪽 서브 트리, 오른쪽 서브 트리로 구성된 노드들의 유한 집합으로 정의된다. 이진트리의 서브트리들은 모두 이진트리여야 한다.
    * 이진트리의 성질
        * 높이가 h인 이진트리
            * 최소 h개의 노드를 가지며, 최대 2**h - 1개의 노드를 가진다.
        * n개의 노드를 가지는 이진트리
            * 최대 n의 높이를 가지며, 최소 upper(log(n+1))개의 노드를 가진다.(log의 base는 2)
    * 이진트리의 분류
        1. 포화 이진 트리(full binary tree) : 각 레벨에 노드가 꽉 차있는 이진트리. 높이가 k인 경우, 노드 2**k - 1개를 갖는다. 
            * 노드의 번호를 부여: 레벨 단위로 왼쪽->오른쪽 순서로 번호를 붙인다.
        2. 완전 이진 트리(complete binary tree) : 높이가 k인 경우 1부터 k-1레벨까지 노드가 모두 채워져 있고, 마지막 레벨에서는 노드가 왼쪽에서부터 중간에 빈 곳 없이 있는 트리(k레벨이 꼭 다 차진 않아도 됨)
            * 포화 이진 트리와 같은 방법으로 노드에 번호를 붙인다.
        3. 기타 이진 트리
* 이진 트리의 표현
    * 배열 표현법
        * 저장하고자 하는 이진 트리를 일단 완전 이진 트리라고 가정하고 번호에 맞는 인덱스에 값을 저장한다.
        * 경사 이진 트리나나 기타 이진 트리의 경우 낭비되는 공간이 많음
        * 이진 트리의 깊이가 k이면, 2**k개의 공간을 연속적으로 할당한 다음 완전 이진 트리의 번호대로 노드들을 저장한다. 
        * 루트가 1부터 시작하므로 배열의 0번째 요소는 사용하지 않는다. 
        * 장점 : 인덱스만 알면, 노드의 부모나 자식을 쉽게 알 수 있다.
            * 노드 i의 부모 노드 인덱스 = i // 2
            * 노드 i의 왼쪽 자식 노드 인덱스 = i * 2
            * 노드 i의 오른쪽 자식 노드 인덱스 = i * 2 + 1
    * 링크 표현법
        * 트리의 노드가 구조체로 표현되고, 각 노드가 왼쪽, 오른쪽 자식을 가리키는 두 개의 포인터를 갖는다.
        * 구현
            ```c
            typedef struct TreeNode {
                element data;
                struct TreeNode *left, *right;
            } TreeNode;
            ```
        * **자식이 없는 쪽은 NULL 값을 갖는다.**
        * 장점 : 루트 노드를 가리키는 포인터만 있으면 트리안의 모든 노드에 접근할 수 있다.
* 이진 트리의 순회
    * 순회(traversal)은 이진 트리에 속하는 모든 노드를 한 번씩 방문하여 노드가 가지고 있는 데이터를 목적에 맞게 처리하는 것을 의미한다.
    * 스택, 큐, 리스트 등은 데이터를 선형으로 저장함 -> 순회 방법이 한 가지
    * 하지만 트리는 계층적이고 2차원적인 자료구조 -> 순회 방법이 여러가지 있을 수 있음
    * 이진 트리의 순회 방법은 루트와 왼쪽 서브트리, 오른쪽 서브트리를 어떻게 방문하냐에 따라 전위, 중위, 후위로 나뉜다.
    * 여기서, 왼쪽 서브트리는 항상 오른쪽 서브트리보다 일찍 방문하고, 전위, 중위, 후위의 이름은 루트를 방문하는 시기에 따라 이름 붙힌다.
    * 루트 방문을 V, 왼쪽 서브트리 방문을 L, 오른쪽 서브트리 방문을 R이라 하면 
        1. 전위(preorder traversal), VLR
        2. 중위(inorder traversal), LVR
        3. 후위(postorder traversal), LRV
        * 일반적으로 이진 트리의 순회 알고리즘은 순환 기법을 이용하여 서브트리에도 같은 순회 방문 순서를 구현한다.
    
    * **전위 순회(preorder traversal)**
        * 루트를 먼저 방문하고 그 다음에 왼쪽, 오른쪽 서브트리 순으로 방문한다.
            1. 루트 노드 방문
            2. 왼쪽 서브트리 방문
            3. 오른쪽 서브트리 방문
        * 전위 순회 스도코드
            ```c
            preoreder(x):
            if x != NULL:
                then print(DATA(x));
                preorder(LEFT(x));
                preorder(RIGHT(x));
            ```
    * **중위 순회(inorder traversal)**
        * 왼쪽 서브트리, 루트, 오른쪽 서브트리 순으로 방문한다.
        * 중위 순회 스도코드
            ```c
            inoreder(x):
            if x != NULL:
                then inorder(LEFT(x));
                print(DATA(x));
                inorder(RIGHT(x));
            ```
        * 즉, 전위, 중위 그리고 뒤에 나올 후위 순회 알고리즘까지 호출의 순서만 다르다.
    * **후위 순회(postorder traversal)**
        * 왼쪽 서브트리, 오른쪽 서브트리, 루트 순으로 방문한다.
            ```c
            void postorder(TreeNode *root) {
                if (root != NULL) {
                    postorder(root->left);
                    postorder(root->right);
                    printf("%d", root->data);
                }
            }
            ```
    * 반복적 순회 알고리즘 - DataStructure-ReprtitiveTraversal 참조.
        * 반복을 이용해 순회 알고리즘을 만들기 위해선, 스택이 필요하다.
        * 스택에 자식 노드들을 저장하고 꺼내면서 순회를 할 수 있다.
        * 순환 호출도 사실은 시스템 스택을 사용하는 것이기 때문에, 별도의 스택을 사용하면 순환 호출을 흉내낼 수 있다.
    
    * 레벨 순회(level order)
        * 각 노드를 레벨 순으로 검사. 같은 레벨의 경우 왼쪽에서 오른쪽으로 순회
        * 전위, 중위, 후위 방법은 모두 스택을 사용한 방법.(반복이든, 순환(시스템스택)이든)
        * 레벨 순회는 큐 자료구조를 사용한다. bfs와 같다.
        * 먼저 큐에 있는 노드를 꺼내어 방문한 다음, 그 노드의 자식 노드를 큐에 삽입하는 과정을 큐가 빈 상태가 될 때까지 반복한다.
        * 구현
        ```c
        void level_order(TreeNode *root) {
            QueueType q;
            init_queue(&q);

            if (root == NULL) return;
            enqueue(&q, root);
            while (!is_empty(&q)) {
                root = dequeue(&q);
                printf(" [%d] ", root->data);
                if (root->left) {
                    enqueue(&q, root->left);
                }
                if (root->right) {
                    enqueue(&q, root->right);
                }
            }
        }
        ```
    * 문제에 따라 순회 방법을 선택. 부모 -> 자식 순으로 노드를 처리하기 위해선 전위 순회를 사용해야 한다.
    * 반대로 자식노드를 처리한 다음, 부모 노드를 처리하려면 후위 순회를 사용한다. 
    * 예를 들어 디렉토리의 용량을 계산하는 문제는 후위 순회를 사용해야 한다.

* 트리의 응용
    1. 수식 트리 처리
        * 수식 트리는(exprssion tree)는 단말노드는 피연산자, 비단말노드(nonterminal node)는 연산자가 된다.
        * 수식 트리를 각각 전위, 중위, 후위 순회 방법으로 읽으면, 신기하게도 각각 전위, 중위 후위 표기 수식이 된다. 
    2. 디렉토리 용량
        * 디렉토리 용량을 구하기 위해선 하위 디렉토리의 용량부터 구해야함 (후위 순회)

* 이진트리의 추가 연산
    * 노드의 개수
        ```c
        int get_node_count(TreeNode *root) {
            int count = 0;
            if (root != NULL)
                count = 1 + get_node_count(root->left) + get_node_count(root->right);
            return count;
        }
        ```
    
    * 단말 노드 개수
        ```c
        int get_leaf_node(TreeNode *root) {
            int count = 0;                
            if (root != NULL) {
                if (root->left == NULL && root->right == NULL) {
                    return 1;
                } else {
                    count = get_leaf_node(root->left) + get_leaf_node(root->right);
                }
            }
            return count;
        }
        ```

    * 높이 구하기
        ```c
        int get_height(TreeNode *root) {
            int height = 0;
            if (root != NULL) {
                height = 1 + max(get_height(root->left), get_height(root->right));
            }
            return height;
        }
        ```

* 스레드 이진 트리(threaded binary tree)
    * 이진 트리의 노드가 많아지면, 순환 호출을 이용한 순회는 상당히 비효율적이 될 수 있다.
    * 스택(순환호출)을 사용하지 않고, 좀 더 효율적으로 순회하는 방법
    * 아이디어
        * n개의 노드로 이루어진 이진 트리는 총 2n개의 링크 필드를 갖는다.
        * 2n개의 링크 필드 중 n-1개의 노드와 노드를 잇는 링크를 제외한, n+1개의 링크는 NULL값을 갖는다.
        * NULL값을 갖는 링크에 중위 순회 방식에서, 선행 노드인 중위 선행자(inorder predecessor)나 후속 노드인 중위 후속자(inorder successor)를 저장시켜 놓은 트리가 스레드 이진 트리이다.
    * 이로써 각 노드들은 중위 순회의 순서대로 연결되고, 순회를 스택을 쓰지않고 효율적으로 처리할 수 있다.
    * 다만, 이 경우 NULL 링크에 스레드가 저장되면, 링크에 저장된게 자식을 가리키는 포인터인지, NULL값 대신 스레드가 저장되있는지 구별해주는 태그 필드가 필요하다.
    * 구현
        ```c
        typedef struct TreeNode {
            int data;
            struct TreeNode *left, *right;
            int is_thread;  // True이면 오른쪽 링크가 스레드
        } TreeNode;
        ```
    * 연산
        ```c
        TreeNode* find_successor(TreeNode *p) {
            TreeNode *q = p->right;
            if (q == NULL || p->is_thread == TRUE) {
                return q;
            }
            while (q->left != NULL) q = q->left;
            return q;
        }

        void thread_inorder(TreeNode *root) {
            TreeNode *q = root;

            while (q->left != NULL) q = q->left;
            do {
                printf("[%d] ", q->data);
                q = find_successor(q);
            } while (q);
        }
        ```
* 이진 탐색 트리(binary search tree)
    * 이진 탐색 트리는 이진 트리 기반의 탐색을 위한 자료구조이다.
        * 탐색
            * 탐색은 컴퓨터에서 가장 시간이 많이 걸리는 작업 중 하나. 또한, 많이 쓰이는 만큼 중요한 연산이다.
            * 탐색은 레코드(record)의 집합에서 특정한 레코드를 찾아내는 작업을 의미한다.
            * 즉, 키를 입력하여 테이블에서 특정한 키를 가진 레코드(다수의 필드로 구성)를 찾는 것이 탐색이다.
    * 정의
        1. 모든 원소의 키는 유일한 키를 가진다.
        2. 왼쪽 서브 트리 키들은 루트 키보다 작다.
        3. 오른쪽 서브 트리 키들은 루트 키보다 크다.
        4. 왼쪽과 오른쪽 서브 트리도 이진 탐색 트리이다.
    * 위의 정의에 따라, 이진 탐색 트리를 **중위 순회**할 경우 오름차순으로 정렬된 키 값이 나온다.
        * 이진 탐색트리가 어느 정도 정렬된 상태를 유지하는 것을 알 수 있다.
    * 탐색연산
        * 이진 탐색 트리에서 특정한 키값을 가진 노드를 찾기 위해서는, 먼저 주언진 탐색키 값과 루트 노드의 키값을 비교한다.
        * 같으면 탐색 성공. 탐색키가 작으면 왼쪽, 크면 오른쪽 서브트리로 가서 다시 탐색을 실행한다.
        ```c
        // 순환적 탐색연산
        TreeNode* search(TreeNode *root, int key) {
            if (root == NULL) return NULL;
            if (key == root->key) return root;
            else if (key < root->key) {
                search(root->left, key);
            } else {
                search(root->right, key);
            }
        }

        // 반복적 탐색연산, 좀 더 효율적
        TreeNode* search(TreeNode *root, int key) {
            while (root != NULL) {
                if (key == root->key) return root;
                else if (key < root->key) {
                    root = root->left;
                } else {
                    root = root->right;
                }
            }
            return NULL;
        }  
        ```
    * 삽입연산
        * 이진 탐색 트리에서 삽입 연산을 하기 위해선, 먼저 탐색이 수행되어야 한다.
        * 이진 탐색 트리에서 키 값을 고유한 값이어야 한다. 즉 탐색과정에서 같은 키 값을 찾았다면 새로 추가하지 않고 return한다.
        * 트리에 같은 키 값이 존재하지 않을 시에는 탐색을 실패한 위치가, 새로운 노드를 삽입하는 위치가 된다.
        * 새로운 노드는 항상 단말노드에 추가된다. 즉, 단말노드를 발견할 때까지 루트에서 키를 검색하고 발견되면 새로운 노드가 단말 노드의 하위 노드로 추가된다.
        ```c
        TreeNode* insert_node(TreeNode *node, int key) {
            if (node == NULL) {
                TreeNode *temp = (TreeNode *)malloc(sizeof(TreeNode));
                temp->key = key;
                temp->left = temp->right = NULL;
                return temp;
            }
            if (node->key > key) {
                node->left = insert_node(node->left, key);
            } else if (node->key < key) {
                node->right = insert_node(node->right, key);
            }
            return node;
        }
        ```
    * 삭제연산
        * 삭제연산은 이진탐색트리에서 가장 복잡한 연산이다. 
        * 먼저 노드를 삭제하기 위해서는 해당 노드가 있는지 탐색한다.
        * 노드가 탐색되면 다음 3가지 경우를 고려한다.
            1. 노드가 단말노드인 경우.
            2. 노드가 왼쪽 또는 오른쪽 서브트리 중 하나만을 가질 경우
            3. 노드가 양쪽 서브트리를 가질 경우.

        * 첫째, 삭제하려는 노드가 단말노드인 경우. - 부모노드의 링크를 NULL로 만들고 free()로 메모리를 해제한다.
        * 둘째, 한쪽 서브트리를 가질 경우. - 자기 노드는 삭제하고 서브트리는 자기 노드의 부모 노드에 붙여준다.
        * 셋째, 양쪽 서브트리 존재. - 후계자 노드를 정해, 값만 바꾸고 다시 서브트리에서 사용된 후계자 노드를 삭제한다.
            * 후계자 노드는 왼쪽 서브트리의 오른쪽 끝과, 오른쪽 서브트리의 왼쪽 끝(두 값이 삭제할 루트 노드와 가장 가까움)를 사용할 수 있다.
            * 일반적으로 오른쪽 서브트리의 후계자 노드를 사용한다.
        ```c
        TreeNode* delete_node(TreeNode *root, int key) {
            if (root == NULL) return root;
            
            if (key < root->key) {
                root->left = delete_node(root->left, key);
            } else if (key > root->key) {
                root->right = delete_node(root->right, key);
            } else {
                if (root->left == NULL) {
                    TreeNode *temp = root->right;
                    free(root);
                    return temp;
                } else if (root->right == NULL) {
                    TreeNode *temp = root->left;
                    free(root);
                    return temp;
                }
                TreeNode *temp = root->right;
                while (temp->left != NULL) temp = temp->left;
                root->key = temp->key;
                root->right = delete_node(root->right, temp->key);
            }
            return root;
        }
        ``` 
    * 이진탐색트리 분석
        * 높이가 h인 이진 탐색 트리에서의 탐색, 삽입, 삭제 연산의 시간복잡도는 O(h)가 된다. 삽입과 삭제 시, 탐색 필요.
        * 따라서 n개의 노드를 가지는 이진 탐색 트리의 경우, 일반적인 이진 트리의 높이는 log(n)(기저=2)의 상한이므로 이진 탐색 트리 연산의 평균적인 시간복잡도도 O(log(n))이다.
        * 다만, 최악의 경우 한쪽으로 치우치는 경사트리가 되어서 트리의 높이가 n이 된다면, 선형 탐색과 같은 O(n) 시간복잡도가 될 수 있다.
        * 따라서 트리의 높이를 log(n)의 상한으로 한정시키는 균형기법으로 AVL트리를 비롯한 여러 기법들이 존재한다.
    * 응용
        1. 이진탐색트리에서 가장 큰 값을 찾을 수 있으면, 우선순위 큐를 구현할 수 있다.
            * 가장 큰 값은 트리 상 가장 오른쪽에 요소이다. 즉
            ```c
            while (root->right != NULL) 
                root = root->right;
            ```
        2. 이진 탐색 트리를 중위 순회하면, 오름차순으로 정렬된 숫자를 얻을 수 있다. 내림차순으로 정렬하는 방법은?
            * 중위순회 LVR을 반대로 RVL로 진행한다.
            ```c
            void inorder_traversal(TreeNode *root) {
                if (root != NULL) {
                    inorder_traversal(root->right);
                    print("%d", root->key);
                    inorder_traversal(root->left);
                }
            }
            ```
        3. **map 자료구조의 구현**
            * map을 구현하는 것은 이진탐색트리의 가장 큰 용도이다.
            * 영어사전을 구현한 것과 같이, 트리노드의 element(key)를 key-value를 저장하는 구조체로 정의한다.
            * 그리고 비교 함수를 작성하면, 모든 원소를 일반적으로 O(log(n)) 시간에 찾을 수 있다. 최악=O(n)
        
    
## 9. 우선순위 큐(Priority Queue)
* 우선순위 큐는 데이터들이 우선 순위를 가지고 있고 우선 순위가 높은 데이터가 먼저 나가게 된다.
    * 우선순위 큐의 구현 방법
        * 우선순위 큐를 구현하는 방법은 여러가지가 있다.
        1. 배열, 연결 리스트
            * 정렬되어 있지 않은 배열(+리스트)은 삽입에 O(1), 삭제에 O(n)이 걸린다.
            * 정렬된 배열+(리스트)은 삽입에 O(n)(이분탐색 + 뒷부분 옮겨줘야함), 삭제에 O(1) 시간이 걸린다.
        2. 힙(heap)
    * 힙(heap)
        * 힙은 **완전 이진트리**의 일종으로, 우선순위 큐를 위해 고안된 자료구조이다.
        * 힙은 여러 값 중 가장 큰 값이나 가장 작은 값을 빠르게 찾아내도록 고안됐다.
        * 최대힙(max heap) 기준, 힙은 부모 노드와 자식노드간에 'key(부모) >= key(자식)'가 항상 성립한다. 최소힙(min heap)은 역으로 'key(부모) <= key(자식)'
        * 이진 탐색 트리는 중복을 허용하지 않지만, 힙은 키 값의 중복을 허용한다.
        * 힙의 목적은 삭제연산 수행 시, 가장 큰 값을 찾아내는 것. 
        * 힙은 '느슨한 정렬' 상태를 유지한다. 
        * 힙으로 만들어진 우선순위 큐는 O(logn) 시간에 삽입과 삭제 연산을 수행한다.    
    * 구현
        * 히프는 완전 이진 트리여서, 각각의 노드에 차례로 번호를 붙일 수 있다. 따라서, 힙을 저장하는 표준 자료구조는 배열이다.
        * 일반 트리와 마찬가지로 0번 인덱스는 쓰지 않는다.
        * 물론 왼쪽 자식 = 부모*2, 오른 자식 = 부모*2 + 1, 부모노드 = 자식//2가 그대로 성립한다.
        * 힙의 정의
        ```c
        #define MAX_ELEMENT 200
        typedef struct {
            int key;
        } element;
        typedef struct {
            element heap[MAX_ELEMENT];
            int heap_size;  // 힙에 저장된 요소의 개수
        } HeapType;
        ```
        * 삽입 연산
            * 힙은 새로운 값을 삽입 시킬 때, 먼저 마지막 요소 다음 인덱스에 값을 저장한다.
            * 그 후, 느슨한 정렬을 유지하기 위해 자신의 부모 노드와 값을 비교하며 위치를 바꿔나간다(승진).
            
        * 삭제 연산
            * 루트 노드를 삭제한 후에 힙의 재구성이 필요한데, 이는 힙의 성질을 만족하기 위해 위, 아래 노드를 교환하는 것을 말한다.
            * 힙에서 루트 요소를 삭제할 때, 루트의 값을 반환하고 가장 마지막에 있는 요소를 루트 자리에 가져온다.
            * 새로운 루트는 자식보다 우선순위가 낮다. 느슨한 정렬을 유지하기 위해, **자식 중에서 더 큰 값과 교환이 일어난다.(최대힙)**
    * 힙의 복잡도 분석
        * 힙은 삽입과 삭제 연산 시, 최악의 경우 이진 트리의 높이 해당하는 비교 연산 및 이동 연산이 필요하다.
        * 힙은 완전 이진 트리이기 때문에 힙의 높이는 logn이 되고, 시간복잡도 또한 O(logn)이 된다.
    * 힙 정렬(heap sort) (최대힙 기준 설명) 
        * 힙의 삭제 연산 수행 시, 가장 큰 값이 나오므로 이를 이용하면 정렬을 할 수 있다.
        * n개의 요소는 O(nlogn)시간 안에 정렬된다.
        * 정렬할 데이터를 힙에 넣고, 한 번에 하나씩 요소를 꺼내면서 뒤쪽부터 저장하면, 오름차순 정렬이 된다.
        * 힙 정렬은 **특히 전체 자료를 정렬하는 것이 아니라 가장 큰 값 몇 개만 필요할 떄 유용하다.**
* 머신 스케줄링(machine scheduling)
    * 동일한 기계 m개를 가지고 각각 처리 시간이 다른 작업 n개를 처리하는 최소 시간을 구하는 문제를 머신 스케줄링이라 한다. ex) 서버에 작업 분배, 
    * 최적해를 찾는 것은 상당히 어렵지만, 간단하게 근사해를 찾는 방법은 있다.
    * 그 중 한가지가 LPT(longest processing time first) 방법으로, 말 그대로 작업 소요시간이 긴 프로세스부터 분배(처리)하는 방식이다.
    * 최소힙을 사용하면 m개의 기계에 작업을 쉽게 분배할 수 있다. 여기서 최소힙은 모든 기계의 작업 종료시간을 저장하고 있다.
    * 작업을 수행하지 않은 최초 상태는 모두 0이고, 힙에서 최소 종료 시간을 갖는 기계를 삭제하여 작업을 할당한다.
    * 작업을 할당받은 기계는 종료 시간을 업데이트 하고 다시 힙에 저장된다. 이제 이 과정을 반복한다.

* 허프만 코드(Huffman codes)
    * 각 글자의 빈도를 알고 있는 메세지를 이진트리를 이용해 압축하는 방법. 이 때 이 이진트리를 허프만 코딩 트리라고 부른다.
    * 압축을 위해선 전체 데이터 양을 줄이기 위해 아스키와 같은 고정된 길이가 아닌 가변 길이의 코드를 사용한다.
    * 즉, 빈도수에 따라서 많이 등장하는 글자에는 짧은 비트열을 사용하고 잘 나오지 않는 글자에는 긴 비트열을 사용하여 전체 크기를 줄이자는 것이다.
    * 글자를 나타내는 비트열은 서로 혼동을 일으키지 않아야 한다. 
    * 문제
        1. 압축해야할 텍스트가 주어졌을 때, 텍스트를 압축해서 비트코드를 자동으로 생성하는 것
        2. 압축된 텍스트를 어떻게 복원할 것인가.
            * 해독은 **모든 코드가 다른 코드의 첫 부분이 아니라는 것**을 이용하여, 앞에서부터 1, 2, 3... 글자를 살펴보면서 테이블에서 매칭되는 글자를 찾는다.
            * 이러한 특징을 만족하는 특수한 코드를 만들기 위해 이진트리를 사용하고, 이를 허프만 코드라고 한다.
    * 허프만 코드 생성
        1. 먼저 빈도수에 따라 글자를 나열한다. 예를 들어 (s(4), i(6), m(8), t(12), e(15))가 될 것이다.
        2. 이제 빈도수가 가장 작은 2개 (s(4), i(6))를 추출하여 이들을 단말노드로 하는 이진트리를 구성한다. 루트의 값은 자식노드의 값을 합한 값이 된다.
        3. 2에서 만든 이진트리의 루트를 다시 힙에 넣고 가장 작은 2개를 뽑아 이진트리를 구성한다.
            * 여기서는 m(8)과 2번에서 s(4)와 i(6)가 합쳐저 만들어진 (10)이 새로운 이진트리를 구성한다.
            * **이진트리를 구성할 때, 작은 수가 왼쪽 서브트리가 된다.
        4. 이 과정을 모두 하나의 이진트리로 합쳐질 때 까지 반복한다. 
        5. 완성된 허프만 트리에서 왼쪽 간선은 비트 1 오른쪽 간선은 비트 0을 나타낸다.
        6. 이제 각 글자에 대한 호프만 코드는 단순히 루트 노드에서 단말 노드까지의 경로에 있는 간선의 값을 읽으면 된다.


## 10. 그래프 I
* 그래프는 객체 사이 연결 관계를 표현하는 자료구조로 대부분의 현실 문제를 나타낼 수 있다.
* 그래프는 인접 행렬이나 인접 리스트로 표현한다.
    * 정의
        * 그래프는 정점(vertex)와 간선(edge)들의 유한집합이다. 정점은 객체, 간선은 객체들간의 관계를 의미한다.
    * 용어
        * 무방향(undirected) 그래프, 방향(directed) 그래프
            * 그래프의 간선의 종류에 따른 분류. 무방향은 간선을 통해 양방향을 갈 수 있으며 (A, B)로 표현한다.
            * 방향 그래프는 <A, B>로 나타내고 간선을 통해 A->B의 방향으로 밖에 움직이 못 한다.
        * 가중치(weighted) 그래프 or Network
            * 간선에 가중치가 존재하는 그래프이다.
            * 가중치는 정점간의 연결강도 및 여러 관계를 표현한다.
        * 부분 그래프(subgraph)
            * 어떤 그래프의 일부 정점과 간선으루 이루어진 그래프
        * 정점의 차수(degree)
            * 인접 정점(adjacent vertex)이란 간선에 의해 직접 연결된 정점을 뜻한다.
            * 정점의 차수는 그 정점에 인접한 정점의 수를 말하며, 무방향 그래프에서 전체 차수의 합은 간선 수의   2배가 된다.
            * 방향 그래프에서는 차수를, 간선의 방향에 따라 진입차수(in-degree)와 진출차수(out-degree)로 나눈다.
        * 경로(path)
            * 임의의 두 정점 u와 v사이 이동할 수 있는 간선이 존재하면 그 길을 경로라고 한다.
            * 경로 중 반복되는 간선이 없을 경우 단순 경로(simple path)라고 하며, 단순 경로의 시작과 끝 정점이   같다면 이를 **사이클(cycle)**이라 한다.
        * 연결(connected) 그래프, 완전(complete) 그래프
            * 무방향 그래프에 있는 모든 정점쌍에 대해 항상 경로가 존재한다면 이를 연결 그래프라고 한다.
            * 트리는 특수한 형태의 그래프로 사이클을 갖지않는 연결 그래프이다.
            * 완전 그래프는 그래프의 모든 정점 간에 간선이 존재하는 그래프를 말한다.
            * 무방향 완전 그래프에서 정점이 n개이면, 간선의 수는 n(n-1)/2 가 된다.
    * 그래프 표현 방법
        * 인접 행렬(adjacency matrix) : 2차원 배열을 이용하여 표현
            * N x N 크기의 행렬로 표현. 간선이 존재하면 1, 존재하지 않으면 0
            * **무방향 그래프의 경우, 대칭행렬이므로 위 삼각, 또는 아래 삼각만으로 표현가능**
            * 두 정점 사이 간선이 존재하는지 빠르게 확인 가능하다.
            * 다만, 희소 그래프의 경우 메모리 낭비가 심하고, 전체 간선을 돌아보기 위해 O(N^2) 시간이 소요된다.
            * 구현
            ```c
            typedef struct GrpahType {
                int num_vertices;
                int matrix[MAX_VERTICES][MAX_VERTICES];
            } GrpahType;
            ```
        * 인접 리스트(adjacency list) : 간선 정보를 담는 여러 개의 연결 리스트와 이를 저장하는 하나의 연결  리스트로 표현
            * n(정점)개의 연결 리스트와, 이를 가리키는 n개의 헤더노드, 2m(간선)개의 노드가 필요하다.(무방향)
            * 따라소 간선이 적은 희소그래프에 적합하다.
            * 두 정점 사이 간선이 존재하는지 확인하기 위해선 정점 차수만큼의 시간이 필요하다.
            * 전체 간선을 돌아보기 위해 O(n+m) 시간이 걸린다.
            * 구현
            ```c
            typedef struct GraphNode {
                int vertex;
                struct GrpahNode *link;
            } GraphNode;

            typedef struct GraphType {
                int num_vertices;
                GraphNode *adj[MAX_VERTICES];
            } GraphType;
            ```
    * 그래프 탐색
        * 하나의 정점으로부터 시작하여 차례대로 모든 정점들을 한 번씩 방문하는 것.
        * DFS(Depth First Search), 깊이 우선 탐색
            * 시작 정점에서 출발하여 연결된 정점으로 이동한 후, 이동한 정점을 시작 정점으로 아직 방문하지 않은 정점에 대해 다시 DFS를 시작한다.
            * visited 배열을 이용해 방문한 노드를 관리하고, 새로 정해진 시작 정점에서 더 이상 갈 곳이 없는 경우, 종료하고 돌아와서 이전 시작 정점의 탐색을 계속한다.
            * 즉, DFS는 순환적인 형태를 가지고 있으며 흔히 순환적 방법 또는 스택을 이용해 구현한다.
            ```c
            // 순환적인 방법
            void dfs(GraphType *graph, int *visited, int start) {
                visited[start] = 1;
                for (GraphNode *temp = graph->adj[start]; temp != NULL; temp = temp->link) {
                    if (visited[temp->vertex]) continue;
                    dfs(graph, visited, temp->vertex);
                }
            }

            // 스택을 이용
            void dfs(GraphType *graph, int start) {
                int temp;
                int *visited = (int *)calloc(MAX_VERTICES, sizeof(int));
                Stack *stack = (Stack *)malloc(sizeof(Stack));
                visited[start] = 1;
                init(stack);
                push(stack, start);

                while (!is_empty(stack)) {
                    temp = pop(stack);
                    for (GraphNode *node = graph->adj[temp]; node != NULL; node = node->link) {
                        if (visited[node->vertex]) continue;
                        visited[node->vertex] = 1;
                        push(stack, node->vertex);
                    }
                }
                free(visited);
                free(stack);
            }
            ```
            * 두 방법 모두 인접 리스트의 경우 O(n+m), 인정 행렬은 O(n^2)의 시간이 걸린다.
        * BFS(Breadth First Search), 너비 우선 탐색
            * 너비 우선 탐색은 트리의 레벨 순회와 비슷하다. 시작 정점으로 부터 거리가 1인 정점부터 시작해 2, 3.. 순으로 탐색하는 방법이다.
            * 즉, 시작 정점으로부터 가까운 정점을 먼저 방문하고 멀리 떨어져 있는 정점을 나중에 방문한다.
            * 이를 구현하기 위해 선입선출 할 수 있는 큐(Queue) 자료구조를 사용한다.
            * 알고리즘은 큐가 소진될 때 까지, 큐에서 정점을 꺼내서 방문하고 인접 정점들을 큐에 추가한다. 
            * 구현
            ```c
            void bfs(GrpahType *graph, int start) {
                int *visited = (int *)calloc(graph->num_vertices, sizeof(int));
                visited[start] = 1;
                Queue *queue = (Queue *)malloc(sizeof(Queue));
                init(queue);
                enqueue(queue, start);

                int temp;
                GraphNode *node;
                while (!is_empty(queue)) {
                    temp = dequeue(queue);
                    for (node = grpah->adj[temp]; node; node = node->link) {
                        if (!visited[node->vertex]) {
                            visited[node->vertex] = 1;
                            enqueue(queue, node->vertex);
                        }
                    }
                }
                free(visited);
                free(queue);
            }
            ```
            * 너비 우선 탐색도 역시, 인접 행렬의 경우 O(n^2), 인접 리스트로 구현 시 O(n+m) 시간이 소요된다.


## 11. 그래프 II
* 최소 비용 신장 트리(MST, Minimum Spannig Tree)
    * 신장 트리(spanning tree)란, 그래프 내의 모든 정점과 일부 간선을 포함하는 트리이다
        * 트리이므로, 연결 그래프의 형태이고 사이클이 없다.
        * 즉, n개의 노드와 n-1개의 간선으로 이어진 형태이다.
        * 신장 트리를 쉽게 구하는 방법으론, DFS나 BFS를 수행하는 것이다.
    * 최소 비용 신장 트리란, weighted 그래프에서 신장 트리 중 간선 가중치의 합이 최소가 되는 신장트리를 의미한다.

    * MST를 구하는 두 가지 탐욕법 기반 알고리즘을 소개한다.
    * Kruskal's algorithm
        * 크루스칼 알고리즘은 처음, 각 노드를 각각의 컴포넌트로 보고 **간선을 기준으로** 가장 가중치가 낮은 간선부터 추가하며 컴포넌트를 잇는다.
        * 간선을 추가할 때, 간선을 추가함으로 인해 사이클이 생기는지 확인하고, 사이클이 생길 경우에는 추가하지 않고 버린다.
        * 즉, 크루스칼은 다음 2개의 단계로 이루어져 있다.
            1. 간선은 가중치 기준 오름차순으로 정렬한다. O(ElogE) (E: 간선 수)
            2. 1에서 정렬한 간선을 작은 것부터 하나씩 살펴보며, 사이클이 생기지 않으면 추가하고 컴포넌트를 잇는다.
        * 위의 2의 과정에선 간선을 추가함으로써 사이클이 생기는지 확인해야 한다.
            * 앞에서 배운 방법으로 BFS나, DFS를 이용하면 사이클의 존재유무를 확인할 수 있지만, 
            * O(n+m)의 시간이 걸리고, 매번 간선을 살펴볼 때마다 반복되므로 전체 시스템이 매우 느려진다.
        * 사이클의 발생 유무를 확인하기 위해, 유니온-파인드 자료구조를 사용한다.
        * **유니온-파인드(union-find)**
            * 유니온-파인드 자료구조는 서로소 집합을 관리하는 자료구조이다.
            * 크게, 집합의 대표값을 찾는 find 연산과 두 집합을 하나로 합치는 union 연산이 있다.
            * 기본 아이디어는, link 배열을 만들고 해당 인덱스에 값이 인덱스가 포함된 집합의 대표 노드를 가리키도록 한다.
            * 동시에 size 배열을 만들어 각 컴포넌트 요소의 수를 저장하고 union 연산 시, 적은 집합을 큰 집합에 붙이는 식으로 구현한다.
            * 위와 같은 방법으로 구현하면 트리의 최대 레벨이 logn이 되므로 find 연산 시 어떤 값에서도 logn 시간에 대표값을 찾을 수 있다.
            * find, union 연산은 다음과 같이 구현된다.
            ```c
            int find(int *link, int x) {
                if (link[x] == -1) return x;
                while (link[x] != -1) { // 비어있는 경우 -1이라 하면,
                    x = link[x];
                }
                return x;
            }

            void union(int a, int b) {
                a = find(a);
                b = find(b);
                if (size[a] < size[b]) {
                    int temp = a;
                    a = b;
                    b = temp;
                }
                size[a] += size[b];
                link[b] = a;
            }
            ```
            * **경로 압축**
            * 만약 find 함수를 다음과 같이 작성하면, find와 union이 상수항과 비슷한 시간에 작동한다.
            ```c
            int find(int *link, int x) {
                if (x == link[x]) return x;
                return link[x] = find(link[x]);
            }
            ```

        * 이렇게 작성한 크루스칼 알고리즘의 시간복잡도는 find와 union은 거의 상수항과 비슷하므로, 간선을 정렬하는 시간이 지배한다. O(ElogE)
    
    * Prim's algorithm
        * 프림 알고리즘은 최초 한 노드를 선택한 후, 해당 노드를 기점으로 연결된 간선 중 최소 간선을 추가해가며, 트리를 만드는 방법이다.
        * 즉, 최초 노드에서 가장 가까운 간선을 추가 후, 현재까지의 노드로 이루어진 트리에서 갈 수 있는 최소 간선을 추가하는 과정을 반복한다.
        * 대부분의 경우, 현재 트리에서 아직 트리에 추가되지 않은 다른 노드로의 최소 간선을 저장하는 배열을 만들어 관리한다.
        * 매 번 트리에 새 노드가 추가될 때, 해당 노드는 빼고, 해당 노드로 부터 생기는 최단 경로를 업데이트 한다.
        * N개의 노드를 둘러보고 추가하는 과정에서, 또 N 크기에 거리 배열을 업데이트 하므로 O(N^2)의 시간복잡도를 가진다.
    * 크루스칼 알고리즘과 비교해서, 프림은 **노드를 중심**으로 만드는 기법이고 전자는 **간선을 중심**으로 MST를 만드는 기법이이다.
    * 시간복잡도를 비교하면 kruskal은 O(ElogE), Prim은 O(N^2)으로 간선이 적은 희소그래프에서는 크루스칼이, 밀집 그래프에서는 프림 알고리즘이 유리하다.

* 최단 경로(shortest path)
* 최단 경로 문제는, 가중치 네트워크에서 임의의 두 정점 i와 j를 연결하는 경로 중 간선들의 가중치 합이 최소가 되는 경롤 찾는 문제를 말한다.
* 최단경로를 찾는 알고리즘은 벨만-포드, 다익스트라, 플로이드-워셜 크게 세가지가 있다.


* 벨만-포드(Bellman-Ford) Algorithm
* 이는 책에 내용에는 없는 부분이다.
    * 벨만-포드 알고리즘은, 시작 정점으로부터 다른 모든 노드로의 최단 경로를 찾는 알고리즘이다.
        1. 최초, 시작 정점을 제외한 다른 노드로의 거리를 INF로 초기화한다.
        2. n-1번의 라운드 동안 모든 간선을 살펴보며, 경로를 줄여나간다.
    * 벨만-포드 알고리즘은 사실 매우 단순하게 진행된다.
    * 최초 다른 노드로의 거리를 INF로 초기화 한 후, 라운드마다 시작 정점의 인접한 정점까지의 최단거리 => 그리고 그 인접한 정점의 최단거리... 순으로 구해나간다.
    * n-1번의 라운드는 시작노드와 마지막 노드 사이 n-2개의 노드가 존재할 수 있기 때문이다.
    * 대부분의 경우에서는 n-1번의 라운드가 모두 진행되기 전에 결과과 나오므로, 이를 이용해 최적화 할 수 있다.
    * 한 번의 라운드 동안, 전체 경로에 대해 변화가 없다면, 알고리즘을 바로 종료해도 좋다.
    * **벨만-포드는 음수 사이클이 그래프에 존재할 경우 음수 사이클을 찾을 수 있으며, 해당 경우엔 최단 경로가 존재하지 않는다.**
        * 음수 사이클을 포함한 경로는 무한히 작아질 수 있다.
    * **음수 사이클을 찾기 위해선 기존 n-1번의 라운드를 진행 후, 1번의 라운드를 더 실행한다.**
    * 즉, n번째 라운드에서도 줄어드는 경로가 존대한다면, 해당 그래프는 음수 사이클을 포함한다.
    * **벨만-포드는 '음수 간선'을 포함하는 최단 경로도** 또한 찾을 수 있다.
    ```c
    // edges에 간선 리스트로, 모든 간선 정보(a, b, w)가 포함되어 있다고 하자.
    int distance[MAX_VERTICES] = { INF, };
    distnace[start] = 0;
    for (int k=0; k < N-1; k++) {
        for (int i=0; i < E; i++) {
            a = edges[i][0];
            b = edges[i][1];
            w = edges[i][2];
            if (distance[b] > distance[a] + w) {
                distance[b] = distance[a] + w;
            }
        }
    }
    ```
    * 벨만-포드의 시간복잡도는 O(nm)가 된다. (n은 노드, m은 간선의 수)

* 다익스트라(Dijkstra) Algorithm
    * 다익스트라도 벨만-포드와 같이 한 시작 정점에서 다른 모든 노드로의 최단 경로를 구하는 알고리즘이다.
    * 마찬가지로 distance 배열을 만들어 시작정점을 제외한 모든 다른 정점으로의 거리를 INF로 초기화한다. (시작 정점은 0)
    * 다익스트라는 좀 더 효율적인 방법으로 최단 경로를 찾는데, 다만 **그래프에 음수 간선이 존재해선 안 된다.**
    * 다익스트라는 다음과 같이 동작한다.
        1. 시작노드에서 이어진 간선을 **우선순위 큐**에 넣는다. 이를 통해 시작 노드로부터 가장 가까운 간선을 선택한다.
        2. 우선순위 큐에서 간선을 뺴내서 선택한다. 선택한 노드에서 시작하는 간선을 포함한 경로를 우선순위 큐에 추가한다.
            * 이 때, 우선순위 큐에 추가하는 거리는 시작 노드로부터의 거리로 한다.
        3. 우선순위 큐가 빌 때까지, 이 과정을 반복한다.
    * 즉, 다익스트라는 매 번 시작 노드로부터 가장 가까운 노드를 선택하며 최단경로를 구하는 알고리즘이다.
    * 이 과정에서 이전에 선택된 노드에 대한 최단 경로는 절대 다시 바뀌지 않는다.
    * 다익스트라가 옳게 동작하는 이유
        1. 매 번 우선순위 큐에서 가장 가까운 노드를 꺼낸다. 이 노드에 대한 경로는 여러가지 있을 수 있다.
        2. 다른 경로로는 또 다른 제2, 제3의 노드를 경유해 돌아오는 경로가 있는데, 
        3. 우선순위큐에서 뺸 이 노드는 아직 추가되지 않은 노드 중 '가장 가까운' 경로를 가진 노드이므로
        4. 다른 곳을 경유해서 오면, 오히려 더 먼 경로가 된다.
    ```c
    //dijkstra algorithm by python
    distance = [INF] * (N+1)    // 거리 정보
    visited = [False] * (N+1)   // processed, 처리되었는지를 저장
    distance[start] = 0

    pq = [] # priority queue
    heappush(pq, (0, start))
    while pq:
        w, now = heappop(pq);
        if visited[now]:
            continue
        visited[now] = True
        for weight, next in adj[now]: # 인접 리스트 형태로 저장
            if distance[next] > distance[now] + weight: // 새 경로가 더 작으면
                distnace[next] = distance[now] + weight // 파이썬은 기본적으로 최소힙을 지원한다.
                heappush(pq, (distance[next], next))    // 거리 바꾸고, 힙에 다시 저장.
    ```
    * 다익스트라 알고리즘의 시간복잡도는 O(N+MlogM)이다. (N : 노드, M : 간선의 수)


* 플로이드-워셜(Floyd-Warshall) Algorithm
    * 플로이드-워셜은 모든 간선에서 다른 모든 간선으로의 최단 경로를 한 번에 구한다.
    * n x n 행렬을 사용하고, 인접행렬의 값은 간선의 가중치를 나타내며, 간선이 존재하지 않을 경우 무한대로 초기화한다.
    * 기본적인 아이디어는, 매 번 라운드마다 중간 경로를 바꿔가며 최단경로를 업데이트 하는 것이다.
    ```c
    for (int k=1; k <= n; k++) {             // 중간 노드
        for (int i=1; i <= n; i++) {         // 시작 노드
            for (int j=1; j <= n; j++) {     // 도착 노드
                // 간선 정보를 인접행렬 형태로 저장
                adj_matrix[i][j] = min(adj_matrix[i][j], adj_matrix[i][k] + adj_matrix[k][j]);
            }
        }
    }
    ```
    * 이 알고리즘은 O(n^3) 시간에 동작한다.
    * 플로이드-워셜에서 음수 사이클이 포함되어 있는지 확인하기 위해선, dist[i][i]가 0이 아닌지 살펴보면 된다. 모두 0인 경우 음수 사이클은 존재하지 않는다.

* 위상 정렬(topological sort)
    * 그래프에 간선이 선행 순서를 나타낼 때, <u, v>가 있다면 정점 u는 정점 v를 선행한다고 말한다.
    * 이 때 방향 그래프에 존재하는 각 정점들의 선행 순서를 위배하지 않으면서 모든 정점을 순서대로 나열하는 것을 위상정렬이라고 한다.
    * **위상 정렬을 사용하기 위해선, 그래프 내의 사이클이 존재해선 안 된다.**
        * 사이클이 존재하면, 사이클 내 그 어떤 정점도 다른 정점 보다 앞설 수 없다.
    * 방법
        1. 그래프에 간선 정보를 바탕으로, 새로운 배열 in_degree에 각 정점에 진입차수를 저장한다.
        2. 진입차수가 0인 정점들을 스택에 넣고(큐도 상관 없다.) 하나씩 정점을 빼가며, 선택된 정점과 인접한 정점의 in_degree 값을 1씩 감소시킨다.
        3. 다시 진입차수가 0이 된 정점들을 스택에 넣고, 위와 같은 과정을 스택이 빌 때 까지 반복한다.
    * 위상정렬의 포인트는 **진입차수가 0인 정점이 여러개 일 경우, 어떤 정점을 선택해도 상관 없다는 것이다.**
        * 하나의 그래프는 여러 위상순서(위상정렬의 결과)가 있을 수 있다.
    * 만일 위상정렬 과정 중에, 모든 정점을 뽑지 못하고 진입차수가 0인 정점이 남아있지 않은 상태가 된다면, 해당 그래프의 위상정렬은 존재하지 않는다.
        * 이러한 그래프는 사이클이 존재한다.


## 12. 정렬
* 정렬은 데이터를 특정한 기준으로 오름차순, 또는 내림차순으로 나열하는 방법이다.
* 어느 상황에서나 최선희 효율을 보여주는 정렬 방법은 없다.
* **안정성** : 같은 값을 가진 데이터가, 정렬 후에도 같은 순서를 유지하는지
* **제자리 정렬(in-place sorting)** : 따로 배열을 만들지 않고, 입력 배열 안에서 정렬이 일어난다.
    * 선택 정렬(selection sort)
        * 선택 정렬은 배열을 순회하며, 가장 작은 수를 앞에부터 채워넣는 방식이다.
            1. 배열을 순회하여 가장 작은 수를 찾는다. 찾은 수와 첫 번째 자리 수와 교환한다.
            2. 0번째를 제외한 가장 작은 수를 찾는다. 찾은 수와 두 번째 자리 수와 교환한다.
            3. 위의 과정을 n-1번 반복한다. n-1번의 라운드 수행 후, n번째는 이미 정렬되어 있다.
        * 즉, 선택 정렬은 앞에부터 **자리**를 기준으로 작은 수를 선택하여 집어넣는 방식이다.
        * 비교연산이 n-1, n-2, n-3, .., 1 = (n-1)*(n-2)/2 = O(n^2) 이고, 
        * 교환연산이 매 라운드 한 번씩, 총 n-1번으로 이동연산은 3*(n-1)번이다.
        * 장점 : 이동연산의 횟수가 미리 결정 된다. 제자리 정렬(in-place sorting)
        * 단점 : 이동 횟수가 큼. **안정성 x**
            * 안정성을 만족하지 않는 예
            ```c
            B, b, a, c      (a < b < c, b=B일때)
            결과는 a, b, B, c가 된다.
            ```

    * **삽입 정렬(insertion sort)**
        * 손 안에 카드를 정렬하는 방식. 
            1. 배열을 정렬된 부분(왼쪽)과 정렬되지 않은 부분(그 외 부분)으로 나눈다.
            2. 배열의 인덱스 1번째 원소부터 돌아보면서, 앞에 정렬된 부분에 올바른 위치를 찾아 끼어넣는다.
            3. 올바른 위치를 찾는 과정에서, 앞에 원소가 현재 정렬중인 원소보다 클 경우 해당 앞에 원소를 한 칸 당겨서 위치를 조정한다.
        * 삽입 정렬은 선택 정렬과는 반대로 자리 기준이 아닌, **원소** 기준으로 정렬하는 방법이다.
        * 삽입 정렬은 비교 연산의 횟수가, 정렬 시 딱 필요한 만큼만 진행된다.
        * 장점 : 정렬된 자료에서 O(n)으로 가장 빠르다, 제자리 정렬, 안정성
        * 단점 : 입력자료가 역순일 경우 O(n^2)의 시간복잡도, 이동연산이 많다.

    * 버블 정렬(bubble sort)
        * 인접한 2개의 레코드를 비교해서 크기가 순서대로 되어 있지 않으면, 서로 교환하는 비교-교환 과정을 통한 정렬 방식
        * 왼쪽부터 오른쪽으로 나아가며 비교-교환하고, 전체적으로는 오른쪽(가장 큰 원소)부터 하나씩 정렬되는 방식이다.
        * 문제점 : 순서에 맞지 않은 요소를 인접한 요소와 교환한다. 특히 최종 정렬위치에 있는 경우라도 교환이 일어날 수 있다. 

    * 쉘 정렬(shell sort)
        * 확장된 삽입 정렬으로써, 초기 n/2만큼의 Gap을 두고 부분 배열을 만들어 삽입 정렬을 실시한다.
        * 그 후 Gap이 1보다 작아질 때 까지, Gap = Gap/2를 수행하며 매번 만들어지는 부분 배열에 삽입 정렬을 수행한다.
        * 일반적인 삽입 정렬은 한번에 한칸씩 밖에 움직이지 못 하는데에 비해, 
        * 쉘 정렬은 여러 칸을 건너뛸 수 있으므로 교환되는 요소들이 최종 위치에 더 가까이 있을 확률이 높아진다.
        * 최악의 경우 O(n^2)이지만, 평균적으로 O(n^1.5)의 시간복잡도를 보인다.

* 이 아래는 훨씬 더 효율적이고, 입력 데이터의 크기가 클 때도 사용할 수 있는 방법이다.
    * **합병 정렬(merge sort)**
        * 합병 정렬은 분할 정복(divide and conquer) 기법에 바탕을 두고있다.
            * 분할 정복: 큰 문제를 여러 개의 작은 문제로 분리하고 각각을 해결한 다음, 결과를 모아 원래의 큰 문제를 해결하는 방법
        * 합병 정렬은 순환적인 형태로 작성되고, 배열을 한 개의 요소까지 나눈 후 합병(merge)하며 정렬한다.
        * 합병에는 합병 결과를 저장할 추가적인 리스트가 필요하다. 
            1. 2개의 리스트 요소들을 처음부터 비교하여, 더 작은 원소를 새 리스트에 저장한다.
            2. 저장된 원소가 존재했던 리스트의 인덱스를 증가시키고, 다시 비교해서 더 작은 원소를 새 리스트에 저장한다.
            3. 위의 과정을 둘 중 한 리스트가 빌 때 까지 반복하고, 남은 리스트의 요소들을 전부 새 리스트에 복사한다.
        * 합병 정렬은 매 번 2개의 부분 집합으로 문제를 분할하므로 log(n)의 깊이를 갖으며,
        * 한 번의 merge 단계는 최대 n번의 비교 연산과 2n번의 이동연산이 필요하다.
        * 따라서 합병 정렬의 시간복잡도는 O(nlogn)이 된다.
        * 장점 : 안정적, O(nlogn)으로 빠른 정렬, 입력 데이터에 상관없이 정렬시간이 동일하다.
        * 단점 : 임시 배열 필요, 이동횟수가 많아 레코드의 크기가 크면 불리함.
        * 다만, 연결 리스트로 레코드를 구성하는 경우 링크만 변경되므로 다른 어떤 정렬보다 효율적일 수 있다.
        
    * **퀵 정렬(quick sort)**
        * 퀵 정렬은 합병 정렬과 같은 분할-정복(divide and conquer) 기법에 기반한 정렬 방식이다.
        * 합병 정렬과의 차이는, 합병 정렬은 mid를 구해 균등 분할하지만, 퀵 정렬은 피봇(pivot)을 두고 비균등 분할을 한다는 것이다.
        * 피봇(pivot)이란, 퀵 정렬에서 정렬의 기준으로 임의 선택을 받은 수를 의미한다.
            * pivot을 선택하는 방법으론 가장 간단하게 왼쪽 원소를 선택할 수도 있고
            * 일반적으로는 왼쪽 오른쪽 중 간 3개의 데이터 중 중간 값을 선택하는 median of three를 많이 쓴다.
        * 구현
        ```c
        int partition(int list[], int left, int right) {
            int row, high, temp;
            int pivot = list[left];
            row = left;
            high = right + 1;
            do {
                do {
                    row++;
                } while (list[row] < pivot);
                do {
                    high--;
                } while (list[high] > pivot);
                if (row < high)
                    SWAP(list[row], list[high], temp);
            } while (row < high);
            SWAP(list[left], list[high], temp);
            return high;
        }

        void quick_sort(int list[], int left, int right) {
            if (left < right) {
                int q = partition(list, left, right);
                quick_sort(list, left, q-1);
                quick_sort(list, q+1, right);
            }
        }
        ```
        * partition 함수는 left - right 범위에서 가장 왼쪽 요소를 pivot으로 정하고, pivot을 기준으로 정렬하고 피봇의 정렬된 위치를 반환한다.
        * partition 함수의 이해
            1. 편의를 위해 가장 왼쪽에 요소를 피봇으로 설정한다.
            2. 피봇 왼쪽의 요소를 row, 오른쪽 끝을 high로 설정하고 row와 high가 엇갈릴 때 까지 row는 증가 high는 1씩 감소시킨다.
            3. 증가/감소 과정에서 row는 피봇보다 큰 원소, high는 피봇보다 작은 원소를 만나면 멈춘다.
            4. row와 high가 멈추면 두개의 값을 교환한다.
            5. 위에 과정을 거쳐 row와 high가 엇갈리게 되면, high와 피봇을 교환한다.
            6. 이로써 피봇의 왼쪽에는 피봇보다 작은 요소만이, 오른쪽에는 피봇보다 큰 요소만이 남는다.
        * partition 과정을 통해 절반씩 대충 정렬하고, q를 기준으로 왼쪽 오른쪽 부분을 분할해서 quick_sort 한다.
        * quick 정렬의 시간복잡도는 평균 O(nlogn)이다. 일반적으로 퀵정렬은 합병, 힙 정렬에 비해 빠르다.
        * 하지만, 대상이 이미 정렬되어 있을 때와 같은 특수한 경우 퀵정렬은 최악의 O(n^2) 시간복잡도를 갖는다.
        * 퀵정렬은 속도가 빠르고 제자리 정렬이지만, 불안정 정렬이기도 하다.
        
    * **힙 정렬(heap sort)**
        * 힙 정렬은 단순하다. 최소힙에 데이터를 저장했다가 순서대로 빼며 저장한다.
        * 힙 자료구조는 데이터의 삽입이나 삭제에 O(logn) 시간이 들기 때문에, 힙정렬의 시간복잡도는 O(nlogn)이다.
        * 힙은 배열로 간단하게 구현이 가능하다. 힙 정렬은 불안정 정렬이다.

    * **기수 정렬(radix sort)**
        * 기수 정렬은 버킷(bucket)을 사용해서, **비교 없이** 정렬하는 방법이다.
        * 비교 기반의 정렬은 O(nlogn)의 하한을 깨뜨리지 못 한다. 기수정렬은 O(kn)(k는 보통 0~9사이 상수)의 시간복잡도를 가진다.
        * 기수 정렬은 추가적인 메모리를 필요로 하고, 정렬할 수 있는 레코드 타입이 한정된다.
            * 기수(radix)란 숫자의 자리수를 의미
            * 레코드의 키들이 동일한 길이를 갖는자나 문자열로 구성되야 함
        * 숫자를 비교 시, 0~9 까지의 10개의 버킷을 사용해서 낮은 자릿수부터 기준으로 버킷에 넣었다가 순서대로 뺀다.
        * 즉, 정렬 과정에서 정렬할 대상을 비교하지 않는다.
        * LSD(Least significant digit), MSD(Most significant digit)
            * 각각 가장 낮은 자리수, 가장 높은 자리수를 의미
        * 각 버킷의 들어간 숫자들은 들어간 순서대로 나와야 한다. 즉, 버킷은 큐(Queue)로 만들어진다.
        * 기수 정렬은 매우 빨라 인기 있지만, 특정 상황에서만 사용이 가능하다.

    * 계수 정렬(counting sort)
        * 기수 정렬은 사실 계수 정렬을 좀 더 효율적이고 일반화 한 정렬이다.
        * 계수 정렬은 한 값당 버킷을 하나씩 부여하고, 각 수의 갯수를 센 후 그 정보를 바탕으로 배열을 만든다.
        * 즉, O(n) 시간복잡도를 갖는다.
        * 하지만 계수 정렬은 수의 범위에 해당하는 만큼의 배열 크기를 가져야 하므로, 많이 사용되지 못 한다.
    
    * 정렬의 비교
        * 안정 정렬 : 삽입, 버블, 합병, 기수
        * 불안정 정렬 : 선택, 쉘, 퀵, 힙
        
        * 속도
        * 알고리즘    최선    평균    최악
        * 삽입        O(n)    O(n^2)  O(n^2)
        * 선택        O(n^2)  O(n^2)  O(n^2)
        * 버블        O(n^2)  O(n^2)  O(n^2)
        * 쉘          O(n^1.5)O(n^1.5)O(n^2)
        * 퀵          O(nlogn)O(nlogn)O(n^2)
        * 합병        O(nlogn)O(nlogn)O(nlogn)
        * 힙          O(nlogn)O(nlogn)O(nlogn)
        * 기수        O(kn)   O(kn)   O(kn)

        
## 13. 탐색
* 탐색은 컴퓨터 시스템에서 가장 많이 수행되는 연산이다. 효율적인 탐색은 언제나 중요하다.
* 정렬되지 않은 배열에서의 탐색 - 사실상 쓰지않는다.
    * 순차 탐색(sequential search)
        * 배열을 0~n-1까지 돌아보며 key값과 비교. 가장 원시적인 형태
    * 개선된 순차 탐색
        * 배열에 마지막에 찾길 원하는 key값을 추가하고, 순차탐색과 같이 key값과 비교한다.
        * 다만, for문에서 i가 범위를 벗어나는지 비교연산을 수행하지 않아 조금 더 효율적이다.
        ```
        int i;
        list[MAX_SIZE] = key;
        for (i=0; list[i] != key; i++)
            ;
        if (i == MAX_SIZE) return -1;
        else return i
        ```
* 정렬된 배열에서의 탐색
    * **이진 탐색(binary search)**
        * 이진 탐색의 기본 아이디어는 중간값 비교이다. 즉, key값과 중간값을 비교 후 대소관계에 따라 key값의 왼쪽/오른쪽을 선택해서 탐색을 시작한다.
        * **매 단계마다 검색해야할 부분의 크기가 반으로 줄어든다** O(logn)
        * 반복문으로 구현이 효율적이다.
        ```c
        int binary_search(int list[], int n, int key) {
            int left, right, mid;
            left = 0;
            right = n;
            while (left <= right) {
                mid = (left + right) / 2;
                if (key == list[mid]) return mid;
                else if (key < list[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }
            }
            return -1;  // 탐색 실패
        }
        ```
    
    * **색인 순차 탐색(indexed sequential search)**
        * 주 자료 리스트(원래 리스트)에 일정 간격을 발췌해서, 인덱스라 불리는 테이블을 만든다.
        * 인덱스 테이블에서 index[i] <= key < index[i+1] 을 만족하는 항목을 찾는다.
            * 인덱스 테이블의 원소는 원래 테이블의 value와 index를 저장하는 두 개의 필드를 갖는 구조체.
        * 주 자료 리스트에 해당하는 범위. (index[i].index ~ index[i+1].index)에서 순차탐색을 통해 키를 찾는다.
        * 전체 테이블의 데이터 수가 매우 커지게 되면, 1차 인덱스 테이블을 발췌한 2차 인덱스 테이블.. 3차 인덱스 테이블 등을 추가할 수 있다.
        * 알려진 시간복잡도는 O(m+n/m) (m:인덱스 테이블의 크기, n: 주 자료 리스트의 크기)
 
    * 보간 탐색(interpolation search)
        * 이진 탐색에 가중치를 두는, 불균등 분할 탐색 기법이다.
        * 기본 아이디어는 정렬된 배열에서의 값과 인덱스가 비례한다고 가정하고, 비례식을 이용해 탐색할 위치를 정한다.
        * 어떤 배열의 초기 상태가 v0 ... vk(key값) ... vn이고, 각 자리의 인덱스가 0 ... k ... n이라 할 때,
        * 'vn - v0 : vk - v0 = n-0 : k-0' 비례식을 이용해 다음에 탐색할 위치를 결정한다.

    * **이진 탐색 트리(binary search tree)**
        * 값의 삽입과 삭제가 많이 일어날 때에는, 정렬된 배열을 이용하는 이진트리는 적합 o하지 않다.
            * 정렬된 배열은 삽입과 삭제의 O(n) 시간복잡도를 갖는다. 
        * 이진 탐색 트리는 삽입과 삭제가 비교적 빠른 시간(O(logn))안에 이뤄 지기 때문에 효율적으로 값을 유지할 수 있다.
        * 평균적인 경우 탐색과 삽입 삭제 연산 모두 O(logn)의 시간복잡도를 갖는다.
        * 다만, **균형트리가 아닐 경우, 특히 최악의 경우 경사 트리일 경우 O(N)의 시간복잡도를 갖는다.**
        * 따라서 **이진탐색트리에서 균형을 유지하는 것이 무엇보다 중요하다.**
    
    * AVL트리
        * 노드에서 왼쪽 서브트리와 오른쪽 서브트리의 높이 차이가 1 이하인 이진탐색트리를 의미한다.
        * AVL트리는 모든 노드의 balance factor +-1 이하인 트리이다. 
            * 균형 인수(balance factor): 왼쪽 서브트리 높이 - 오른쪽 서브트리 높이
        * 탐색연산 - 일반적인 이진 탐색 트리와 동일하다.
        * AVL트리의 삽입과 삭제 연산은 균형 상태가 깨질 수 있는 연산이다.
        * 삽입 연산
            * 삽입 연산시에는 삽입되는 위치에서 루트까지의 경로에 있는 조상 노드들의 balance factor에 영향을 줄 수 있다.
            * 따라서 삽입 연산 후에 불균형 상태가 된 가장 가까운 조상노드의 서브 트리들에 대해 다시 균형을 잡아야 한다.
                * 그 외 노드들은 이레 변경할 필요가 없다. 
            * AVL트리는 '회전'을 통해 이를 해결한다.
        * 회전
            * AVL트리에 삽입 연산에서 균형이 깨지는 경우는 총 4가지의 경우가 있다.
            * 이 떄는 트리를 부분적으로 회전시켜 균형을 맞춘다.
            1. LL: 현재 root(의미상) 왼쪽 자식의 왼쪽에 노드가 추가되면서 발생한다. 
                * root를 기준으로 root가 parent, 왼쪽 자식이 child가 되서 '오른쪽 회전'을 수행한다.
                ```c
                AVLNode* rotate_right(AVLNode *parent) {
                    AVLNode *child = parent->left;
                    // 원래 child의 오른쪽 서브트리는 parent의 왼쪽 서브트리가 된다.
                    parent->left = child->right;    
                    child->right = parent;
                    return child;   // 새로운 루트를 반환
                }
                ```
            2. LR: 현재 root 왼쪽 자식의 오른쪽에 노드가 추가되면서 발생한다.
                * **root의 왼쪽 자식을 기준으로 '왼쪽 회전'**
                * **그 후, root를 기준으로 '오른쪽 회전'을 수행한다.**
            3. RR: 현재 root 오른쪽 자식의 오른쪽에 노드가 추가되면서 발생한다. 
                * root를 기준으로 왼쪽 회전을 수행한다.
            4. RL: 현재 root 오른쪽 자식의 왼쪽에 노드가 추가되면서 발생한다.

        * balance factor를 구하는 함수
            * 균형 인자(balance factor)는 왼쪽 서브트리의 높이 - 오른쪽 서브트리의 높이로 정의했다.
            * 이는 높이를 구하는 함수 get_height()를 이용해 쉽게 구현이 가능하다.
            * get_height()는 다음과 같이 구현한다.
            ```c
            int get_height(AVLNode *node) {
                int height = 0;
                if (node != NULL) {
                    height = 1 + max(get_height(node->left), get_height(node->right));
                }
                return height;
            }
            ```
            * 그러면 balance factor를 구하는 get_balance() 함수는 다음과 같이 작성할 수 있다.
            ```c
            int get_balance(AVLNode *node) {
                if (node == NULL) return 0;
                return get_height(node->left) - get_height(node->right);
            }
            ```
        
        * 삭제 연산
            * 이진 트리와 마찬가지로 탐색을 먼저 수행 후, 삭제할 노드를 찾는다.
            * 삭제할 노드의 종류에 따라 연산이 달라진다.
                1. 단말노드
                    * 단말노드의 경우 free로 해제하고 return NULL을 돌려준다.
                2. 한쪽 서브트리가 존재
                    * 현재노드를 free 할당 해제하고 서브트리를 반환한다.
                3. 양쪽 서브트리 존재
                    * 후계자 노드(왼쪽 서브트리 가장 큰값 or 오른쪽 서브트리 가장 작은 값)를 찾는다.
                    * 후계자 노드와 값을 바꾸고, 해당하는 측의 서브트리에서 후계자 노드 삭제
                    * 따로 반환하지 않는다.
            * 현재 node에 대한 balance factor를 구한다.
                1. balance > 1 && get_balance(node->left) >= 0
                    * LL의 경우로 rotate_right(node)를 반환한다.
                2. balance > 1 && get_balance(node->left) < 0
                    * LR의 경우로 node->left = rotate_left(node->left) 이후
                    * rotate_right(node)를 반환한다.
                3. balance < -1 && get_balance(node->right) <= 0
                    * RR의 경우로 rotate_left(node)를 반환한다.
                4. balance < -1 && get_balance(node->rightt) > 0
                    * RL의 경우로 node->right = rotate_right(node->right) 이후
                    * rotate_left(node)를 반환한다.
            * 마지막으로 위의 4경우에 모두 들지 않을 경우 현재 node를 반환한다.

    * 2-3 트리
        * 2-3 트리란 차수가 2 또는 3인 노드를 갖는 트리를 말한다.
        * 2-3 트리에서 각 2-노드는 이진 트리에서의 노드와 같이 한개의 값과 총 두개의 자신보다 작은/큰 서브트리를 가진다.
        * 2-3 트리에 3-노드는 두 개의 값의 가지고, 차례로 min보다 작은 서브트리, min < ... < max인 서브트리, max보다 큰 서브트리, 총 3개의 서브트리를 갖는다.
        * 탐색연산
            * 2-3트리의 탐색연산은 이진탐색트리에 탐색연산에 3-노드를 추가하면 된다.
            * 즉 TreeNode 구조체에 자신이 2-노드인지, 3-노드인지를 저장하는 type 필드를 추가하고,
            * 이를 통해 비교하여 아래와 같이 구현할 수 있다.
            ```c
            int search(TreeNode *node, int key) {
                if (node == NULL) return False;
                else if (node->key1 == key) return True;
                else if (node->type == TWO_NODE) {  // 2-노드
                    if (node->key1 > key) 
                        return search(node->left, key);
                    else 
                        return search(node->right, key);
                } else {    // 3-노드
                    if (node->key1 > key)
                        return search(node->left, key);
                    else if (node->key2 < key) 
                        return search(node->right, key);
                    else
                        return search(node->middle, key);
                }
            }
            ```
        * 2-3트리는 삽입과 삭제 연산이 AVL트리보다 간편하다
            * 삽입연산
                * 2-3트리에 데이터 추가시, 노드에 추가할 수 있을 때 까지(2개까지) 추가되고, 더 이상 저장이 불가능하면 노드를 분리한다.
                * 노드의 분리는 중간값을 한 레벨위로 올리고 제일 작은 값을 왼쪽 노드로, 큰 값을 오른쪽 노드로 만든다.
                    1. 단말노드 분리
                        * 부모노드가 2-노드인 경우 middle이 부모노드로 올라가고 각각은 좌우 노드가 된다.
                        * 부모노드가 3-노드인 경우 middle이 올라가고, 부모노드에서 분리과정이 되풀이 된다.
                        * 즉, 부모노드가 3-노드인 경우, 되돌아가며 통합-분리의 과정이 일어난다.
                    2. 비단말 노드 분리
                        * 마찬가지로 middle을 다시 부모로 올리고 작은 값과 큰값을 별개의 노드(서브트리)로 분리한다.
                        * 마찬가지로 부모노드가 꽉차면, 부모노드에서도 분리과정이 일어난다.
                    3. 루트 노드 분리
                        * 루트 노드를 분리하게 되면 새로운 노드가 하나 생기면서 **트리의 높이가 하나 증가한다.**
                        * 새로 만들어지는 노드(middle)은 새로운 루트 노드가 된다. 
                        * 2-3 트리에서 트리의 높이가 증가하게 되는 것은, 이 경우 뿐이다.
    
    * 2-3-4 트리
        * 2-3-4 트리는 하나의 노드가 4개의 자식까지 가질 수 있도록 2-3 트리를 확장한 것이다.
        * 2-3 트리의 룰 대로 4-노드는 3개의 데이터를 가지며, key1보다 작은, key1<...<key2, key2<...<key3, key3보다 큰, 총 4개의 서브트리를 갖는다.
        * 탐색연산도 마찬가지로 위의 2-3트리에 탐색연산에 type == FOUR_NODE일 때를 추가한다.
        * 삽입 연산
            * 삽입 연산은 2-3 트리와 조금 다르다. 2-3 트리는 삽입 또는 삭제를 위한 순회(루트->단말)와 분할광 합병을 위한 순회(단말->루트)가 필요했다.
            * 2-3-4 트리는 이런 후진 분할(backward split)을 미연에 방지하기 위해, (루트->단말) 과정에서 4-노드를 만나면 무조건 분할시킨다. 
            * 따라서 단말 노드에 도달하게 되면 단말노드의 부모 노드는 4-노드가 아니라는 것이 보장된다.
    
                    
## 14. 해싱
* 해싱은 key에 산술적인 연산(hash 함수)을 적용하여, 항목이 저장되어 있는 테이블의 인덱스를 계산한 후 항목에 접근한다.
* 해시 테이블(hash table)은 key에 대한 연산의 결과로 직접 접근 가능한 구조를 의미. 해싱(hasing)은 해시 테이블을 이용한 탐색을 의미
* 사전(dictionary) 자료형 구현에 사용되며, map이나 hash, table 등으로 불리기도 한다.
* 컴파일러의 심볼 테이블, 데이터베이스 인덱싱, 검색 엔진 등에서 사용된다.
* Dictionary (사전 ADT)
    * 사전은 (키, 값) 쌍의 집합. 키와 그 키에 관련된 값을 동시에 저장하는 자료구조.
        * key: 사전의 단어와 같이 항목을 다른 항목과 유일하게 구별시켜주는 것.
        * value: 단어의 설명에 해당. 키와 연관시켜 저장하고 싶은 값
    * 항목이 저장되는 위치와는 관련없이, 사전의 모든 항목들은 '키'에 의해 식별/관리된다.
        * 사전 자료형의 정렬여부는 구현 시 결정해야할 선택적 요소.
    * 사전의 대표적인 연산
        * add(key, value): (키,값) 쌍을 추가
        * delete(key): key에 해당하는 (키, 값) 쌍을 찾아 삭제. 관련된 value를 반환. 탐색 실패 시 null 반환.
        * search(key): key에 해당하는 value를 찾아 반환. 탐색 실패 시 null 반환.
* 해싱(hashing)
    * 해시 테이블은 간단하게 배열로 구현한다. 인덱스를 알고 있다면 배열에 자료를 삽입/삭제하는 연산은 매우 빠르다.
    * 탐색 키는 문자열/매우 큰 숫자가 될수도 있으므로 탐색 키를 작은 정수로 사상(mapping)시키는 '해시 함수'가 필요하다
    * **즉, 해싱이란 키를 해시함수에 적용해 해시 주소를 생성하고, 이를 인덱스로 써서 해시 테이블에 접근하는 것.**
    * 해시 테이블
        * 해시 테이블은 M개의 버킷(bucket)으로 이루어진다.
        * **하나의 버킷은 s개의 슬롯(slot)을 가질 수 있으며, 하나의 슬롯에는 하나의 항목이 저장된다.**
            * 서로다른 두 개의 키가 해시함수에 의해 동일한 해시주소로 변환될 수 있음.
            * 슬롯을 여러개 두어서 여러개의 항목을 동일한 버킷에 저장한다. (사실 대부분은 하나의 버킷에 하나의 슬롯만 가짐)
    * **충돌(Collision)**
        * 서로 다른 두 개의 키를 해시함수에 적용시켰을 때, 같은 결과가 나오는 경우.
            * 이러한 키 k1, k2를 동의어(synonym)이라 한다.
        * 충돌이 자주 발생하면 버킷 내부에서 탐색 시간이 길어져서 탐색 성능이 저하될 수 있다.
        * 충돌이 버킷에 할당된 슬롯 수보다 많이 발생하면, 더 이상 항목을 저장할 수 없어 오버플로우가 발생한다.
    * 실제로는 해시 테이블의 크기는 제한적이기 때문에, 모든 키에 대해 공간을 할당할 수 없다. 보통 키는 매우 많고 테이블은 제한적.
    * **실제 해싱에서는 충돌과 오버플로우가 빈번하게 발생 => 시간복잡도는 O(1)보다 떨어지게 된다.**
    * 해시 함수 (hash function)
        * 키 값을 해시 테이블 주소로 변환하는 함수
        * 좋은 해시 함수으 조건
            1. 충돌이 적어야 한다.
            2. 해시함수 값이 해시테이블의 주소 영역 내에서 고르게 분포되어야 한다.
            3. 계산이 빨라야 한다.
        
        * 제산 함수
            * 나머지 연산이용. **키를 해시 테이블의 크기로 나눈 나머지를 해시주소로 사용**
            * **해시 테이블 크기 'M'은 소수(prime number)로 선택.**
                * 소수가 아닌 짝수/홀수의 경우 해시함수 값이 편향될 수 있다.
                * M이 소수라면 0~M-1을 골고루 사용하는 값을 만들어낸다.
            * 나머지 연산의 결과가 음수라면 M만큼 더해준다. (결과값은 항상 0~M-1 사이)
        
        * 폴딩 함수
            * 주로 키가 해시 테이블의 크기보다 더 큰 정수일 경우 사용.
                * ex) 키는 32비트, 인덱스는 16비트 정수인 경우.
            * **키를 몇 개의 부분으로 나누어 이를 더하거나 비트별 XOR같은 bool연산을 통해 구한다.**
            ```
                hash_index = (short) (key ^ (key >> 16));
            ```
            * 키를 여러 부분으로 나누어 모두 더한 값을 해시 주소로 사용한다.
                * shift folding: 여러 부분으로 나누어 더한 값 사용.
                * boundary folding: 키의 이웃한 부분을 거꾸로 더해 사용.
                * ex) 키가 12320324111220일 때,
                * shift => 123 + 203 + 241 + 112 + 20 = 699
                * boudary => 123 + 302 + 241 + 211 + 20 = 897
        
        * 중간 제곱 함수: 키를 제곱한 값의 중간 몇 비트를 취해서 해시 주소 생성.
        * 비트 추출: 해시 테이블 크기가 2^k 일 때, 키를 이진수로 간주하여 임의의 위치 k개의 비트를 해시주소로 사용.
        * 숫자 분석: 숫자로 구성된 키에서 편중되지 않는 수들을 조합하여 사용. 
            * ex) 학번의 경우 앞의 네글자는 입학년도로 편중되어 있으므로 가급적 사용 x
        * 주의) 탐색키가 문자열인 경우?
            * 보편적으로 문자의 아스키 코드값이나 유니 코드값 사용. 
            * 방법.1 각 문자의 아스키 코드 값을 모두 더함
                * 문제: 1. cup과 puc을 구분하지 못 한다.
                        2. 아스키 코드 범위는 65~122로 정해져 있기 때문에, 세 글자로 이루어진 키는 195~366으로 해시코드가 집중된다.
            * 방법.2 **아스키 코드 값에, 글자의 위치에 기반한 값을 곱한다**
                * 양의 정수 g에 대하여 (일반적으로 g = 31)
                ```c
                    int hash_function(char *key) {
                        int hash_index = 0;
                        while (*key) {
                            hash_index = g * hash_index + *key++;
                            return hash_index;
                        }
                    }
                 ```

    * 오버플로우를 해결하는 방법
    1. Open Addressing(개방 주소법): 충돌이 이러난 항목을 해시 테이블의 다른 위치에 저장한다.
        * 조사(probing): 해시 테이블에서 비어있는 공간을 찾는 것 
        * 선형 조사법 (linear probing): 해시 테이블(ht)를 순차탐색, ht[k]에서 충돌 발생 시, ht[k+1]이 비어있는지 확인.
            * 만약 테이블의 끝에 도달하면 다시 테이블의 처음으로 간다. 원래자리로 돌아오면 테이블이 가득 찬 상테임을 알 수 있다.
            * 문제점: 군집화(clustering), 한 번 충돌이 시작되면 그 위치에 항목들이 집중되는 현상.
        * 이차 조사법 (quadratic probing): 다음 조사할 위치를 ht[k], ht[k] + 1 * 1, ht[k] + 2 * 2, ht[k] + 3 * 3...
            * 모든 위치를 조사하게 만들려면, **테이블 크기는 소수여야 한다.**
        * 이중 해싱 (double hashing): 오버플로우가 발생한 후 다음 저장할 위치를 정할 때, 별개의 해시 함수를 사용.
            * 일반적인 형태의 이차 해싱함수 step = C - (k mod C) , C는 보통 M(테이블 크기)보다 약간 작은 소수
            * h[k], h[k] + step, h[k] + 2*step, h[k] + 3*step...
            * 마찬가지로 **테이블의 크기는 반드시 소수여야 한다.** (모든 위치를 조사하기 위해.)

    2. Chaining(체이닝): 해시 테이블의 하나의 위치가 여러 항목을 저장할 수 있도록 해시 테이블의 구조르 변경한다.
        * 해시 테이블의 버킷을 연결리스트로 만든다. => 버킷에는 이론적으로 무한개의 항목이 들어갈 수 있다.
        * 고려할 점: 새로운 항목을 연결 리스트의 어디에 삽입할 것인가?
            1. 키의 중복을 허용한다면? -> 연결 리스트 처음에 삽입하는게 효율적.
            2. 중복을 허용하지 않는다면? -> 어차피 연결 리스트를 처음부터 끝까지 탐색해야 하므로 뒤에 삽입.
* 성능 분석
    * 선형 조사(linear probing)의 경우, 적재밀도(저장된 항목 수/버킷의 수)가 0.5를 넘어갈수록 급격하게 탐색 시간이 증가.
    * 이차 조사, 이중 해싱의 경우 적재밀도를 0.7 이하로 유지시키는 것이 좋다.
    * 체이닝의 경우, 적재밀도가 늘어나도 성능이 급격하게 떨어지진 않는다. 또한, 체이닝은 연결 리스트로 구현되어 실제 필요한 만큼만 노드가 생성. 공간효율적이다.
    * 일반적으로 제산 해시 함수와 체이닝을 함께 사용하는 방법이 효율적이다.


