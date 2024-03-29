# TDD에 대한 다양한 시각

## TDD와 소프트웨어 디자인

### TDD가 주는 설계상의 이점

소프트웨어를 개발하는 건 쉬운 일이 아니다. 특히, 개발 범위 및 규모가 클수록, 그리고 해당 도메인의 업무가 복잡다단할수록 어지러운 코드가 많이 양산되어 넘어서기 어려운 수준(insurmountable chaos level)의 개발 난이도를 만들어내기 쉽다. 결국 혼란 속에서 자멸하거나 누더기가 돼서는 건드릴 수 없게 변한다. 동작은 하지만 만지면 곧잘 깨져버리는 유약한 소프트웨어가 된다는 이야기다. 이런 상황에서 TDD는 어떤 설계상의 이점을 줄 수 있을까? TDD로 개발을 하면 TDD 자체가 단위 단위의 작은 설계를 만들어낸다. 입력과 출력, 그리고 해당 모듈이 동작하기 위해 필요한 요소 파악 등이 사전에 확실하게 고려되고 테스트된다. 그렇게 만들어지는 모습을 강형 마이크로 디자인(Robust Micro Design)이라고 한다. 이런 디자인의 특징 중 하나는 다른 모듈에 대한 의존성이 상대적으로 적다는 점이다. 왜냐하면 다른 모듈에 의존성이 많아지면 적절한 테스트 케이스를 작성하기가 점점 어려워지기 때문에, 개발자가 스스로 모듈의 의존성을 줄이려는 노력을 초반부터 해나가게 된다. 또한 각 모듈은 최대한 테스트를 독립적으로 수행할 수 있도록 만들기 때문에 자기 완결성(self completeness)이 높은 모듈이 된다. 결과적으로 신뢰수준을 따져봐야 하는 모듈에 대해, 개발자가 접근해야 하는 영역을 좀 더 추상화된 상위 레벨로 올려줄 수 있기 때문에, 소프트웨어의 복잡도가 낮아지고 개발자가 좀 더 일반화되고 추상화된 형태의 디자인에 더 집중할 수 있게 도와준다. 

### TDD와 객체 지향 프로그래밍(OOP)

OOP에서는 모듈화가 굉장히 강조되고 있고, 대부분의 객체 지향 프로그래밍 언어는 모듈화를 지원하는 기능을 기본적으로 내장하고 있다. 이를테면 클래스나 패키지, 인터페이스 등이 그런 요소다. OOP가 각광받고 유독 모듈화가 강조되는 이유는 '**안전한 부품**' 그리고 '**재사용성**'을 높여주는 요소를 좀 더 쉽게 만들 수 있기 때문이다. 그리고 그걸 이루기 위해 최대한 따라야 하는 가이드로, OOP의 원칙이라는 것이 있다. 그런 OOP의 근간이 되는 기초 원칙 중에는 '**모듈(module)은 높은 응집도(high cohesion)를 유지하고 낮은 결합도(loose coupling)를 갖도록 만들어야 한다**'는 원칙이 있다. 아주 중요한 OOP의 핵심 원칙 중 하나다. 수많은 개발자들이 객체 지향 언어를 사용해서 절차적 프로그래밍을 하고 있다. 또 설사, 위 원칙을 염두에 두고 설계를 하고 코드를 작성했다 하더라도, 정말 잘 따르고 있는 건지, 따랐다면 해당 모듈이 어느 정도 레벨의 응집도와 결합도를 갖고 있는지 판단하기가 쉽지 않다.

어느 회사에서 직원의 연봉을 구하는 프로그램 모듈을 작성한다고 가정해보자. 만일 객체 지향 설계를 하라고 한다면 많은 경우 클래스부터 만들려고 할 것이다. 아쉽지만 그건 별로 좋은 방법이 아니다. 또 그 다음엔 여러 모듈(클래스)을 만들어서 이용할 수 있는 한 최대한 재사용하려고 노력할 것이다. 그러다 보면 다음과 같은 코드도 서슴지 않고 나온다. 코드가 무슨 내용인지 크게 집중할 필요는 없다. 그냥 코드의 구조만 보자. 그리고 이어지는 두 코드는 서로 관련 없는 각각의 코드다.

