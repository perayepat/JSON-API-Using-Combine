# JSON-API-Using-Combine
how to to consume json data using combine

### Overview 
- we are using codable support with combine

#### Basic How to 
How to get data from a service using combine

```swift
    func getPosts() -> AnyPublisher<[Post],Error> {
    
    guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts") else {
          fatalError("Invalid URL")
      }
      
      return URLSession.shared.dataTaskPublisher(for: url).map { $0.data }
      .decode(type: [Post].self, decoder: JSONDecoder())
      .receive(on: RunLoop.main) /// Get the values on the main thread
      .eraseToAnyPublisher()
        
    }
```

Using it in a tableview controller in UIkit

``` swift 
class PostListTableViewController: UITableViewController {
    
    private var webservice = Webservice()
    private var cancellable: AnyCancellable? /// Also called a subscription will only live as long as this view controller is alive
    
    private var posts = [Post]() {
        didSet {
            self.tableView.reloadData()
        }
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        /// Getting a hold of the publisher and assigning it to somewhere using the cancallable
        self.cancellable = self.webservice.getPosts()
            .catch { _ in Just(self.posts)} /// Catching the errors
            .assign(to: \.posts, on: self)
    }
    
    override func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }
    
    override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return self.posts.count
    }
    
    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        
        let cell = tableView.dequeueReusableCell(withIdentifier: "PostTableViewCell", for: indexPath)

        let post = self.posts[indexPath.row]
        cell.textLabel?.text = post.title
        
        return cell
    }
    
}

```
