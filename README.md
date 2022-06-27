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