##### 예 1

```java
public class MeasureCounter {
    
    public String getTotalMeasure(String name){
        Stock b = new Stock(name); // 객체 생성
        int firstCountRate = b.getFirstCountRate(); // 최초 비율
        int runningCount = b.getRunningCount()/2; // 러닝카운트
        int remainingCount = b.getRemainings(); // 남은 수량
        return name + ": " + (firstCountRate + runningCount + remainingCount);
    }
    ...
}
```

##### 예 2

```java
public class PublicItem {
    public int getDualCharge(Sender sender){
        int fare = 0; // 요금
        if ( this.item.getStore().getCustomer(sender.getName()).isVip() ){
            fare = sender.getBox().getFare() * 0.8;
        } else {
            fare = sender.getBox().getFare();
        }
        return fare;
    }
    ...
}
```

그나마 깔끔하게 작성한다고 한 코드다. 그리고 잘 동작한다. '아, Java로 짰고, 잘 동작하고, 눈으로 보기에도 별로 복잡하지 않고, 간결한게 아주 마음에 드네!'라는 생각이 들 수도 있다. 하지만 그러면 안 된다. 만일 뭐가 잘못된 코드인지 잘 모르겠다면 큰일이다. 고무적인 사실은, 위 두 코드는 TDD로 만들었다면 좀처럼 쉽게 만들어지지 않았을 코드라는 점이다. 우선 첫 번째 코드부터 보자. getTotalMeasure()라는 메소드를 작성하기 위해 TDD로 작성한다고 가정해보자. 테스트 코드 입장에서 미리 준비 가능한 건 name뿐이다. name을 넣으면 해당 이름에 해당하는 Stock의 특정 정보들을 알려주는 기능을 만들면 될 것 같다.

> 아, 잠깐! 그런데 TotalMeasure를 구하려면 Stock 관련 정보가 있어야 할 것 같은데, 내 클래스 안에는 없잖아! 그럼, Stock 관련 정보는 어디에 있는 거지? 아, Stock 클래스에 있지. 잠깐! 이거 뭐야? 내가 테스트하고자 하는 기능들은 대부분이 내 자신의 클래스인 MeasureCounter에 있는 게 아니라, Stock에 있잖아! 여기서 이걸 테스트 하는 게 맞는 걸까?

TDD로 작성을 하게 되면 이런 식으로 기능과 객체의 관계를 스스로 먼저 고민하게 된다. 그리고는 때때로 현재와 같은 클래스 구조로는 테스트 코드를 미리 작성할 수가 없다는 사실을 스스로 알아차리게 된다. 테스트 케이스 코드는 입력과 출력(in/out)이 명확하고, 사용되는 재료가 적으면 적을수록 작성하기가 쉽다. 즉, 의존관계가 많은 코드는 테스트 코드 자체를 만들기가 어렵다. 사람의 본성이란 게, 어려우면 안하게 된다. 의존관계를 최대한 만들지 않고 기능이 동작하도록 노력을 기울이게 된다. TDD는 자신의 모듈에 필요한 게 무엇인지를 미리미리 고민하게 만든다. 기능 위주로 테스트를 하기 때문에 불필요한 속성/필드가 무엇인지도 초반부터 밝혀진다. 따라서 테스트를 작성하다 보면 getTotalMeasure 메소드는 기능이 전적으로 Stock 클래스에 의존하고 있음을 알게 되고 해당 기능을 Stock 클래스를 테스트하는 걸로 변경해야겠다는 생각이 든다. 그리고 결국에 가서는 MeasureCounter 클래스에서 getTotalMeasure가 존재할 필요가 없음을 깨닫게 된다. 이게 바로 응집도를 높이고 결합도를 낮춘 모습이다. 두 번째 소스도 테스트 케이스를 작성하다 보면, 자신의 모듈이 동작하기 위해 필요한 재료를 상당히 멀리서 가져온다는 사실을 테스트 케이스 작성 시에 발견하게 된다.

```java
this.item.getStore().getCustomer(sender.getName()).isVip();
```

