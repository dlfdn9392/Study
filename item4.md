## 4. 객체를 사용하기 전에 반드시 그 객체를 초기화하자.

### 선언만 하고 초기화를 하지 않는다면...

```cpp
class Point {
    int x, y;
};

... 

Point p;    // X된다.

// 쓰레기값 무작위 비트 이상한 값
```

- 변수들은 항상 초기화를 해서 사용.
```cpp
int x = 0;
const char * text = "ABC";
```
- 그 외 경우는 다 생성자에서 해결한다.
- 생성자에서는 그 객체의 모든 것을 초기화한다.

```cpp
class ABEntry{
private:
    std::string theName;
    int theNum;

    ABEntry(const std::string& name){
        theName = name;
        theNum = 0;
        // 이것은 대입이지 초기화가 아니야
        // 이미 theName의 기본 생성자 호출
        // int(기본제공타입)는 초기화 보장을 못한다.
    }
}


class ABEntry{
private:
    std::string theName;
    int theNum;

    ABEntry(const std::string& name)
    :theName(name), theNum(0)   // 초기화 리스트
    {}    
}

class ABEntry{
    ABEntry()
    :theName(), theNum(0)   // 복사 생성자의 원리
    {}    
}

```
- 데이터 멤버에 대한 생성자의 인자로 바로 쓰인다.
- 클래스 데이터 멤버는 모두 초기화 리스트에 !
- 상수이거나 참조자인 데이터 멤버의 경우엔 의무적이다.

### 초기화의 순서
- 기본 -> 파생 클래스
- 클래스 내에선 선언된 순서대로.

### 번역단위마다 다른 초기화 순서.
- 비지역 정적 객체의 초기화 순서는 개별 번역 단위에서 정해진다. ??????
```
비지역 = 함수 안에 있지 않은
정적 = static. 프로그램 시작부터 끝까지 살아있는 객체.
```
- 별도로 컴파일된 소스 파일이 2개 이상 있는데 이 사이에서 순서 어떻게 정해
- 비지역을 지역으로 바꿔서 그 안에서 순서 정해놔 미리





