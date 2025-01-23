네트워크 연결 상태를 확인하는 방법으로는 Network 프레임워크의 NWPathMonitor가 있습니다.

이를 이용해 현재 네트워크가 연결되어 있는지 여부를 판단 가능했습니다.

```swift

final class NetworkMonitor {

private let queue = DispatchQueue.global(qos: .background)

private let monitor: NWPathMonitor



init() {

monitor = NWPathMonitor()

dump(monitor)

}



func startMonitoring(statusUpdateHandler: @escaping (NWPath.Status) -> Void) {
    monitor.pathUpdateHandler = { path in
    // 메인 스레드에서 작업하도록 하여 UI 업데이트시 문제가 발생하지 않도록 함.
        DispatchQueue.main.async {
            statusUpdateHandler(path.status)
        }
    }
    monitor.start(queue: queue)
}

func stopMonitoring() {
    monitor.cancel()
}
```

위 처럼 NWPathMonitor의 startMonitoring의 escaping handler로 NWPath.Status를 통해 연결 상태에 따라 다른 동작을 수행하도록 할 수 있었습니다.

이제 해당 객체를 호출해서 사용할 부분을 찾아야 했는데, 이는 SceneDelegate에서 해줄 수 있었습니다.

### SceneDelegate의 willConnectTo

scene이 앱에 추가될 때 호출되는 메서드 (화면에 앱이 등장할 때 호출된다고 생각)

```swift

func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {

guard (scene is UIWindowScene) else { return }

networkMonitor.startMonitoring { [weak self] connectionStatus in

switch connectionStatus {

case .satisfied:

os_log("Internet Connected!")

case .unsatisfied:

self?.showAlert("인터넷 연결 끊김", message: "인터넷 연결 상태를 확인해주세요.")



os_log("Internet Disconnected!")

case .requiresConnection:

break

@unknown default:

fatalError()

}

}

}

func sceneDidDisconnect(_ scene: UIScene) {

networkMonitor.stopMonitoring()

}

```

어떤 화면으로 이동하던지 그 화면에 이동할 경우 모니터링이 시작되게 하였고, 다른 뷰로 넘어가서 sceneDidDisconnect가 되었을 경우 모니터를 종료하도록 하였습니다.
