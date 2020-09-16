---
title: Readable Code를 읽으며(2)
date: 2020-09-15
categories: readable code
---
## 2편 입니다.
흐름제어와, 코드를 잘게 쪼개는 방법, 변수와 가독성에 대한 이야기를 할까 합니다.  
지난번 포스팅하면서 내용도 많이 부족했고, 이런글을 블로그에 올려놨다는 것 자체가 좀 부끄럽게 느껴지기도 했다.  
그래도 이제 좀 알게 되었으니 이번에 올리는 건 좀 더 발전된 글이 되길 바란다.  

## 흐름제어
조건, 루프, 흐름을 통제하는 선언문이 코드에 없으면 코드를 매우 읽기 편할 것이다.  
이와 같은 분기문, 점프문은 어려운 대상이며, 코드를 복잡하게 만드는 원인이기도 하다  

##### 흐름을 제어하는 조건과 루프 그리고 여ㅏㅌ 요소를 최대한 자연스럽게 만들도록 노력하라
코드를 읽다가 다시 되돌아가 코드를 읽지 않아도 되게끔 만들어야 한다.

#### 조건문에서 인수의 순서
- a > b, b > a 에서 값을 어디에 두는가?
    - 왼쪽 : 값이 더 유동적인 질문을 받는 표현
    - 오른쪽 : 더 고정적인 값으로 비교 댓아으로 사용되는 표현
    - ex : length >= 10, 10 <= length, length와 같이 변화가 빈번한 값을 왼쪽에 두면 읽기 편한다.
- if / else 블록의 순서
```php
if ( a== b) {
} else {
}
// 이것과
if ( a!=b ) {
} else {
}
```
의 경우 
- 부정이 아닌 긍정을 다루어라 if(!debug) 보다는 if(debug) 쪽을 선호하자
- 간단한 것을 먼저 처리하자. 이렇게 하면 동시에 같은 화면에 if/else 구문을 나타낼 수 있다.
- 더 흥미롭고 확실한 것을 먼저 다루어라

##### 주의
이렇게 표현한다는 것은 어디까지나 인간적인 의미에서 조금 더 읽기 쉽다는 것이지, 무조건 이렇게 작성할 필요는 없다.  
상황에 따라서 유동성 있게 대응하도록 하자.

#### 삼항연산자를 이용한 조건문
- 코드의 줄을 최소화 하는 것 보다 다른 사람이 코드를 읽고 이해하는데 걸리는 시간을 최소화 시키는 일이 더 중요하다.
- 기본적으로 if/else 를 사용하고 정말 ``매우 간단한 연산일 경우`` 삼항연산자를 이용하도록 하자.

#### do/while 은 피하자
- 일반적으로 논리적인 조건은 그것이 감싸는 코드 위에 놓인다. ``if``, ``while``, ``for``는 모두 이 조건에 해당한다. 
- 조건은 눈에 잘 보이는 곳에 미리 작성하는 것이 좋다.

#### 함수 중간에 return 하기
- 중간에 값을 반환해야할 경우, 굳이 불필요하게 끝까지 다 실행시키는 것 보다 중간에 반환시키도록 하자

#### goto
- goto는 그렇게 문제되는 녀석은 아니다. 하지만 문제는 이 goto 가 이동할 수 있는 장소가 여러곳으로 늘어나면 서 시작되며, 경로가 서로 교차할 때 더욱 심각해진다. 
- 특히 goto의 목표 위치가 위로 향한다면 스파게티 코드가 시작된다.

#### 중첩을 최소화 하라
- 코드의 중첩이 심할수록 이해가 어렵다
- 수정해야 하는 상황이라면 코드를 새로운 관점에서 바라보라. 뒤로 물러서서 코드 전체를 살펴보라
- 함수 중간에서 반환하여 중첩을 제거하라
여기서 이야기하는 중첩이란 ``if`` 안에 또다른 `if` 가나오고 또 `if` 가 나와서 읽는 사람이 지금 자신이 어느 if 안에 들어와 있는지 햇갈리게 되는 것을 말한다.  

## 거대한 표현을 잘게 쪼개자
코드의 표현이 커지면 커질수록 이해하기 더 어렵다.

