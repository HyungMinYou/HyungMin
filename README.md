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

---------------------------
```
import UIKit

var items = ["책 구매", "철수와 약속", "스터디 준비하기"]
var itemsImageFile = ["cart.png", "clock.png", "pencil.png"]

class TableViewController: UITableViewController {
```
1. 추가한 이미지 파일을 외부 변수인 'items'와 'ItemsImageFile'로 선언합니다.<br>
2. 외부 변수를 선언하면 모든 클래스에서 이미지를 사용할 수 있습니다.<br><br><br>

---------------------------
```
items.remove(at: (indexPath as NSIndexPath).row)
itemsImageFile.remove(at: (indexPath as NSIndexPath).row)
```
선택한 셀을 삭제하는 코드입니다.<br><br><br>

-----------------------------
```
self.navigationItem.leftBarButtonItem = self.editButtonItem
```
바 버튼으로 목록 삭제 동작 코드이며 [edit] 버튼을 왼쪽에 추가하였습니다.<br><br><br>

--------------------------------
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

-----------------------------------
```
@IBAction func btnAddItem(_ sender: UIButton) {
    items.append(tfAddItem.text!)                  //items에 텍스트 필드의 텍스트 값을 추가합니다.
    itemsImageFile.append("clock.png")             //itemsImageFile에는 무조건 'clock.png' 파일을 추가합니다.
    tfAddItem.text=""                              //텍스트 필드의 내용을 지웁니다.
    _ = navigationController?.popViewController(animated: true)         //테이블 뷰로 돌아갑니다.
```
새 목록을 추가하는 코드입니다.<br><br><br>


-------------------------------
```
if segue.identifier == "sgDetail" {
    let cell = sender as! UITableViewCell
    let indexPath = self.tvListView.indexPath(for: cell)
    let detailView = segue.destination as! DetailViewController
    detailView.receiveItem(items[((indexPath as NSIndexPath?)?.row)!])
    }
```
세그웨이를 이용하여 뷰를 전환하는 것과 같은 방법을 사용합니다.<br><br><br><br><br><br><br>



# 13장 < AVAudioPlayer >
- iOS에서는 기본적으로 음악 재생 앱과 녹음 앱을 제공합니다. 오디오 파일을 재생할 수 있다면 벨소리나 알람과 같이 각종 소리와 관련된 다양한 작업을 할 수 있습니다.
- 또한 일정 관리 앱에 녹음 기능을 추가해 목소리로 메모를 하는 등 메인 기능이 아닌 서브 기능으로도 사용할 수 있습니다. 오디오를 재생하는 방법 중 가장 쉬운 방법은 AVAudioPlayer를 사용하는 것입니다.<br><br>


코드 설명
-------------------------
```
import UIKit
import AVFoundation

class ViewController: UIViewController, AVAudioPlayerDelegate{
```
오디오를 재생하려면 헤더 파일과 델리게이트가 필요하므로 'AVFoundation'을 불러오고, 'AVAudioPlayerDelegate' 선언을 추가합니다.<br>

---------------------
```
var audioPlayer : AVAudioPlayer!          //AVAudioPlayer 인스턴스 변수
var audioFile : URL!                      //재생할 오디오의 파일명 변수
let MAX_VOLUME : Float = 10.0             //최대 볼륨, 실수형 상수
var progressTimer : Timer!                //타이머를 위한 변수
```
<br><br><br>


---------------------
```
audioFile = Bundle.main.url(forResource: " ", withExtension: "mp3")
```
추가한 오디오 파일명의 .mp3를 뺀 파일명을 " " 안에 입력합니다.<br><br><br>

-----------------
```
func initPlay() {
}
```
오디오 재생을 초기화하는 함수입니다.<br><br><br>

--------------------
```
slvolume.maximumValue = MAX_VOLUME
slvolume.value = 1.0
pvProgressPlay.progress = 0

audioPlayer.delegate = self
audioPlayer.prepareToPlay()
audioPlayer.volume = slVolume.value
```
1. 슬라이더의 최대 볼륨을 상수 MAX_VOLUME인 10.0으로 초기화합니다.
2. 슬라이더의 볼륨을 1.0으로 초기화합니다.
3. 프로그레스 뷰의 진행을 0으로 초기화합니다.
4. audioPlayer의 델리게이트를 self로 합니다.
5. prepareToPlay()를 실행합니다.
6. audioPlayer의 볼륨을 방금 앞에서 초기화한 슬라이더의 볼륨 값 1.0으로 초기화합니다.<br><br><br>

-------------------
```
func convertNSTimeInterval2String(_ time:TimeInterval) -> String {              // "00:00" 형태로 바꾸기 위해 TimeInterval 값을 받아 문자열로 돌려보내는 함수
    let min = Int(time/60)
    let sec = Int(time.truncatingRemainder(dividingBy: 60))
    let strTime = String(format: "%02d:%02d", min, sec)
    return strTime
```
1. 재생 시간의 매개변수인 time 값을 60으로 나눈 '몫'을 정수 값으로 변환하여 상수 min 값에 초기화합니다.
2. time 값을 60으로 나눈 '나머지' 값을 정수 값으로 변환하여 상수 sec 값에 초기화합니다.
3. 이 두 값을 활용해 "%02d:%02d" 형태의 문자열(String)로 변환하여 상수 strTime에 초기화합니다.
4. 이 값을 호출한 함수로 돌려보냅니다.<br><br><br>

