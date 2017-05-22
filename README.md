## Simple GET request
```swift
URLSession.shared.dataTask(with: url) { (data, response, error) in
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else {
        // request failed
        return
    }
    // handle status code
}.resume()
```
## Query parameters
```swift
var urlComponents = URLComponents(string: "https://www.google.com")!
urlComponents.queryItems = [
    URLQueryItem(name: "foo1", value: "bar"),
    URLQueryItem(name: "foo2", value: "baz")
]
let url = urlComponents.url // https://www.google.com?foo1=bar&foo2=baz
```
### Extension example
```swift
extension URL {
    init?(baseUrl: String, queryItems: [String: String]) {
        guard var urlComponents = URLComponents(string: baseUrl) else { return nil }
        urlComponents.queryItems = queryItems.map { URLQueryItem(name: $0.key, value: $0.value) }
        guard let finalUrlString = urlComponents.url?.absoluteString else { return nil }
        self.init(string: finalUrlString)
    }
}
```
## Method, body and headers
```swift
var request = URLRequest(url: url)
request.httpMethod = "POST"
request.httpBody = data
request.addValue("application/json", forHTTPHeaderField: "Content-Type")
request.addValue("*/*", forHTTPHeaderField: "Accept")
```
### Extension example
```swift
extension URLRequest {
    static func request(url: URL, headers: [String: String] = [:], method: String = "GET", data: Data? = nil) -> URLRequest {
        var request = URLRequest(url: url)
        request.httpMethod = method
        request.httpBody = data
        headers.forEach { header in
            request.addValue(header.value, forHTTPHeaderField: header.key)
        }
        return request
    }
}
```
## JSON
### From JSON object to Data
```swift
guard let data = try? JSONSerialization.data(withJSONObject: jsonObject, options: []) else {
    // JSON serialization failed
    return
}
```
### From Data to JSON object
```swift
guard let json = try? JSONSerialization.jsonObject(with: data, options: []) else {
    // JSON serialization failed
    return
}
```
