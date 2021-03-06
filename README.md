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
주의해야 할 점은 left edge motion으로 이전화면으로 돌아갈 수 없다는 점이다. 오직 stack 즉 네비게이션 스택 방식으로 구현해야 left edge 모션을 이용할 수 있다.
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
## Navigation Controller를 사용하여 화면 전환하기
Navigation Controller는 계층적인 뷰 컨트롤러 구조를 관리하기 위한 컨트롤러이다.<br>
앞에서 네이게이션 스택으로 자식을 관리한다고 했다.<br>
<br>
func pushViewController(_ viewController: UIViewController, animated: Bool)<br>
<br>
func popViewController(animated: Bool) -> UIViewController?<br>
<br>
위에서 보이듯이 pushViewController메소드를 사용해 네비게이션 스택에 자식을 push하고<br>
popViewController 메소드를 이용해 네비게이션 스택에서 pop한다.<br>
주의) View Controller를 인스턴스화 하는 방법은 guard let viewController = self.storyboard?.instantiateViewController(identifier: "스토리보드지정식별자" ) else {return}
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

### ViewController Life Cycle
UIViewController의 객체에는 뷰 객체를 관리하는 메소드들이 있다.<br>
이 메소드들은 각자 자신들이 불려져야 하는 타이밍에 ios 시스템에 의해 자동으로 호출된다.<br>
UIViewController의 high 클래스를 생성할 때, 이 메소드들을 오버라이드하여 라이프 사이클 상황에 맞게 적절한 로직들을 추가할 수 있다.<br>
뷰가 보여지는 상황은 크게 네가지로 분리할 수 있다.<br>
-Appearing: 뷰가 화면에 나타나고 있다<br>
-Apppeard: 뷰가 화면에 나타났다<br>
-Disappearing: 뷰가 화면에서 사라지고 있다<br>
-Disappeared: 뷰가 화면에서 사라짐<br>


# viewDidLoad()
뷰 컨트롤러의 모든 뷰들이 메모리에 로드됐을 때 호출<br>
메모리에 처음 로드될 때 한 번만 호출<br>
보통 딱 한 번 호출될 행위들을 이 메소드 안에 정의 함<br>
뷰와 관련된 추가적인 초기화 작업, 네트워크 호출<br>
<br>
# viewWillAppear()
뷰가 뷰 계층에 추가되고, 화면에 보이기 직전에 매 번 호출<br>
다른 뷰로 이동했다가 돌아오면 재호출<br>
뷰와 관련된 추가적인 초기화 작업<br>
<br>
# viewDidAppear()
뷰 컨트롤러의 뷰가 뷰 계층에 추가된 후 호출<br>
뷰를 나타낼 때 필요한 추가 작업<br>
애니메이션을 시작하는 작업<br>

# viewWillDisappear()
뷰 컨트롤러의 뷰가 뷰 계층에서 사라지기 전에 호출합니다<br>
뷰가 생성된 뒤 작업한 내용을 되돌리는 작업<br>
최종적으로 데이터를 저장하는 작업<br>

# viewDidDisappear()
뷰 컨트롤러의 뷰가 뷰 계층에서 사라진 뒤에 호출<br>
뷰가 사라지는 것과 관련된 추가 작업<br>
<br>

### 화면 간 데이터 전달 방법
instantiateViewController 사용처에서 이동할 뷰 컨트롤러로 다운캐스팅하면 (guard문 이용)<br>
이동할 뷰 컨트롤러의 멤버에 접근할 수 있다.<br>
이전화면으로 데이터 전달은 delegate 패턴 이용<br>
delegate 패턴은 ios에서 자주 사용되는 디자인 패턴<br>
delegate는 위임하다라는 사전적 의미를 갖고 있다.->위임자라고 생각하자!<br>
위임자를 갖고 있는 객체가 다른 객체에게 자신의 일을 위임하는 디자인 패턴이다<br>
<br>
데이터를 전달할 화면에 프로토콜을 하나 정의한다<br>
<br>
protocol SendDataDelegate: AnyObject {
  func sendData(name: String)
  }
  <br>
  weak var delegate: SendDataDelegate?<br>
  <br>
  delegate 변수도 작성해 준다(weak반드시 붙여주기: 강한 순환 참조 발생할 수도 있어서)<br>
  <br>
  이전화면으로 이동하는 버튼 액션 함수에다 sendData 함수를 작성해준다<br>