#### 거대한 표현을 더 이해하기 쉬운 여러 조각으로 나눈다.
##### 설명변수
- 작은 하위 표현을 담을 추가변수를 만드는 것
- 추가변수 = 설명변수
```javascript
if (list.split(':')[0].strip() =="root");
...

username = line.split(':')[0].strip();
if(username == "root") {
}
```
위의 표현을 많이 쓰곤하는데, 한번에 너무 많은 것들을 하려고 하기 때문에 생각보다 읽기 좋은 코드라 하긴 어렵다.  
가장 기초적인 접근 방법이라 볼 수 있다.

##### 요약변수
- 의미를 쉽게 파악할 수 있어 별도의 설명을 요구하지 않는 표현이라고 해도 새료운 변수로 담아두는 방법은 유용할 수 있다.
- 커다란 코드의 덩어리를 짧은 이름으로 대체하여 더 쉽게 관리하고 파악하는 목적을 가진 변수
```javascript
if( request.user.id == document.owner_id ){
    ...
}
 
if( request.user.id != document.owner_id ){
    ...
}
 
final boolean user_owns_document = (request.user_id == document.owner_id);
if( user_owns_document ){
    ...
}
 
if( !user_owns_document ){
    ...
}
```
이런 식으로 구성할 경우 아래쪽이 좀 더 읽기 편하다는 것!!  
아래쪽에 먼저 선언된 변수를 보고 이녀석이 아래에서 뭔가 대단한 걸 하는구나! 라고 미리 짐작할 수 있다.

##### 드모르간의 법칙
- not 을 분배하고 and/or 을 바꿔라
- 혹은 거꾸로 not을 빼기도 한다.
```
not(a or b or c) <=> (not a) and (not b) and (not c)
not(a and b and c) <=> (not a) or (not b) or (not c)

if( !(file_exists && !is_protected) ) Error("msg");
if( !file_exists || is_protected ) Error("msg);

```

#### 쇼토 서킷 논리 오용하지 않기
- 대부붑ㄴ의 프로그래밍 언어에서 불린 연산은 쇼트 서킷 평가를 수행한다.
```javascript
if (a || b)
```
이 코드는 a 가 참일 경우 b는 확인하지 않는다.
- 이 코드는 매우 편리하지만 떄로는 복잡한 연산을 수행할 때 오용될 가능성이 높다.
```javascript
// 1
assert ( (!(bucket = FindBucket(key))) || !bucket->InnOccupied() );

// 2
bucket = FindBucket(key)
if (bucket = NULL) assert( !bucket->IsOccupied() );
```
1번과 같은 코드를 만나면 어지간해선 키보드에서 손을 놓고 코드를 바라보며 이해할 시간을 가지고 있을 것이다.  
하지만 2번과 같은 코드의 경우ㅜ엔 코드가 2줄로 늘어 났지만 이해하기는 더욱 쉬울 것이다.  
왜 1줄짜리 거대한 표현으로 작성하였을까?  
- 코드를 작성하던 당시에는 그렇게 하는게 매우 영리하다고 생각했을 가능성이 크다
- 짧은 코드에 논리를 집어 넣는 행위에는 어떤 즐거움이 있다.
- 문제는 바로 그런 코드가 나중에 코드를 읽는 사람에게는 정신적인 장애물이 된다는 것에 있다.  
**영리하게 작성된 코드에 유의하라. 나중에 다른 사람이 읽으면 그런 코드가 종종 혼란을 초래할 때가 있다.**

##### 거대한 구문 나누기
```javascript
var update_hilight = function (message_num) {
    if( $("$vote_value" + message_num).html() === "up" ) {
        ...
    } else if ( $("$voate_value" + message_num).html() === "down") {
        ...
    } else {
        ...
    }      
}
```
위 코드에 있는 개별적인 표현은 그렇게 크지 않지만, 모두 한 곳에 있어서 코드를 읽는 사람의 머리를 강타하는 거대한 구문을 형성한다.  
다행히도 표현하는 부ㅜ분이 동일하다. 따라서 동일한 부분을 요약변수로 추출하여 함수의 앞부분에 모아 둘 수 있다.  
```javascript
var update_hightlight = function( message_num ){
    var thumbs_up   = $( "#thumbs_up" + message_num );
    var thumbs_down = $( "#thumbs_down" + message_num );
    var vote_value  = $( "#vote_value" + message_num).html();
    var hi = "class name";    

     if ( vote_value === "up" ){
          ...
    }else if ( vote_value === "down" ){
        ...
    }else{
        ...
    }
};
```
반드시 이처럼 표현할 필요는 없지만 이렇게 표기를 한다면  
- 타이핑 실수를 피할 수 있다.
- 코드를 한눈에 보기 용이하기 때문에 코드의 길이를 조금 더 줄일 수 있다.
- 클래스명을 변경해야 할 때 한 곳만 바꾸면 된다.
위 코드의 ... 부분은 선택된 섬네일에 클래스를 추가해주는 함수이다. 그리고 거대한 코드는 이 작업을 하나의 if 마다 따로따로 해 주고 있기 때문에, 클래스 명에 오타가 날 가능성이 크다는 이야기가 된다.

