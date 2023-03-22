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
    ```
    function recsum(x) {
        if (x === 0) {
            return 0;
        } else {
         return x + recsum(x - 1);
        }
    }
    ```

    * Tail Recursion
    ```
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
    ```
    struct 구조체태그 {
         항목1; 
         항목2;
    }; 
    ```   
* typedef 구조체 정의
    * 아래 예에서는 student가 새로운 데이터 타입의 이름이 된다.
        ```
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
        ```
        typedef struct {
            int row;
            int column;
            int value;
        } element;
        ```
        * 위에 element배열을 요소로 갖는 matrix 구조체를 만든다.
        ```
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
    ```
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
    ```
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
    ```
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
        ```
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
        ```
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
        ```
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
        ```
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
        ```
        typedef int element;
        typedef struct {
            element data;
            struct ListNode *link;  // 자기 참조 구조체
        } ListNode;
        ```
    * 노드의 생성
        * 연결 리스트의 모든 데이터의 접근하기 위해, 헤드 포인터가 필요하다.
        * 노드가 없는 단순 연결리스트는 다음과 같이 생성한다.
            ```
            ListNode *head = NULL;
            ```
        * 즉, 리스트의 공백검사는 헤드 포인터가 NULL인지 확인한다.
        * 연결리스트는 필요할 때마다 노드를 동적 할당하여 생성한다. 
            ```
            head = (ListNode *)malloc(sizeof(ListNode));
            head->data =10; // 노드에 데이터를 저장한다.
            head->link = NULL;
            ```
    * 노드의 연결
        * 노드의 연결은 간단하게, 새로운 노드를 동적으로 생성 및 데이터를 저장하고 이전 노드의 링크에 생성한 노드의 주소를 넣는다.
            ```
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
            ```
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
            ```
            typedef struct ListType {
                int size;
                ListNode *head;
                ListNode *tail;
            } ListType;
            ```
        * 헤더노드를 운용하기 위해선, 헤더 노드를 동적으로 생성하고 초기화해야한다.
            ```
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
        ```
        typedef int element;
        typedef struct DListNode {  // 이중 연결 노드 타입
            element data;
            struct DListNode *llink;
            struct DListNode *rlink;
        } DListNode;
        ```
    * 연산
        1. 삽입 연산
            ```
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
            ```
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
        ```
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
            ```
            void push(LinkedStackType *s, element item) {
                StackNode *newnode = (StackNode *)malloc(sizeof(StackNode));
                newnode->data = item;
                newnode->link = s->top;
                s->top = newnode;
            }
            ```
        2. pop()
            ```
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
        ```
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
            ``` 
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
            ```
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
            ```
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
            ```
            preoreder(x):
            if x != NULL:
                then print(DATA(x));
                preorder(LEFT(x));
                preorder(RIGHT(x));
            ```
    * **중위 순회(inorder traversal)**
        * 왼쪽 서브트리, 루트, 오른쪽 서브트리 순으로 방문한다.
        * 중위 순회 스도코드
            ```
            inoreder(x):
            if x != NULL:
                then inorder(LEFT(x));
                print(DATA(x));
                inorder(RIGHT(x));
            ```
        * 즉, 전위, 중위 그리고 뒤에 나올 후위 순회 알고리즘까지 호출의 순서만 다르다.
    * **후위 순회(postorder traversal)**
        * 왼쪽 서브트리, 오른쪽 서브트리, 루트 순으로 방문한다.
            ```
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
        ```
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
        ```
        int get_node_count(TreeNode *root) {
            int count = 0;
            if (root != NULL)
                count = 1 + get_node_count(root->left) + get_node_count(root->right);
            return count;
        }
        ```
    
    * 단말 노드 개수
        ```
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
        ```
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
        ```
        typedef struct TreeNode {
            int data;
            struct TreeNode *left, *right;
            int is_thread;  // True이면 오른쪽 링크가 스레드
        } TreeNode;
        ```
    * 연산
        ```
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
        ```
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

        // 반복적 탐색연산
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
    
