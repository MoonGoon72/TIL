> [!note]
> search bar와의 상호 작용을 기반으로 검색 결과를 표시하는 ViewController

ViewController의 `navigationItem.searchController`에 할당하면 SearchBar의 검색 결과를 표시할 수 있습니다.

## SearchBar text를 기반으로 필터링하기
###  UISearchResultsUpdating
UISearchResultsUpdating 프로토콜을 채택하면
```swift
func updateSearchResults(for searchController: UISearchController)
```
해당 메서드를 반드시 구현해야 합니다. 

해당 메서드는 UISearchController의 `SearchBar.text`의 값이 바뀌면 자동으로 호출됩니다.