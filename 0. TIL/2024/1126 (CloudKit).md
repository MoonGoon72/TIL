
# 2024-11-26 오늘의 기록
## 기존 Local CoreData 영구 저장소를 iCloud에 연동 가능하도록 수정함.
이는 기존에 `NSPersistentContainer`를 `NSPersistentCloudKitContainer`로 변경해주면 자연스럽게 CloudKit으로 사용이 가능합니다.
![](https://i.imgur.com/3VPorcw.png)

NSPersistentCloudKitContainer는 NSPersistentContainer를 상속받기 때문에 교체해도 아무런 문제 없이 사용이 가능합니다.

## Local CoreData로 사용하던 것을 iCloud 동기화 sync 기능을 추가
현재까지 알아낸 바로는, 유저가 iCloud 연동을 원하는지 여부를 bool 값으로 저장하고 있다는 가정하에 (UserDefaults에 저장) 이 값을 바탕으로
persistentContainer(type: NSPersistentCloudKitContainer).persistentDescriptions.first로 NSPersistentDescription을 가져옵니다.
해당 description의 `.cloudKitContainerOptions`를 nil로 초기화 해주면 해당 영구 저장소는 iCloud를 사용하지 않습니다.
그게 아니라면 자동으로 iCloud 연동 기능을 사용하게 됩니다.

## remote-notification error

![](https://i.imgur.com/isVwTFn.png)

cloudKit에서의 background update를 위해서는 remote notification 기능을 켜줘야 합니다.
![](https://i.imgur.com/3rit2yE.png)

Target -> Signing & Capabilities -> + -> Background Mode 추가 한 뒤 `remote notification`에 체크를 해주면 됩니다.

CloudKit 추가도 마찬가지로 Target -> Signing & Capabilities -> + -> iCloud 추가하면 됩니다.

## iCloud 연동할 store 선택
![](https://i.imgur.com/0FHp8gd.png)

저희 프로젝트는 Default 1개의 Configuration만 사용하기 때문에 해당 Configuration을 선택한 뒤 우측 inspector에서 Used with CloudKit을 선택해주면 동기화하게 됩니다!

만약 Configurations가 없다면 `NSPersistentCloudKitContainer`는 `첫 번째 StoreDescription`과  `entitlements`의 `첫 번째 CloudKit Container identifier`를 일치시킵니다.
```swift
guard let description = persistentContainer.persistentStoreDescriptions.first
	else { throw Error.storeSetUpFailed }
	if !isSync {
		description.cloudKitContainerOptions = nil
	} else {
		description.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "iCloud.kr.codesquad.boostcamp9.RetsTalk")
	}
```
![entitlements 파일](https://i.imgur.com/GyMmFpm.png)
![](https://i.imgur.com/zfQ6HVp.png)

## CoreData와 CloudKit 연동은 어떻게 자연스레 되는가?

### CoreData -> CloudKit
![](https://i.imgur.com/pCZcHOp.png)
(추측으로는) 앞서 
```swift
persistentContainer = NSPersistentCloudKitContainer(name: name)
```
를 만들어 주었고, 이로부터 CoreData는 반드시 생성이 됩니다. (올바른 name이라면)

그래서 CloudKit을 껐다가 켰을 때 container를 재생성하고 cloudKitContaierOptions를 할당해주지만,
```swift
description.cloudKitContainerOptions = NSPersistentCloudKitContainerOptions(containerIdentifier: "iCloud.kr.codesquad.boostcamp9.RetsTalk")
```

기존의 CoreData가 바뀌는 것은 아니기 때문에, 이 경우에는 Background task로 CloudKit에 전달이 되게 됩니다.
### CloudKit -> CoreData
![](https://i.imgur.com/KSHTudS.png)

위 경우는 `CloudKit`에서는 사용자의 다른 계정에 주기적으로 `Push Notification`을 보냅니다. 그 다음 각 장치에서 시스템은  마지막 패치 이후 변경된 모든 레코드를 다운로드하고, `NSManagedObject`의 인스턴스로 변환하는 백그라운드 작업을 만듭니다.
CoreData는 이러한 managedObject를 local store에 저장합니다.
## 참고 자료
[Syncing a Core Data Store with CloudKit](https://developer.apple.com/documentation/coredata/mirroring_a_core_data_store_with_cloudkit/syncing_a_core_data_store_with_cloudkit)