진정 자신의 클래스 안에 존재해야 하는 커플링 객체가 item인지 customer인지 다시 한번 고민하도록 유도한다. 왜? 테스트 케이스를 작성하려면 재료가 필요하니까. 그리고 그 재료가 어떻게 쓰이는지 미리 고민해야 하니까. 이렇듯 TDD는 좀 더 나은 설계, 좀 더 나은 모듈 디자인이 될 수 있도록 스스로 생각할 수 있게 유도하고, 결과적으로는 모듈이 더 나은 구조를 가질 수 있게 한다. 

### 유연한 코드

유연한 코드를 만든다는 건 무엇을 의미할까? 흔히 변화에 쉽게 적응할 수 있는 코드 (기능 변경 요구에도 구조가 변경되지 않는 코드)를 유연한 코드라 부른다. 또한, 한편으론 요구사항을 받아들이는 데 개발자의 거부감이 적은 코드를 뜻하기도 한다. 대부분의 경우 개발자의 거부감은 개발자가 다루는 소프트웨어 소스의 복잡도에 기인한다. 코드가 복잡하면, 신경 쓸게 많아지고 민감하게 동작하는 코드가 많아진다. 그래서 복잡한 소스를 수정할 경우 개발자에겐 거부감이 생기게 된다.

이 거부감은 따지고 보면 결국 소프트웨어 실패(failure)에 대한 두려움이다. 그런데 사실 이 부분에선 모순이 존재한다. 소프트웨어의 변경이 쉬워지려면 코드가 유연해야 하는데, 소프트웨어 공학에서 추구하는 유연함(flexibility)이란 때로 '개발비용 증가'의 또 다른 요소가 되기도 한다. 이 비용은 온전히 개발자가 떠안게 되는 비용이다. 지나치게 다양한 선택지가 존재하는 환경은 최적의 선택지를 고를 확률을 떨어뜨린다. 그래서 유연한 코드를 만들어서 변경을 용이하게 만든다는 건, 무수히 많은 레고 조각을 제공해놓고는 "어떠한 모양의 자동차도 만들 수 있다!"라고 말하는 것과 유사하다. 그럼 어떻게 해야 할까? 최대한으로 적절한(reasonable) 선택을 할 수밖에 없다. **현재 필요한 기능에만 최대한 집중**하는 게 여기서 말하는 조금의 노력이고 적절한 선택이다.

개발자는 일반적으로 다음과 같은 감정을 갖는다.

- **한 클래스의 메소드 숫자가 많을수록 복잡하다고 느낀다.**
- **한 시스템의 클래스 개수가 많을수록 복잡하다고 느낀다.**

따라서 코드 검사(inspection) 툴이나 동료검토(peer-review)를 통해 스스로 작성한 코드가 적절히 유연한 코드인지 측정해볼 필요가 있다. 이때, 다음과 같은 측정은 제대로 된 측정이라 할 수 없다

- **작성자 자신이 사용한 디자인 패턴들의 목록 감별 시간**
- **작성자 자신이 코드에 변경을 가하는 데 소요되는 시간**

이때 걸리는 노력과 시간으로 코드의 적절한 유연성 여부를 스스로 판단하는 건, 작성자의 지식 수준과 코드 소유욕에 기반한 측정 오류의 흔한 예가 된다. 그것보다는 아래에 해당하는 시간을 측정해보자.

- **시스템에 익숙하지 않은 사람이 새로운 요구사항 반영을 위해 소프트웨어에 변경을 가하는데 소요되는 시간**

이 시간과 노력을 측정하는 것이 적절히 유연한 코드인지를 판단하는 기준으로 좀 더 유용하다. 그리고 이 원칙은 테스트 케이스를 작성할 때도 마찬가지다.

## TDD 유의사항 