------------------
```
audioPlayer.play()
setPlayButtons(false, pause: true, stop: true)
```
1. audioPlayer.play 함수를 실행해 오디오를 재생합니다.
2. [Play] 버튼은 비활성화, 나머지 두 버튼은 활성화합니다.<br><br><br>

-----------------
```
audioPlayer.pause()
setPlayButtons(true, pause: false, stop: true)
```
1. audioPlayer.pause 함수를 실행해 오디오를 잠시 멈춥니다.
2. [Pause] 버튼은 비활성화, 나머지 두 버튼은 활성화합니다.<br><br><br>

-----------------
```
audioPlayer.stop()
setPlayButtons(true, pause: false, stop: false)
```
1. audioPlayer.stop 함수를 실행해 오디오를 정지합니다.
2. [Play] 버튼은 활성화, 나머지 두 버튼은 비활성화합니다.<br><br><br>

-------------------
```
let timePlayerSelector:Selector = #selector(ViewController.updatePlayTime)
```
1. 재생 타이머를 위한 상수를 추가합니다.<br>
*# selector 중요 (함수안에 다른 함수를 호출하고 싶을 때 사용* <br><br><br>

----------------
```
audioPlayer.volume = slVolume.value
```
볼륨 조절하는 코드이며 화면의 슬라이더를 터치해 좌우로 움직이면 볼륨이 조절되도록 합니다.<br><br><br>

---------------
```
var audioRecorder : AVAudioRecorder!
var isRecordMode = false
```
1. audioRecorder 인스턴스를 추가합니다.
2. 현재 '녹음 모드'라는 것을 나타낼 isRecordMode를 추가합니다. 기본값은 false로 하여 처음 앱을 실행했을 때 '녹음 모드'가 아닌 '재생 모드'가 나타나게 합니다.<br><br><br>

---------------
```
func selectAudioFile() {
    if !isRecordMode {
      audioFile = Bundle.main.url(forResource: " ", withExtension: "mp3")
    } else {
       let documentDirectory = FileManager.default.urls
          (for: .documentDirectory, in: .userDomainMask)[0]
       audioFile = documentDirectory.appendingPathComponent
          ("recordFile.m4a")
    }
}
```
1. 재생 모드일 때는 오디오 파일인 " "가 선택됩니다.
2. 녹음 모드일 때는 새 파일인 "recordFile.m4a"가 생성됩니다.<br><br><br>

----------------
```
override func viewDidLoad() {
    super.viewDidLoad()
    selectAudioFile()
    if !isRecordMode {
        initPlay()
        btnRecord.isEnabled = false
        lblRecordTime.isEnabled = false
    } else {
        initRecord()
    }
}
```
1. if문의 조건이 '!isRecordMode'입니다. 이는 '녹음 모드가 아니라면'이므로 재생 모드를 말합니다. 따라서 initPlay 함수를 호출합니다.
2. 조건에 해당하는 것이 재생 모드이므로 [Record] 버튼과 재생 시간은 비활성화로 설정합니다.
3. 조건에 해당하지 않는 경우, 이는 '녹음 모드라면'이므로 initRecord 함수를 호출합니다.<br><br><br>

------------------
```
func initRecord() {
    let recordSettings = [
        AVFormatIDKey : NSNumber(value: kAudioFormatAppleLossless as UInt32),
        AVEncoderAudioQualityKey : AVAudioQuality.max.rawValue,
        AVEncoderBitRateKey : 320000,
        AVNumberOfChannelsKey : 2,
        AVSampleRateKey : 44100.0] as [String : Any]
     do {
          audioRecorder = try AVAudioRecorder(url: audioFile, settings: recordSettings)
     } catch let error as NSError {
          print("Error-initRecord : \(error)")
     }
     
     audioRecorder.delegate = self
     audioRecorder.isMeteringEnabled = true
     audioRecorder.prepareToRecord()
     
     slVolume.value = 1.0
     audioPlayer.volume = slVolume.value
     lblEndTime.text = convertNSTimeInterval2String(0)
     lblCurrentTime.text = convertNSTimeInterval2String(0)
     setPlayButtons(false, pause: false, stop: false)
     
     let session = AVAudioSession.sharedInstance()
     do {
          try session.setCategory(AVAudioSessionCategoryPlayAndRecord)
     } catch let error as NSError {
          print(" Error-setCategory : \(error)")
     }
     do {
          try session.setActive(true)
     } catch let error as NSError {
          print(" Error-setCategory : \(error)")
     }
}
```
1. 포맷은 'Apple Lossless', 음질은 '최대', 비트율은 '320,000bps', 오디오 채널은 '2'로 하고 샘플률은 '44.100Hz'로 설정합니다.
2. selectAudioFile 함수에서 정한 audioFile을 URL로 하는 audioRecorder 인스턴스를 생성합니다. 이때 try-catch문을 사용합니다. 마지막에 audioRecorder의 델리게이트를 설정합니다.
3. AudioRecorder의 델리게이트를 self로 설정합니다.
4. 박자 관련 isMeteringEnabled 값을 참으로 설정합니다.
5. prepareToRecord 함수를 실행합니다.
6. 볼륨 슬라이더 값을 1.0으로 설정합니다.
7. audioPlayer의 볼륨도 슬라이더 값과 동일한 1.0으로 설정합니다.
8. 총 재생 시간을 0으로 바꿉니다.
9. 현재 재생 시간을 0으로 바꿉니다.
10. [Play], [Pause] 및 [Stop] 버튼을 비활성화로 설정합니다.<br><br><br>


