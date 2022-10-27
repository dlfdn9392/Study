# Effective C++ 3th

## 항목 3. 낌새만 보이면 const를 들이대 보자!
### const
- const 객체는 외부 변경이 절대 불가능하다.
- 제작자의 의도를 컴파일러 및 다른 프로그래머와 공유할 수 있는 수단 
    
<br/>

### const 문법
```
void f1(const Widget *pw)
void f1(Widget const *pw)
```
```
      char *       p = greeting;
const char *       p = greeting; // 상수 데이터
      char * const p = greeting; // 상수 포인터
const char * const p = greeting;
```
- iterator는 자신이 가리키는 대상이 아닌 것을 가리키는 경우를 불허 (const 포인터)
- const_iterator는 그 반대 (const 데이터)
<br/>

<br/> 

### const와 함수의 결합
- 안전성, 효율성 극대화
- 기본 제공 타입과의 호환성을 제공해야 한다 
```
const Rational operatior* (const Rational& lhs, const Rational& rhs);

(a * b) = c // ??????
            // ==의 오타?
```

- 상수 객체에 대한 참조자를 조작하는 것이 프로그램 성능면에서 중요하다 (항목 20 참조)

<br/>

### 상수성의 개념
- physical constness (bitwise)
    - 멤버 함수가 그 객체의 어떤 비정적 데이터 멤버도 수정하지 않아야 한다.
    - 데이터 멤버에 대해 대입연산이 없어야한다. 
- logical constness
    - physical 적으로는 문제 없지만 (컴파일러가 허용하지만) 논리적으로 정말 수정 안되는게 맞냐...
    - ex) 포인터, 참조자가 가리키는 대상을 수정할 때 ?

<br/>

### const의 유무로 오버로딩이 가능하다 !
```
class TextBlock{
    public:
        ...
        const char& operator[] (std::size_t position) const
        {return ...}

        char& operator[] (std::size_t position)
        {return ...}        
};

TextBlock tb("hello");
tb[0] = 'x';

const TextBlock ctb("world");
ctb[0] = 'x';       // Error!! 호출은 된다.
```

- 논리적 상수성의 위배
```
class CTextBlock{
    public:
        ...
        char& operator[] (std::size_t position) const
        {return ...}        

    private:
        char *pText;
};

const CTextBlock cctb("Hello");
char *pc = &cctb[0];
*pc = 'J';      // X됐다.
```
- mutable로 선언한 멤버는 const 멤버 함수 내에서도 수정 가능.
```
mutable std::size_t textlength;
```

### const만으로 구분은 했는데 코드의 중복이...
```
class TextBlock{
    public:
        ...
        const char& operator[] (std::size_t position) const
        {
            return text[position];
        }

        char& operator[] (std::size_t position)
        {
            return const_cast<char&>(
                static_cast<const TextBlock&>
                (*this)[position]
            );
        }        
};    
```

- 비상수가 상수버전을 호출하게 만든다
- 역은 너무 위험부담이 크다..

![Screenshot from 2022-10-26 20-49-29](https://user-images.githubusercontent.com/86697110/198018766-00661554-dd7c-4826-b963-b34b18fa0ac6.png)


- const는 에러 잡는데에 도움을 준다. 어떤 유효범위에 있는 객체에도 붙는다.
- 컴파일러 입장은 물리적 상수성만 지킨다. 개발자라면 논리적 상수성까지.
- 코드 중복을 피하자.