<br>
  
  @IBAction func tapBackButton(_ sender: UIButton) {
    self.delegate?.sendData(name: "Jinyong")
    self.presentingViewController?.dismiss(animated: true, completion: nil)
      }
    }

이렇게 코드를 작성하면 데이터를 전달받은 뷰 컨트롤러에서 sendData 프로토콜을 채택하고 delegate를 위임받게 되면 sendDatadelegate 프로토콜을 채택한 이전화면에서 <br>
정의된 sendData함수가 실행되게 된다. 그럼 이전 화면 뷰 컨트롤러에서 delegate를 위임받아보겠다<br>
<br>
이전 화면이 ViewController 클래스이면 <br>


import UIKit

class ViewController: UIViewController, SendDataDelegate {

.
.
.

 @IBAction func tapCodePresentButton(_ sender: UIButton) {
    guard let viewController = self.storyboard?.instantiateViewController(identifier: "CodePresentViewController") as? CodePresentViewController else {return}
    viewController.modalPresentationStyle = .fullScreen
    viewController.name = "Jinyong"
    viewController.delegate = self
    self.present(viewController, animated: true, completion: nil)
    }
    
    func sendData(name: String) {
    self.nameLabel.text = name
    self.nameLabel.sizeToFit()
    }
    .
    .
    .
    
    }
    
    <br>
## 세그웨이로 구현된 화면 전환에서 데이터 전달하기<br>
마찬가지로 전환되는 뷰 컨트롤러 인스턴스에 접근해 프로퍼티로 데이터를 전달한다<br>
세그웨이로 구현된 화면전환 방법에서 전환되는 화면의 값을 전달하기 위한 제일 좋은 위치는 전처리 prepare 메소드이다.<br>
prepare 메소드는 오버라이드 하면 세그웨이를 실행하기 직전에 시스템에 의해 자동으로 호출되기 때문이다.<br>
<br>


override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
if let viewController = segue.destination as? SeguePushViewController {
     viewController.name = "전달하려는 값" // name은 그쪽 프로퍼티
   }
}

---> 전달된 쪽<br>


@IBOutlet weak var nameLabel: UILabel!
var name: String?

override func viewDidLoad() {
  super.viewDidLoad()
  print("SeguePushViewController 뷰가 로드 되었다")
  if let name = name {
    self.nameLabel.text = name
    self.nameLabel.sizeToFit()
       }
     }

## 발생한 오류
SettingViewController의 class 연결 중 inherit module from target을 클릭 안해서 
Thread 1: Exception: "[<UIViewController 0x7f9832207800>
이런 이상한 에러가 떴다
알아내느라 한참 걸렸다. 
그리고 설정 화면의 설정 값이 유지가 안된다는 문제가 있었는데
관련 코드를 더 작성해 해결했다.

### 완성본

![스크린샷 2022-06-27 오후 11 52 36](https://user-images.githubusercontent.com/102133961/175970739-c0ff8688-7e06-46d0-acc4-1470f1a448a0.jpg)<br>
![스크린샷 2022-06-27 오후 11 52 49](https://user-images.githubusercontent.com/102133961/175970766-4fe9540d-f2bf-45ed-8e5a-58f44690b484.jpg)<br>
![스크린샷 2022-06-27 오후 11 53 34](https://user-images.githubusercontent.com/102133961/175970791-b1524896-2d04-45f1-8522-9cb8a882489a.jpg)<br>
![스크린샷 2022-06-27 오후 11 53 39](https://user-images.githubusercontent.com/102133961/175970818-f5d95f41-0fa8-4b9a-8570-969244f00918.jpg)<br>
![스크린샷 2022-06-27 오후 11 53 48](https://user-images.githubusercontent.com/102133961/175970838-570e6dbf-ac7c-4dd0-a56f-dcbf91ec7b9b.jpg)<br>
