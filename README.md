# LED-display-App
경기장이나 노래방 또는 콘서트에서 쓸 수 있는 led 글자 앱
<br>
### 활용 기술
UINavigationController<br>
화면 전환 <br>
ViewController Life Cycle<br>
화면간 데이터 전달 방법<br>
에셋 카탈로그<br>
<br>
### UINavigationController
Content View Controller<br>
화면을 구성하는 뷰를 직접 구현하고 관련된 이벤트를 처리하는 뷰 컨트롤러<br>
(storyboard생성시 기본으로 생성되는 뷰컨트롤러)<br>
Container View Controller<br>
하나 이상의 child view controller를 갖고 있다<br>
하나 이상의 child view controller를 관리하고 레이아웃과 화면 전환을 담당한다.<br>
화면 구성과 이벤트 관리는 child view controller에서 한다.<br>
대표적으로 Navigation Controller와 TabBar Controller가 있다.<br>
<br>
## UINavigationController
계층구조로 구성된 content를 순차적으로 보여주는 container view controller<br>
ex) 설정 앱<br>
Navigation 스택으로 자식 view controller를 관리한다<br>
Navigation Stack은 일반적인 스택과 같이 LIFO 구조를 하고 있다.<br>
설정 앱에서 항목을 선택하면 그 child view controller가 스택에 쌓이는 구조이다.<br>
반대로 백버튼이나 left edge swipe gesture로(왼쪽 당김 제스쳐) 이전 화면으로 돌아가면<br>
스택에서 방금 전 보고 있던 뷰 컨트롤러가 pop된다.<br>
<br>
## Navigation Bar
Navigation Controller를 구현할 시 화면 상단에 항상 보여지는 뷰<br>
루트 뷰 컨트롤러 외에 모든 뷰 컨트롤러에 백 버튼이 있어<br>
유저가 계층구조에서 이전 화면으로 돌아갈 수 있도록 해준다.<br>
<br>
### 화면 전환
ios에서 화면을 전환하는 방식에는 크게 두 가지가 있다<br>
소스코드를 통해 전환하는 방식<br>
스토리보드를 통해 전환하는 방식<br>
조금 더 세부적으로 보자면<br>
-View Controller의 View 위에 다른 View를 가져와 바꿔치기 : 이건 되도록 사용하지 말아야 한다. 뷰 위에 다른 뷰를 가져오는 건 메모리 누수 위험을 안고 간다.<br>
-View Controller에서 다른 View Controller를 호출하여 전환하기<br>
-Navigation Controller를 사용하여 화면 전환하기<br>
-segueway(화면 전환용 객체)를 이용하여 화면 전환하기<br>
<br>
## View Controller에서 다른 View Controller를 호출하여 전환하기
presentation방식이라고 부르기도 한다.<br>
기존 뷰 컨트롤러 위에 새로운 뷰 컨트롤러를 가져와 덮는 방식이다<br>
<br>
func present(_ viewControllerToPresent: UIViewController, animated flag: Bool, completion: (() -> Void)? = nil)<br>
<br>
위에 보이는 present 메소드에 이동할 화면의 뷰 컨트롤러를 넘겨주면 이전 화면에서 이동할 뷰 컨트롤러가 표시된다<br>
present 메소드의 첫번째 파라미터에는 새로운 화면으로 이동할 화면의 뷰 컨트롤러 인스턴스를 넣어주면 되고<br>
두 번째 파라미터는 애니메이션 효과를 boolean값으로 준다<br>
세 번째 파라미터에선 completion이란 클로저를 전달받고 있는데 이 completion이란 파라미터에 클로저를 정의해주면 화면 전환이 완료되는 시점에 맞춰 completion 클로저가 호출되게 된다.<br>
화면 전환 방식은 비동기 방식으로 처리되기 때문에 화면 전환이 완료된 이후에 무언가 처리하고 싶은 것이 있다면 이 클로저에 작성하면 된다.<br>
<br>
func dismiss(animated flag: Boll, completion: (() -> Void)? = nil)<br>
<br>
dismiss 메소드는 이전화면으로 돌아가게 해주는 메소드이다<br>
그저 이전 화면으로 돌아가는 용도라 뷰 컨트롤러의 인스턴스를 인자로 받지 않는다.<br>
그래서 첫 번째 파라미터는 애니메이션 설정, 두 번째는 completion 클로저로 구성되어있다.<br>
즉 화면을 걷어내는 방식으로 화면 전환을 처리하는 메소드라고 보면 된다.<br>
<br>
## Nacigatoin Controller를 사용하여 화면 전환하기
Navigation Controller는 계층적인 뷰 컨트롤러 구조를 관리하기 위한 컨트롤러이다.<br>
앞에서 네이게이션 스택으로 자식을 관리한다고 했다.<br>
<br>
func pushViewController(_ viewController: UIViewController, animated: Bool)<br>
<br>
func popViewController(animated: Bool) -> UIViewController?<br>
<br>
위에서 보이듯이 pushViewController메소드를 사용해 네비게이션 스택에 자식을 push하고<br>
popViewController 메소드를 이용해 네비게이션 스택에서 pop한다.<br>
<br>
## 화면 전환용 객체 세그웨이(segueway)를 이용해 화면 전환하기
<br>
세그웨이는 두 개의 뷰 컨트롤러 사이에 연결된 화면 전환 객체를 가리킨다<br>
스토리보드를 통해 출발지와 목적지를 직접 지정하는 방식이다.<br><br>
세그웨이를 사용하면 따로 코드를 사용하지 않아도 스토리보드 조작만으로 화면을 전환할 수 있다.<br>
세그웨이의 종류에는 Action Segueway와 Manual Segueway가 있다.<br>
출발점이 뷰 컨트롤러 자체인 경우를 메뉴얼 세그웨이라고 하고, 출발점이 버튼 셀 등이면 액션 세그웨이, 트리거 세그웨이라고 나누어 부른다<br>
액션 세그웨이는 버튼 터치 같은 액션 이벤트가 트리거 실행으로 바로 연결된다.<br>
메뉴얼 세그웨이는 적절한 시점에 perform segue라는 메소드를 호출하면서 세그웨이가 실행되어 화면 전환이 일어난다.<br>
<br>
Action Segueway 종류<br>
<br>
액션 세그웨이는 여러 종류가 있다.<br>
Show: 가장 일반적인 세그웨이, 네비게이션 컨트롤러를 사용하면 화면 전환 시 뷰 컨트롤러가 네비게이션 스택에 쌓이게 되고, 만약 네비게이션 컨트롤러를 사용하지 않을 경우엔 뷰 컨트롤러가 present 된다<br>
Show Detail: Split 뷰에서 사용하는 세그웨이로 아이폰에서 사용하게 되면 Show segueway와 동일하게 작동하지만, 아이패드에서 사용하면 split 구조로 보이게 된다.<br>
Present Modally: 이전 뷰 컨트롤러를 덮으면서 새로운 화면이 나타난다 (앞에서 이야기한 presentation 방식으로 작동한다)<br>
Present As Popover: 아이패드에서 사용되는 것으로 팝업창을 띄울 때 사용(아이폰에서는 사용 안 됨)<br>
Custom: 세그웨이를 사용자가 원하는 방식으로 커스텀할 때 사용한다.<br>






