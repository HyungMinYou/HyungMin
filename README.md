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
items.remove(at: (indexPath as NSIndexPath).row)
itemsImageFile.remove(at: (indexPath as NSIndexPath).row)
```
1. 선택한 셀을 삭제하는 코드입니다.<br><br><br>


```
self.navigationItem.leftBarButtonItem = self.editButtonItem
```
1. 바 버튼으로 목록 삭제 동작 코드이며 [edit] 버튼을 왼쪽에 추가하였습니다.<br><br><br>


```
let itemToMove = items[(fromIndexPath as NSIndexPath).row]
let itemImageToMove = itemsImageFile[(fromIndexPath as NSIndexPath).row]
items.remove(at: (fromIndexPath as NSIndexPath).row)
itemsImageFile.remove(at: (fromIndexPath as NSIndexPath).row)
items.insert(itemToMove, at: (to as NSIndexPath).row)
itemsImageFIle.insert(itemImageToMove, at: (to as NSIndexPath).row)
```
1. 이동할 아이템의 위치를 itemToMove에 저장합니다.<br>
2. 이동할 아이템의 이미지를 itemImageToMove에 저장합니다.<br>
3. 이동할 아이템을 삭제합니다. 이때 삭제한 아이템 뒤의 아이템들의 인덱스가 재정렬됩니다.<br>
4. 이동할 아이템의 이미지를 삭제합니다. 이때 삭제한 아이템 이미지 뒤의 아이템 이미지들의 인덱스가 재정렬됩니다.<br>
5. 삭제된 아이템을 이동할 위치로 삽입합니다. 또한 삽입한 아이템 뒤의 아이템들의 인덱스가 재정렬됩니다.<br>
6. 삭제된 아이템의 이미지를 이동할 위치로 삽입합니다. 또한 삽입한 아이템 이미지 뒤의 아이템 이미지들의 인덱스가 재정렬됩니다.<br><br><br>


```
@IBAction func btnAddItem(_ sender: UIButton) {
    items.append(tfAddItem.text!)                  //items에 텍스트 필드의 텍스트 값을 추가합니다.
    itemsImageFile.append("clock.png")             //itemsImageFile에는 무조건 'clock.png' 파일을 추가합니다.
    tfAddItem.text=""                              //텍스트 필드의 내용을 지웁니다.
    _ = navigationController?.popViewController(animated: true)         //테이블 뷰로 돌아갑니다.
```
1. 새 목록을 추가하는 코드입니다.<br><br><br>

