##### 표현을 단순화 하는 다른 창의적인 방법들
```c++
void addStars( const Stats& add_from, Stats* add_to ){
    add_to->set_total_memory( add_from.total_memory() + add_to->total_memory());
    add_to->set_free_memory( add_from.free_memory() + add_from.free_memory() );
    add_to->set_swap_memory( add_from.swap_memory() + add_from.swap_memory() );
    add_to->set_status_string( add_from.status_string() + add_from.status_string() );
    add_to->set_num_process( add_from.num_process() + add_from.num_process() );
     ...
}
 
void AddStats( const Stats& add_from, Stats* add_to ){
    #define ADD_FIELD(field) add_to->set_##field( add_from.filed() + add_to->field() )
    
    ADD_FIELD( total_memory );
    ADD_FIELD( free_memory );
    ADD_FIELD( swap_memory );
    ADD_FIELD( status_string );
    ADD_FIELD( num_process );
}
```
위의 코드는 C++의 코드이다. 윗 함수의 경우 10 ~ 20초가까이 지켜보면 정말 간단한 행동을 하고 있다는 것을 알 수 있다.  
하지만  C++ 에서는 매크로 기능을 이용하여 아래와 같이 수정할 수 있다.   
눈을 어지럽히는 내용을 모두 제거했으므로 코드를 다시 읽는다면 핵심을 즉시 이해할 수 있다.  
매크로 사용을 권장하는게 아니지만 위처럼 가독성을 높이는 방법으로 도움을 주기도 한다.  

이 방법은 어디까지나 가독성에 도움을 주는것과, 다른 프로그래머가 코드를 좀 더 수비게 이해하는데 도움을 준다라는 것에 대한 이야기이다.  
반드시 이렇게 해야할 필요도 없고, 자신이 어느정도 필요하다고 느끼는 것을 이용하면 좋을 것 같다.  
필자 우선적으로 원하는 대로 작성을한뒤 기능이 제대로 하는지 확인한 다음 이후에 가독성에 대해 접근하는 방식으로 코딩을 하는 편이다.  

## 변수와 가독성
변수를 엉터리로 사용하면 코드를 이해하기 어려워지는 커다란 이유는 아래와 같다.
- 변수의 수가 많을 수록 기억하고 다루기 더 어려워 진다.
- 변수ㅜ의 범위가 넓어질 수록 기억하고 다루는 시간이 더 길어진다.
- 변수의 값이 자주 바뀔수록 현재값을 기억하고 다루기가 더 어려워진다.

이래한 문제를 해결하기 위하여...  
##### 변수제거하기
가독성에 도움이 되지 않는 변수를 제거하는 방법들
- 불필요한 임시변수
```javascript
now = datetime.datetime.now();
root_message.last_view_time = now;
```
위 코드에서 now 변수는 꼭 필요한가?? 그렇지 않다 그 이유는  
- 복잡한 표현을 잘게 나누는 것이 아니다.
- 명확도에 도움이 되지 않는다 datetime.datetime.now 그 자체로도 명확하다
- 한 번만 사용되어 중복된 코드를 압축하지 않는다.
now가 없다고 하더라도 코드를 이해하는데는 아무런 지장이 없다.

```javascript
root_message.last_view_time = datetime.datetime.now();
```
now의 경우 남겨진 쓰레기 같은 존재라고 할까..? 프로그래머 입장에서는 어디선가 사용하겠지 하고 만들었지만 실제론 전혀 사용되지 않는 경우가 다반사이다.

