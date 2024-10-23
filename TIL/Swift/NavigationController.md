# NavigationBar

## prefersLargeTitles

NavigationBarTitle을 크게 하고 싶을 때는

```swift
navigationController?.navigationBar.prefersLargeTitles = true
```

를 사용하면 됩니다.

하지만 위 코드만 작성하면, navigaiton이 이동하여 다른 뷰로 이동했을 때도 Title이 커지게 됩니다.

이 경우는 이동된 viewController에 ViewDidLoad에서 largeTitleDisplayMode를 .never로 설정해주면 됩니다.

```swift
navigationItem.largeTitleDisplayMode = .never
```

## Toolbar

화면 아래에 있는 버튼영역을 담당하는 곳을 Toolbar라고 합니다.
navigationController에 있는 toolbar는 기본적으로 hidden 속성을 가지고 있어서,
이를 보여주기 위해서 해당 속성을 false로 바꿔줘야 합니다.

```swift
navigationController?.isToolbarHidden = false
```

이후 버튼을 추가해줘야 하는데, 그냥 버튼만 추가하면 버튼이 다닥다닥 붙어있습니다.

![](https://i.imgur.com/CIbFTXB.png)

버튼 사이에 간격을 주기 위해서는 `flexibleSpace`가 필요합니다.

```swift
let flexibleSpace = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: self, action: nil)
```

공간만 차지하는 flexibleSpace 버튼을 원하는 위치의 배열 인덱스에 넣어주면 잘 적용이 됩니다.

예를들어 [`전체 선택 버튼`, `flexibleSpace`, `닫기 버튼`] 이 될 수 있습니다.

전체 코드는 다음과 같습니다.

```swift
func setToolbar() {
    let selectAllButton: UIBarButtonItem = UIBarButtonItem(title: "전체 선택", style: .plain, target: self, action: nil)
    let closeButton: UIBarButtonItem = UIBarButtonItem(title: "닫기", style: .plain, target: self, action: nil)
    let flexibleSpace = UIBarButtonItem(barButtonSystemItem: .flexibleSpace, target: self, action: nil)
    let toolbarButtons: [UIBarButtonItem] = [selectAllButton, flexibleSpace, closeButton]
    toolbarItems = toolbarButtons
}
```