- ### 테스트 케이스는 이름이 중요하다

  TDD에 익숙하지 않고 학습 단계에 있을 때 흔히 간과하는 것 중 하나는, 작성된 테스트를 수행하거나 읽는 사람이 자신 혼자라고 은연중에 가정한다는 점이다. 그러다 보니 무엇을 테스트하는 것인지 테스트 메소드 이름만으로는 알기 어려운 경우가 종종 발생한다. 자신만 알아보기 쉽게 만들었다든가, 아니면 오해의 소지가 있는 단어를 썼기 때문이다. 보통 테스트가 실패하면 실패한 메소드의 이름이 테스트의 기준이 된다. 따라서 테스트의 이름이 잘 작성되어 있어야 실패에 대한 대응을 빠르게 할 수 있다. 또 가끔은, testDeposit_minusCase01, testDeposit_minusCase02 같은 식의 일련번호를 이름에 사용하는 모습을 보곤 한다. 이런 경우도 권장하지 않는다. 테스트 메소드의 이름이 좀 더 길어지더라도 해당 테스트가 무엇이고 실패한 세부 항목이 무엇인지 곧바로 알 수 있어야 한다. 번호 대신에 의미 있는 이름을 붙여주자. 그리고 경우에 따라서는 테스트 메소드의 이름에 한글을 사용하는 것도 도움이 된다.

  ```
  testWithdraw_마이너스통장_대출한계이상으로_인출요구시();
  testWithdraw_마이너스통장_연체이자미납자_인출요구시();
  ```

  그런데 이렇게 테스트 메소드의 이름을 설명적으로 풀어쓰는 식으로 작성하다 보면 자칫 테스트 메소드의 이름이 길어질 수 있다. 그렇다고 해서 굳이 불편해한다든가, 약어를 써서 줄이려고 한다든가 할 필요는 전혀 없다. 로직을 살펴보지 않고 메소드 이름만으로도 충분히 의미를 전달할 수 있을 정도로 작성하는 것이 중요하다.

- ### 더 이상 제대로 동작하지 않는 테스트 케이스는 제거한다

  자동화된 테스트 메소드는 소스의 품질을 좌우하는 중요한 자산이다. 하지만 그렇다고 해서 많으면 많을수록 무조건 좋은 건 절대 아니다. 중복된 테스트 케이스나 업무 변경 등의 이유로 더 이상 제대로 동작하지 않는 테스트 케이스는 과감하게 제거한다. 소스의 품질과 테스트 케이스의 숫자는 항상 일치하는 게 아니기 때문이다.

- ### TDD는 자동화된 테스트를 만드는 것이 최종 목표가 아니다

  TDD는 개발의 목표 지점을 미리 정하기 위해 단위 테스트 케이스를 만들고, 목표 상태 도달 여부를 빨리 확인하기 위해 단위 테스트 케이스를 자동화시킨다. 자동화된 단위 테스트 케이스들은 TDD의 부산물이다. 개발과 설계를 위한 보조 도구이지 목적은 아니다. 자동화된 테스트는 말 그대로 자동적으로 수행되는 테스트를 의미하고, 소프트웨어의 현재 상태의 정상 여부 판단이 목표이다. 향후 소프트웨어 내부가 변경됐을 때 문제가 없는지를 판단하는데도 물론 사용할 수 있다. 미묘한 차이점인데, 확실히 구분해놓아야 하는 개념이다.

- ###  모든 상황에 대한 테스트 케이스를 만들 필요는 없다

  요구사항에 맞는 현재 필요한 기능에 대한 테스트만 만든다. 개발자가 개발을 하다 보면 종종 지나치게 테스트에 집착하는 경향을 보이다가 어느 순간엔가 지쳐버리는 모습 을 보곤 한다. 'TDD는 힘들고 어렵고 복잡하구나'라고 말이다.

- ### 여러 개의 실패하는 테스트 케이스를 한 번에 만들지 않는다

  개발 중에는, 작성하고 있는 하나의 클래스에 대해서는 하나의 실패하는 테스트만 유지한다. 해당 실패를 성공시킨 다음에 다음 실패하는 케이스를 작성한다.
  
- ### 하나의 테스트 케이스는 하나만 테스트하도록 작성한다

  때로는 하나의 테스트 메소드에서 하나 이상의 항목을 한꺼번에 테스트하고자 하는 욕망에 빠질 수 있다. 하나의 테스트 메소드 내에서는 한 가지만 테스트한다는 이 원칙을 강조하는 어떤 사람은 하나의 테스트 메소드 내에서는 assert 계열의 단정문도 딱 한 번씩만 쓰자고 말한다. 그렇게까진 못하더라도 테스트 메소드 이름을 배신하듯 여러 항목을 테스트하는 건 최대한 자제해야 한다.

