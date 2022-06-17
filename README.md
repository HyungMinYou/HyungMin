# 12장 <테이블 뷰 컨트롤러>
- 테이블 뷰 컨트롤러는 사용자에게 목록 형태의 정보를 제공해 줄 뿐만 아니라 목록의 특정 항목을 선택하여 세부 사항을 표시할 때 유용합니다.
  대표적인 앱으로 알람, 메일, 연락처가 있습니다.
- 데이터를 목록 형태로 보여 주고 관리하는 앱을 테이블 뷰 컨트롤러를 이용하여 쉽게 만들 수 있습니다.
<br><br>

코드 설명
-------------------------
```
@IBOutlet var tvListView: UITableView!
@IBOutlet var tfAddItem: UITextField!
@IBAction func btnAddItem(_ sender: UIButton)
@IBOutlet var lblItem: UILabel!
```
1. 테이블 뷰 컨트롤러의 클래스 선언문 바로 아래에 아웃렛 변수의 이름과 유형을 설정합니다.<br>
2. 애드 뷰 컨트롤러의 클래스 선언문 바로 아래에 아웃렛 변수의 이름과 유형을 설정합니다.<br>
3. 버튼에 액션을 추가할 것이므로 Action을 선택한 후 이름과 유형을 설정합니다.<br>
4. 디테일 뷰 컨트롤러의 클래스 선언문 바로 아래에 아웃렛 변수의 이름과 유형을 설정합니다.<br><br><br>


```
import UIKit

var items = ["책 구매", "철수와 약속", "스터디 준비하기"]
var itemsImageFile = ["cart.png", "clock.png", "pencil.png"]

class TableViewController: UITableViewController {
```
1. 추가한 이미지 파일을 외부 변수인 'items'와 'ItemsImageFile'로 선언합니다.<br>
2. 외부 변수를 선언하면 모든 클래스에서 이미지를 사용할 수 있습니다.<br><br><br>


```

```
