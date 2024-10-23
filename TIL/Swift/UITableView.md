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