- ### 전통적인 테스트 기법을 배워두자

  각종 테스트 기법을 잘 알면 좀 더 효율적인 테스트 클래스 작성이 가능해진다. 그리고 어떤 부분이 테스트가 필요한지도 더 잘 파악할 수 있게 된다. 테스트 마스터가 될 필요는 없지만, 기초 정도는 배워놓으면 TDD를 잘하는 데에 좀 더 도움이 된다.

- ### 테스트 케이스는 최대한 고립시킨다

  각각의 단위 테스트는 다른 모듈이나 시스템에 최대한 독립적이고 고립된 형태로 작성 될수록 단단한 테스트 케이스가 될 수 있다. 그래서 가급적이면 다음과 같은 것들이 테스트에 들어가지 않도록, 혹은 직접적으로 영향을 미치지 않도록 작성하면 더 좋다.

  - 테스트 케이스가 작성되어 있지 않은 다른 모듈
  - 데이터베이스 연동
  - 외부 시스템
  - 콘솔 출력(System.out)
  - 네트워크

  물론 위에 나열한 것 자체를 테스트한다면 달라지겠지만, 대부분의 경우 현재 기능을 위해 외부에 의존하는 것들이다. 이를테면 데이터베이스가 없다면 테스트 케이스를 수행할 수 없다든가, 네트워크가 끊겨 있거나 불안정한 상태에서는 관련된 기능 전체를 다 테스트를 수행할 수 없게 된다면 곤란하다. 결국 최대한 의존관계를 줄이고, 경우에 따라서는 Mock 객체를 이용해서라도 독립성을 보장해줘야 특정 테스트 실패에 따른 실패 전파현상(failure propagation)을 최소한으로 줄일 수 있다. 참고로, 의존성을 분리시키지 못해서 코드를 조금만 수정해도 많은 테스트가 실패하게 되는 테스트를 깨지기 쉬운 테스트(fragile test)라고 부른다.

## TDD와 리팩토링

효과적인 TDD를 진행하는 데 있어 알아야 할 사항이 많지만, 그중에서 TDD 자체를 제외하고 딱 하나만 우선적으로 알아야 한다면, 그건 아마 리팩토링(refactoring)일 것이다. 그만큼 TDD에서 리팩토링은 중요한 부분을 차지한다. TDD는 잘 동작하는 깔끔한 코드를 목표로 한다. 이를 통해 간결한 설계와 개발을 지향한다. 리팩토링은 설계를 개선하고 유지하는 것이 목표다. 이를 위한 기본 조건은 잘 동작하는 깔끔 한 코드일 수밖에 없다. 둘은 서로 함께했을 때, 각자 더 높은 가치를 갖게 된다. 그리고 자동화된 테스트 케이스 없이 리팩토링을 하는 건 절대 금지하는 항목 중 하나다. 그런데 자동화된 테스트 케이스는 소스가 작성된 다음에 작성하려면, 심리적인 요인에다가 테스트 작성에 소요되는 시간과 육체적인 상황으로 인해 쉽지가 않다. 따라서 소스가 작성되는 시점에 함께 만들 수 있으면 금상첨화일 것 같다. 어떻게 하면 되려나 생각해봤더니, TDD를 하면 코드가 작성됨과 동시에 자동화된 테스트 케이스가 나오게 된다고 한다. 'TDD를 적용해야겠다'는 생각이 든다. TDD를 할 때 하나의 단계가 끝날 때마다 리팩토링을 하면, 대상 코드뿐 아니라 자동화된 테스트 케이스 자체도 사용하기 좋은 코드로 정련이 된다. 서로가 서로를 맞물고 있는 형태다. **TDD에 있어서 리팩 토링은 매우 중요하다.**

리팩토링의 궁극적인 목적은 '**이해하기 쉽고, 수정하기 쉬운 코드로 만들자**'이다. 그리고 여기서의 이해는 컴퓨터가 아니라 사람이 주어다. 우리는 종종 다른 사람을 배려하지 않는 코드를 작성하곤 한다. 나타내고자 하는 의미를 좀 더 명확하게 하는 것, 그것이 리팩토링의 기본이다. 리팩토링은 복잡하거나 어려운 그 무엇, 이를테면 디자인 패턴 같은 부류가 절대 아니다. 쉽게 배우고 크게 효과를 볼 수 있는 몇 안 되는 기술 중 하나다.

