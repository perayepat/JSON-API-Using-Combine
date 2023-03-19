# JSON-API-Using-Combine
how to to consume json data using combine

### Overview 
- we are using codable support with combine

This example code will return a string of the post data

```swift
struct Post: Codable{
    let title: String
    let body: String
}

func getPosts() -> AnyPublisher<[Post], Error> {
    
    guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts") else {
        fatalError("Invalid URL")
    }
    
    return URLSession.shared.dataTaskPublisher(for: url).map { $0.data }
        .decode(type: [Post].self, decoder: JSONDecoder())
        .eraseToAnyPublisher() /// it is not teid up to any publihser and allows any publisher to access it
}

/// We assign it to canellable so can hold on to the publisher
let cancellable = getPosts().sink(receiveCompletion: { _ in }, receiveValue: { print($0) })
```