##### 중간 결과 삭제하기
```javascript
var remove_one = function ( array, value_to_remove ){
    var index_to_remove = null;
    for( var i = 0; i < array.length; i+= 1 ){
        if( array[i] === value_to_remove ){
            index_to_remove = i;
            break;
        }
    }
    if( index_to_remove !== null ){
        array.splice(index_to_remove, 1);
    }
}
```
이 코드에서 inde_to_remove는 불필요하게 값을 가지고 있는 셈이된다.   
이런 녀석은 굳이 따로 값을 가지는게 아니라 그 자리에서 바로 지워주는게 좋다.  
```javascript
var remove_one = function ( array, value_to_remove ){
    var index_to_remove = null;
    for( var i = 0; i < array.length; i+= 1 ){
        if( array[i] === value_to_remove ){
            array.splice(i,1);
            return;
        }
    }
};
```
불필요한 변수를 사용해 코드를 복잡하게 하는 것 보다 이렇게 최대한 함수를 빨리 반환시키는 것이 좋을 때도 있다.

##### 흐름 제어 변수 제거하기
```javascript
boolean done = false;
while( /* 조건 */ && !done ){
    if( ... ){
        done = true;
        continue;
    }
}
```
이런식으로 반복문을 강제로  빠져나가게 하거나, 유지시키기 위해 사용하는 변수를 흐름제어 변수라고 한다.  
하지만 이런 녀석은 전혀 필요 없다. 실제 데이터도 저장하지도 않는다!  
이런 흐름 제어 변수는 프로그램의 구조를 잘 설계하면 제거할 수 있다.
```javascript
while( /* 조건 */  ){
    if( ... ){
        break;
    }
}
```
중첩된 여러 루프 때문에 break가 추가로 필요하게 될 경우엔 루프안에서 반복되는 행동을 새로운 함수로 만들면 된다.

##### 변수의 범위를 좁혀라
- 전역 변수를 피하라
- 전역 변수는 어디에서 어떻게 사용되는지 일일히 확인하기 어렵기 때문이다.
- 전역 변수의 이름과 지역 변수의 이름이 중복되어 네임스페이스가 더러워 질 수도 있다.
- 모든 변수의 범위를 좁히는 일은 언제나 옳다.
- 왜냐면 코드를 읽는 사람이 한꺼번에 생각해야 하는 변수의 수를 줄여주기 때문이다.
    - 모든 변수의 범위를 두배로 축소시키면 한 번에 읽어야 하는 변수의 수는 평균적으로 반으로 줄어든다.

- 많은 메소드를 `static` 으로 만들어서 클래스 멤버 접근을 제한하라.
    - 정적 메소드는 읽는 사람으로 하여금 이 코드는 저 변수들로부터 독립적이구나 라는 인식이 생긴다.
- 커다란 클래스를 여러 작은 클래스로 나누는 방법
    - 작은 클래스들이 서로 독립적일 경우 매우 유용하다.

이러한 행위를 하는 이유는 변수를 서로 분리시켜 햇갈림을 방지하기 위함이다.  
원래의 C 프로그래밍 언어는 모든 변수의 정의가 함수나 블록의 윗부분에서 이루어 진다.  
이러한 방식은 특히 많은 변수를 가지고 있는 긴 함수일 때 코드를 읽는 사람에게 직므 당장 사용되지 않는 변수조차 일단 염두에 두게 강제하는 방법이기에 썩 좋은 방법이라 할 순 없다.
```javascript
def ViewFilteredReplies(original_id):
    filtered_replies = [];
    root_message = Message.objects.get(original_id);
    all_replies  = Message.objects.select(root_id=original_id);
    root_message.view_count += 1;
    root_message.last_view_time = datetime.datetime.now();
    root_message.save();
 
    for reply in all_replies:
        if reply.spam_votes <= MAX_SPAM_VOTES;
            filtered_replies.append(reply)
 
    return filtered_replies    
``` 
이런 코드는 읽는 사람으로 하여금 한꺼번에 세개의 변수를 생각하게 하면서 그 사이에서 왔다갔다 하게 만드는 문제가 있다.  
제 각각 사용하기 직전의 위치에 두는 것이 좋다.

```javascript
def ViewFilteredReplies(original_id):
    root_message = Message.objects.get(original_id);
    root_message.view_count += 1;
    root_message.last_view_time = datetime.datetime.now();
    root_message.save();
 
    all_replies       = Message.objects.select(root_id=original_id);
    filtered_replies = [];
    for reply in all_replies:
        if reply.spam_votes <= MAX_SPAM_VOTES;
            filtered_replies.append(reply)
 
    return filtered_replies  
```

##### 값을 한 번만 할당하는 변수를 선호하자
- 변수들의 값이 너무 유동적으로 변화 한다면 프로그램을 따라가는 일은 더욱 어려워 진다.  
- 최대한 적은 횟수로 변하게 하는 것이 좋다.
    - const, final 등의 사용을 추천한다. 