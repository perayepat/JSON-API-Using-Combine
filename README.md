# JSON-API-Using-Combine
how to to consume json data using combine

### Overview 
- we are using codable support with combine

How to get data from a service using combine

```swift
    func getPosts() -> AnyPublisher<[Post],Error> {
    
    guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts") else {
          fatalError("Invalid URL")
      }
      
      return URLSession.shared.dataTaskPublisher(for: url).map { $0.data }
      .decode(type: [Post].self, decoder: JSONDecoder())
      .receive(on: RunLoop.main)
      .eraseToAnyPublisher()
        
    }
```