## TDD와 짝 프로그래밍

사람은 혼자 배울 수는 없다. 책이든 사람이든 스승이 필요하다. 책은 일방적으로 전달하는 매체이고 사람은 상호작용할 수 있다. 책은 내용이 고정되어 있고, 사람은 1~2분 사이에도 이전보다 발전한 상태가 될 수 있다. 사람 사이의 상호작용과 서로의 경험과 지식이 혼합되는 형태의 학습은 단순히 책을 보고 따라 할 때와는 상당히 다르다. 애자일 진영에서 쉽게 많이 사용되고, 그 효과에 대해 널리 이야기되는 실천법으로 짝 프로그래밍(pair programming, 페어 프로그래밍)이 있다. 오랜 기간 동안 함께 일해 온 동료들끼리도, 옆 동료의 일하는 방식을 자세히 살펴본 경험이 거의 없는 경우가 많다. TDD는 설계를 위한 기법이고 작업을 해나가려면 때때로 많은 전략과 사고가 필요한 기법이다. TDD와 짝 프로그래밍은 잘 어울린다. 한 사람보다는 두 사람의 머리가 더 나은 답을 찾을 확률이 높다. 보통 사용자 스토리를 완성하기 위한 세부 ToDo 리스트가 만들어진 다음에, 짝 프로그래밍으로 TDD를 적용해보면 효과가 더 높다. TDD에 빨리 적응하는 데도 도움이 되고, 도메인 업무에 대한 이해를 높이는 데도 크게 도움이 된다. 특히 프로젝트 초반일수록 얻게 되는 이점이 더욱 더 크다. 그 외에도 짝 프로그래밍의 장점 몇 가지를 꼽아보자면 다음과 같다.

- 팀원 간의 좀 더 많은 대화로 목표 시스템에 대한 이해 증가
- 제품에 대한 공동설계와 공동소유
- 개발에 효율을 높일 수 있는 작업 방식의 공유
- 개발 스킬 향상

물론 이 외에도 많은 것을 얻을 수 있다. TDD의 스킬을 빨리 높이고 싶다면, 힘들더라도 짝 프로그래밍을 도전해보기 바란다. 고생한 만큼 얻는 게 클 것이다. 비록 실패하더라도 다음에 동일하게 실패하지 않을 수 있는 배움은 남게 되어 있다. 그리고, 필요하다면 TDD가 어느 정도 익숙해질 때까지 팀 내에서 짝 프로그래밍을 의무화하는 것도 좋은 방법이 될 수 있다. 하다 보면 느니까 포기하지 말고 꼭 해보자.

## 행위 주도 개발

TDD는 프로그램의 가장 하위 부분인 단위 메소드의 기능에 집중한다. 물론 테스트 케이스 작성이 추가되고 메소드 호출의 단계가 상위레벨로 진행될수록, 단위 테스트라기 보다는 사용자 테스트(acceptance test)에 가까워지긴 한다. 하지만, 어쨌든 시스템의 하위 수준이라고 불릴 만한 밑바닥에서부터 시작하는 것이 일반적이다. 시스템 안쪽에 서부터 시작해서 사용자에 가까운 바깥쪽으로 만들어나가는 방식이기 때문에, TDD와 같은 방식을 인사이드-아웃(inside-out) 방식이라고 부른다. 보통 이런 형식으로 진행되는 TDD에는 몇 가지 흔한 어려움이 있다. 바로, 'TDD를 어디에서부터 시작할 것인가?', '테스트를 어느 정도 작성하면 될 것인가?'와 같은 TDD의 시작점과 범위에 대한 문제다. 그리고 때에 따라서는 '어떤 것은 꼭 테스트해야 하고, 어떤 것은 테스트를 안 해도 될까?'에 대한 적절한 판단도 함께 필요하다. 이런 문제를 좀 더 문맥적으로 해결할 수 있게 도와주는 방식으로 행위 주도 개발(Behavior-Driven Development, BDD)이라는 게 있다.

