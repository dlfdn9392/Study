## 9. 객체 생성 및 소멸 과정 중에는 절대로 가상 함수를 호출하지 말자

```cpp
class Smoother
{
public:
  Smoother()
  : initialized_(false) {

    configure();
  }

  ~Smoother() {}

  virtual void configure() const = 0;
}

class DummySmoother : public nav2_core::Smoother
{
public:
  virtual void configure() const;
}
```
- 기본 클래스 생성자가 먼저 호출되는데 파생 클래스 멤버들은 아직 초기화가 안되어있음.
- 일단은 기본 클래스로 결정되어 있음
- 위에서 아래로 내려가지 않는다


```cpp
class Smoother
{
public:
  Smoother(const std::string& loginfo)
  : initialized_(false) {

    configure(loginfo);
  }

  ~Smoother() {}

  void configure(const std::string& loginfo) const;
}

class DummySmoother : public nav2_core::Smoother
{
public:
    DummySmoother()
    :Smoother(createLog())

private:
    static std::string createLog (param)
}
```

- 필요한 초기화 정보를 밑에서 위로 올린다


## 10. 대입 연산자는 *this의 참조자를 반환하게 하자
```cpp
x = y = z = 15;
x = (y = (z = 15));
```
- 사슬처럼 되는것은 좌변 인자에 대한 참조자를 반환하기 때문
```cpp
class Widget{
public:
    Wieget& operator= (const Widget& rhs){

        return* this;
    }
}

// operator+=   -=   *= 도 마찬가지
```
- 이것은 관례