------------------
```
if sender.isOn {
   audioPlayer.stop()
   audioPlayer.currentTime = 0
   lblRecordTime!.text = convertNSTimeInterval2String(0)
   isRecordMode = true
   btnRecord.isEnabled = true
   lblRecordTime.isEnabled = true
} else {
   isRecordMode = false
   btnRecord.isEnabled = false
   lblRecordTime.isEnabled = false
   lblRecordTime.text = convertNSTimeInterval2String(0)
}
selectAudioFile()
if !isRecordMode {
   initPlay()
} else {
   initRecord()
}
```
1. 스위치가 [On]이 되었을 때는 '녹음 모드'이므로 오디오 재생을 중지하고, 현재 재생 시간을 00:00으로 만들고, isRecordMode의 값을 참으로 설정하고, [Record] 버튼과 녹음 시간을 활성화로 설정합니다.
2. 스위치가 [On]이 아닐 때, 즉 '재생 모드'일 때는 isRecordMode의 값을 거짓으로 설정하고, [Record] 버튼과 녹음 시간을 비활성화하며, 녹음 시간은 0으로 초기화합니다.
3. selectAudioFile 함수를 호출하여 오디오 파일을 선택하고, 모드에 따라 초기화할 함수를 호출합니다.
<br><br><br><br><br><br><br>



# 14장 <비디오 재생 앱>
- 비디오 플레이어는 아이폰 사용자들이 가장 많이 사용하는 앱 중의 하나입니다.
- AVPlayerViewController를 사용하면 앱 내부에 저장되어 있는 비디오 파일뿐만 아니라 외부에 링크된 비디오 파일도 간단하게 재생할 수 있습니다.<br><br>


코드 설명
-------------------------
```
import AVKit
```
비디오 관련 헤더 파일을 추가합니다.<br><br><br>

------------------------
```
@IBAction func btnPlayInternalMovie(_ sender: UIButton) {
    let filePath:String? = Bundle.main.path(forResource: "FastTyping", ofType: "mp4")
    let url = NSURL(fileURLWithPath: filePath!)
    
    let playerController = AVPlayerViewController()
    
    let player = AVPlayer(url: url as URL)
    playerController.player = player
    
    self.present(playerController, animated: true) {
        player.play()
    }
}
```
1. 비디오 파일명을 사용하여 비디오가 저장된 앱 내부의 파일 경로를 받아옵니다.<br>
2. 앱 내부의 파일명을 NSURL 형식으로 변경합니다.<br>
3. AVPlayerViewController의 인스턴스를 생성합니다.<br>
4. 앞에서 얻은 비디오 URL로 초기화된 AVPlayer의 인스턴스를 생성합니다.<br>
5. AVPlayerViewController의 player 속성에서 생성한 AVPlayer 인스턴스를 할당합니다.<br>
6. 비디오를 재생합니다.<br><br><br>

-----------------------
```
let url = NSURL(string: "https://dl.dropboxusercontent.com/s/e38auz050w2mvud/Fireworks.mp4")!

let playerController = AVPlayerViewController()

let player = AVPlayer(url: url as URL)
playerController.player = player

self.present(playerController, animated: true) {
   player.play()
}
```
외부에 링크된 비디오를 재생하는 코드입니다. 외부에 링크된 비디오를 재생하는 방법은 앱 내부 비디오를 재생하는 방법에서 url을 얻는 방법만 다르고 그외의 방법은 모두 동일합니다.<br><br><br>

------------------------
```
private func playVideo(url: NSURL) {        //url을 인수로 받는 playVideo라는 함수를 만듭니다.
}
```
비디오 재생 함수 코드입니다.<br><br><br>

---------------------
```
playVideo(url: url)
```
위 외부에 링크된 비디오를 재생하는 코드 부분을 playVideo(url: url)로 대체합니다. 이 코드는 url을 얻은 후 playVideo 함수를 호출합니다. 이렇게 하면 전체 소스가 훨씬 간략해지고 수정하기도 편리합니다.

