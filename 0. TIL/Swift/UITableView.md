## UITableView

### UITableViewDelegate

UITableViewDelegate 프로토콜을 채택하면 아래 두 가지 함수는 반드시 구현해야 합니다.

```swift
func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int { }

func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell { }
```

### Trailing Swipe

아래 함수를 사용하면 tableViewCell에 대해 traillingSwipe가 가능해집니다.

```swift
tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration?
```

## TableView 다시 그리기

- reladData는 뷰 전체를 다시 그리는 것
- 특정 셀만 다시 그리고 싶다면 `reloadRows(at:with:)`, `performBatchUpdates(_:completion)` 을 사용하자

## TableViewCell 재사용

### tableView.register

tableViewCell을 재사용하기 위해서 `tableView.register` 를 해줄 때에는 tableViewCell에 대해 등록을 해주어야 합니다.

```swift
tableView.register(UITableViewCell.self, forCellReuseIdentifier: "ContentTableViewCell")
```

## TableViewCell 다중 선택

```swift
tableView.allowsMultipleSelectionDuringEditing = true

tableView.setEditing(true, animated: true)
```

다중 선택된 셀의 개수를 파악하기 위해서는
`tableView.indexPathsForSelectedRows?.count` 를 통해 파악할 수 있습니다.

## TableViewCell identifier

TableViewCell을 재사용하기 위한 identifier를 만들어주기 위해서 각 셀 파일에 `static let identifier = "ClassName"` 을 사용해주었습니다.

이 경우도 나쁘지 않지만, 만약 셀의 Class naming이 바뀐다면 휴먼 에러가 발생할 가능성이 생깁니다!!
해당 휴먼 에러를 방지하기 위해 (식별자를 클래스명으로 한다면) 다음과 같이 식별자를 정해줄 수 있었습니다.

```swift
static var identifier: String {
    String(describing: self)
}
```

해당 코드를 UIView나 UITableViewCell의 익스텐션에 추가해주게 된다면

`CustomTableViewCell.identifier` 로 휴먼에러를 방지할 수 있게 됩니다.
