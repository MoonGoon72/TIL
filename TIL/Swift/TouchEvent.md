# TouchEvent
## HitTest

- 서브뷰 배열에 있는 애들을 재귀적으로 찾는 것.
- 시작점을 찾아주는 일을 한다.
  - 터치 이벤트를 받을 뷰를 찾는 과정.

### 메서드

- touchesBegan(\_:with:)
- touchesMoved(\_:with:)
- touchesEnded(\_:with:)
- touchesCancelled(\_:with:)

hit-test view가 모든 메서드를 받는다.
## UIResponder

- 이벤트를 전달한다.
- Responder Chain을 통해 내가 이벤트를 처리하지 않으면 상위 Responder에게 이벤트를 전달한다.

## UIButton

- UIButton의 다섯 가지 상태
  - default : 기본
  - highlighted : 마우스를 가져다대서 강조된 상태
  - focused : 마우스를 안 가져다댔지만 버튼을 누를 수 있는 상태. 맥이나 아이패드에 사용됨
  - selected : 눌려진 상태
  - disabled : 비활성화 상태

## UIEvent
- UIEventType
  - UIEventTypeTouches : 터치
  - UIEventTypeMotion : 움직임 (e.g. 흔들기)
  - UIEventTypeReomteControl : 원격 물리 이벤트?
    - 에어팟을 눌렀을 때 이벤트를 커스텀해줄 수 있다.

## Responder Chain

- Chain Of Responsibility
  - 엄밀히 말하면 디자인 패턴 중 하나이다.
- Responder Chain을 따라 이벤트가 전달된다.
  - 특정 Responder 가 이벤트를 처리해도 다음 responder에게 넘겨줄 수 있다.

### Responder Chain이 끊어지는 경우

- UIScrollView를 상속 받은 뷰들
  - e.g. UITableView, UICollectionView
  - first responder여도 이벤트를 넘기지 않고 scrollview가 받아서 버려버린다.
  - 상위 responder로 이벤트를 전달해주고자 한다면 따로 상속해주어 next에 전달해주도록 해야한다.
- Touch Event를 무시하는 경우 (disable isUserInteractionEnabled)
- 뷰가 안 보이는 경우
  - clipToBounds
    - 잘려서 안보이는 영역은 responder x
  - hidden
    - 숨겨지니 안보임
  - alpha값이 매우 작은 경우 -> Button의 경우 0.05보다 작으면 안 눌린다.