- ### BDD의 정의와 목표

  BDD는 **책임관계자**(stake holder)**의 관점에서 보는 애플리케이션의 행위(동작) 중 가치 있는 기능부터 개발하는 방식**이다. 그렇기 때문에 보통, 애플리케이션의 로직 중에서도 특히 업무규칙과 관련된 부분을 밖으로 노출하는데 집중한다. 그리고 프로그래머가 아닌 책임관계자도 쉽게 살펴보고 읽을 수 있도록 가급적 자연어를 사용해 기술하려 노력한다. BDD는 이런 식으로 프로젝트에 참여하는 인원들, 이를테면 품질전문가(QA), 업무전문가, 경우에 따라서는 고객에 이르기까지, 참여자들의 협업을 증진시키는 것을 목표로 만들어졌다. BDD라는 이름 자체는 2003년 댄 노스(Dan North)라는 엔지니어가 만들었다. 

- ### BDD의 특징

  BDD는 세 가지 측면에서 TDD와 차이점이 있다.

  첫 번째로, BDD는 사용자(고객)에 좀 더 가까운 고수준의 기능영역을 우선적으로 다룬다. 고객은 JUnit이 어노테이션으로 동작하는지, test로 시작하는 메소드로 동작하는지 관심이 없다.

  두 번째로 BDD는 '무엇을 테스트할 것인가?'에 좀 더 초점을 맞추고 있다. 흔히 BDD로 개발을 진행할 때, 전체를 관통하는 가장 중요한 질문은 "현재 시스템에 들어 있지 않은 기능 중 가장 먼저 구현돼야 하는 기능은 무엇인가?"라고 말한다. 작성하고자 하는 프로그램 기능들의 비즈니스적인 가치를 파악하고 그에 대한 우선순위를 부여한다. 만일 현재 계좌이체 기능(Transaction)이 구현되어 있지 않은 상태이고, 해당 기능이 우선적으로 구현돼야 할 만큼 중요하다고 판단되면, 다음과 같은 테스트 메소드를 작성하는 것이다.

  ```java
  class AccountTest {
      
      public void shouldTransferMoneyToOtherAccount(){
          ...
      }
  }
  ```

  사용자에 가까운 부분에서 안쪽 모듈을 향해 개발을 진행해나가기 때문에 BDD를 아웃사이드-인(outside-in) 방식이라고도 부른다.

  BDD의 세 번째 특징은, **테스트 메소드 작성에 집중할 수 있는 문장적인 템플릿을 제공한다**는 점이다. 다음은 일반적으로 BDD를 적용할 때 사용하는 문장 템플릿이다.

  ##### BDD의 기본 템플릿

  | 문장  | 설명                 |
  | ----- | -------------------- |
  | Given | 주어진 상황이나 조건 |
  | When  | 기능 수행            |
  | Then  | 예상 결과            |

  어찌 보면 별것 아닌 템플릿이지만, 많은 BDD 지지자들은 위 템플릿의 강력함에 대해 칭송한다. 그 이유 중 하나로 일반적인 인간의 사고방식과 비슷한 스타일이라는 점을 꼽는다. 사용하는 언어는 생각의 방식에 영향을 크게 미치기 마련이다. 이런 식으로 선 조건과 동작, 그리고 결과를 구분지어 생각하도록 유도하는 방식은, 그것도 자연어 문장으로 만들어지는 템플릿은 assertEquals보다는 훨씬 편안함을 준다. 그리고 여기에 덧붙여 메소드 이름에 'Should' 같은 목표 행위를 기술하기 위한 단어를 적절히 사용하면 코드의 목적이 좀 더 분명해지곤 한다. 그리고 이왕이면 책임관계자들이 이해할 수 있는 자연어 문장으로 표현된 테스트 케이스(시나리오)와 결과를 만드는 것을 지향한다.

  ##### 예

  ```java
  public void shouldWithdrawMoney() {
      ...
  }
  ```

  사실 TDD의 테스트 케이스를 잘 살펴보면 test라는 이름으로 시작했기 때문에 흔히 맞는지 틀리는지(true or false)만을 체크하는 식으로 이름을 짓게 된다. 하지만 행위 기준으로 보면 예상하는 행동을 하는지(should do something) 위주로 작성하게 된다. 언뜻 보면 비슷한 듯 보이지만, 사실 잘 살펴보면 지향하는 바가 조금 다르다.

