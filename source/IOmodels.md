# Blocking / Non Blocking과 Sync / Async

  NOn blocking, Blocking, Sync 와 Async I/O에 대한 각각의 개념을 읽다 보면 모호한 부분이 생기게 된다. 우리는 우선 이분법적으로 생각하기를 좋아하기 때문에 Non blocking과 Blocking 그리고 Sync와 Async가 각각 대칭에 있다는 것을 쉽게 알 수 있다. 이때 주의해야할 점은 Blocking == Sync / Non Blocking == Async 라고 생각하기 쉽다는 점이다. 이 글에서 우선 네가지 개념에 대해서 알아보고 그 사이에 연관관계를 알아보는 시간을 갖도록 하자

## AIO

![](./AIO_figure1.gif)

위 그림은 IBM의 article [Boost application performance using asynchronous I/O](https://developer.ibm.com/articles/l-async/) 에서 AIO에 대해서 설명하기 위해 사용된 도표이다. 각 축을 보면 알 수 있듯이 Blocking == Sync / Non Blocking == Async이 아님을 알 수 있다. Syncronous는 Blocking과 Non-Blocking의 특징과 만나 다른 I/O 모델을 만들게 된다. 이 글의 요지는 이 글이 씌어진 당시 Syncrounous/Blocking의 특징을 가지고 있는 모델이 주로 이루지만 비효율 적이기 때문에 AIO(Asycronous I/O)라 불리는 효율적인 I/O 모델로 가야한다 라는 요점이다. 10년이상이 지난 현재 대부분 AIO 모델을 사용하고 있다는 것이 놀랍다. 



## Blocking I/O

Blocking은 호출한 쪽이 호출된 쪽에 의해서 다른 일을 할 수 없는 상태를 말한다. 호출하는 쪽에서 호출을 한뒤에 결과가 돌아 올 때까지 아무런 일을 못하고 기다리게 된다.

## Non Blocking I/O

Non Blocking은 호출한 쪽이 호출된 된 쪽이 결과 값을 주기전데 다른 일을 할 수 있는 상태를 의미한다. Blocking에 비해 훨씬 자원을 효율적으로 사용하는 경우이다.

## Syncronous I/O

Syncronous와 Asyncronous를 구별하는 차이는 결과 값을 누가 제어하는 가이다. Syncronous의 경우 호출한 쪽에서 결과 값을 요구하는 경우를 의미한다.

## Asyncronous I/O

Asyncronous의 경우 Syncronous와 달리 호출된 쪽에서 결과값을 알려주는 방식이다.



## 관점의 차이

각각 Blocking/Nonblocking 과 Syncronous/Asyncronous는 닮아 보이지만 관점에 차이가 있다. Blocking/Nonblocking은 호출한 쪽이 호출후 결과를 기다리는지 안기다리는지를 중점으로 구별하고 Syncronous/Asyncronous는 요청한 결과값을 호출된 쪽에서 알아서 알려주는지 호출한 쪽에서 다시 요청해야 알려주는지에 관점을 두고 있다.



 IBM에서 I/O 모델에 대한 Article을 작성한지 10년이 넘게 지났고 이미 AIO에 대해 익숙해진 탓에 각각의 개념을 이해하기 힘들었을 수도있다. 이제는 너무 일반화 되어서 NonBlocking과  Asyncronous의 개념을 붙여서 생각하는 것이 너무 익숙하기 때문이다. 하지만 각각의 개념에 차이점이 있으니 도표에 나와있는 모델들을 각각 알아보도록 하자



## Blocking - Syncronous

![](./AIO_figure2.gif)

 두개념을 붙여 생각하는 것은 당연하다. 요청한 쪽은 요청된 쪽에서 결과 값을 줄 때까지 아무런 작업을 하지 않고 기다리고 있는다.



## Nonbloking - Syncronous

![](./AIO_figure3.gif)

 요청한 쪽은 요청후에 다른 작업을 할 수 있다. 그러나 요청받은 쪽에서 결과 값을 돌려주지 않기 때문에 주기적으로 리턴 값을 확인하는 작업을 거쳐야 한다. 이전에 설명했던 작업보다는 효율 적이지만 그래도 비효율 적인 면이 있다. 요청한쪽에 리턴값이 준비되었는지 확인하는 요청을 polling 기법이라고 한다.



## Blocking - Asyncronous

![](./AIO_figure4.gif)

 어찌보면 Syncronous - blocking과 별차이 없지만, 요청받은 쪽에서 요청을 받은 직후 요청된 쪽에 신호를 보낸다는 점이 다르다. 그 후 결과 값이 준비되면 요청한 쪽에 알리지만, 요청한 쪽은 그 때까지 블로킹되어 있기 때문에 Syncronous - blocking과 성능 상으로 변반 차이가 없다.



## Asyncornous - Non Blocking

![](./AIO_figure5.gif)

 가장 효율적인 방법이다. 요청한 쪽은 요청직후에 다른 작업을 수행한다. 지속적으로 결과값이 나왔는지 요청된 쪽에 물어볼 필요없다. 요청된 쪽에서 요청을 받으면 확인 메세지를 보내고 결과값을 찾는 과정을 거친다. 그리고 결과 값을 찾으면 다시 알려주기 때문이다. 우리는 이를 Callback으로 익히 알고 있다.

 





- https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
- https://tech.peoplefund.co.kr/2017/08/02/non-blocking-asynchronous-concurrency.html
- https://musma.github.io/2019/04/17/blocking-and-synchronous.html
- https://developer.ibm.com/articles/l-async/