- ### BDD 접근 전략

  개발 시에 BDD와 TDD 중 꼭 하나만을 선택해야 하는 건 아니다. 각각 필요한 영역이 다르기 때문이다. 고객은 알 필요가 없지만, 개발자는 반드시 알아야 하고 견뎌내야 하는 로직을 다룰 때는 TDD가 좋다. 그리고 고객 관점에서 이야기하거나 대상 시스템의 최종 형태를 파악하며 개발해야 할 때, BDD를 우선적으로 사용한다.

- ### BDD와 TDD

  TDD가 예제에 의한 개발(driven by example)과 단위 테스트 케이스를 지향한다면, BDD는 사용자 시나리오를 통해 사용자 테스트와 회귀 테스트를 지향한다.(물론 둘 다 자동화는 기본이다)

  TDD로 개발하는 시스템은 모듈 모듈이 자칫 지나치게 작고 얇게 만들어져서 산만하게 흩어진 모습을 갖게 될 수 있다. BDD는 최종 목표를 미리 명확하게 해서, 이렇게 시스템의 모듈이 잘게 산재되는 것을 막고 가장 높은 가치를 제공하는 데 집중해서 만들 수 있도록 유도한다. TDD가 개발자를 위한 방식이라면 BDD는 책임관계자와 함께 하기 위한 방식이다.

- ### BDD 정리

  종종 BDD는 단어가 주는 뉘앙스로 인해, TDD에서 유래된 xDD 시리즈의 말장난처럼 들리기도 한다. 하지만 그렇지는 않다. BDD는 TDD에서 사용되는 일종의 개발 전략이고 어떤 면에서는 발전형이라고 볼 수도 있다. 그와 동시에 핵심 업무도메인을 대하는 일종의 대화법이다. 사실, BDD와 TDD는 사용하는 어휘와 목적이 조 금 다를 뿐, 목표가 같다. 간결한 설계를 만들고, 소프트웨어를 만들기 위한 출발점을 제공한다는 것, 바로 그것이다.

  한 가지 덧붙이자면, 개인적으로 흥미롭다고 생각하는 것 중 하나는, DDD(도메인 주도 개발)나 BDD가 추구하는 목표 지점 중에 개발자와 업무전문가가 함께 이야기 할 수 있는 언어를 만들기 위한 노력이 존재한다는 점이다. 이를 위한 DDD에서는 DSL(Domain Specific Language)이라는 좀 더 생활언어 친화적인 중간 프로그래밍 언어(half programming language)를 제시하고 있고, BDD에서는 문장으로 이뤄진 시나리오나 스토리를 반자동적으로 프로그래밍화를 만드는 기법을 제시한다. 

  마지막으로, BDD는 대략 어떠한 모습으로 만들어지는지 간략한 예제로 살펴보자. 다음은 JBehave(http://jbehave.org/)라는 BDD 프레임워크로 만들어진 테스트 케이스 예제다.

    ##### JBehave로 만들어진 코드 샘플

  ```java
  public class LoginSteps extends Steps {
  // setup 코드들
  ...
            
      @Given("로그인하지 않은 상태라고 가정하고")
      public void logOut() {
      	page.click("logout");
  	}
    
      @When("$username과 패스워드 $password로 로그인했을 때")
      public void logIn(String username, String password) {
          page.click("login");
      }
        
      @Then("다음과 같은 메시지가 보여야 한다, \"$message\"")
      public void checkMessage(String message) {
          ensureThat(page, containsMessage(message));
      }
  }
  ```

    ##### 다음은 easyb(http://www.easyb.org/)로 작성된 테스트 케이스 예제다.

    ##### easyB로 작성된 테스트 코드

  ```java
  scenario "계좌에서 인출하기", {
      given "초기 10000원으로 계좌를 생성했다고 가정하고", {
          initialBalance = 10000
              account = new Account()
              account.balance = initialBalance
      }
        
      when "1000원을 인출하면", {
          withdrawals = 1000
              account.withdraw(withdrawals)
      }
    
      then "1000원을 뺀 나머지금액 9000원이 있어야 한다", {
          account.balance.shouldBe initialBalance - withdrawals
      }
  }
  ```