# Network I/O

---

Network Input/Output (I/O) forms the backbone of most modern applications. It's how applications interact with networks, allowing them to send and receive data to and from remote servers.

C# provides comprehensive support for `Network I/O` operations through its `System.Net` and `System.Net.Sockets` namespaces, among others. These namespaces include a variety of classes and methods that encapsulate the complexity of network programming, making it easier for developers to create network-centric applications.

## HttpClient
The `HttpClient` class in C# is a part of the `System.Net.Http` namespace and provides a modern, flexible, and highly configurable way to send HTTP requests and receive HTTP responses from a resource identified by a URI (Uniform Resource Identifier). It's frequently used to consume APIs, download files, or scrape web content.

The `HttpClient` class is designed to be re-used for multiple requests. As such, it's typically instantiated once and re-used throughout the life of an application, which can improve performance and system resource usage by allowing socket reuse.

The `HttpClient` class includes several methods to send HTTP requests. The primary methods are:

- `GetAsync`: Sends a GET request to the specified URI and returns the response body as a string.
- `PostAsync`: Sends a POST request to the specified URI with a specified content.
- `PutAsync`: Sends a PUT request to the specified URI with a specified content.
- `DeleteAsync`: Sends a DELETE request to the specified URI.

```csharp
HttpClient client = new HttpClient();

// Send a GET request
var response = await client.GetAsync("https://api.example.com/data");

// Ensure we get a successful response
response.EnsureSuccessStatusCode();

// Read the response content
string content = await response.Content.ReadAsStringAsync();

```

In this example, we create an instance of `HttpClient`, send a `GET` request to a specified URI, ensure we received a successful response, and then read the response content into a string.

### GetAsync

`GetAsync` sends a `GET` request to a specified URI. This is an asynchronous operation, meaning the method returns immediately after calling without waiting for the HTTP response. Instead, it returns a Task representing the ongoing operation, which eventually produces the `HttpResponseMessage` once completed.

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        try
        {
            HttpResponseMessage response = await client.GetAsync("http://api.example.com/data");
            response.EnsureSuccessStatusCode();
            string responseBody = await response.Content.ReadAsStringAsync();
            Console.WriteLine(responseBody);
        }
        catch(HttpRequestException e)
        {
            Console.WriteLine("Exception Caught!");
            Console.WriteLine($"Message: {e.Message}");
        }
    }
}
```

In this example, we send a `GET` request to `http://api.example.com/data`, and then read the response body into a string.

### PostAsync

`PostAsync` is another method in the `HttpClient` class. It sends a POST request to a specified URI and some HTTP content. Like `GetAsync`, it's an asynchronous operation and returns a `Task<HttpResponseMessage>`.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        try
        {
            var json = "{\"name\":\"John Doe\"}";
            HttpContent content = new StringContent(json, Encoding.UTF8, "application/json");
            HttpResponseMessage response = await client.PostAsync("http://api.example.com/data", content);
            response.EnsureSuccessStatusCode();
            string responseBody = await response.Content.ReadAsStringAsync();
            Console.WriteLine(responseBody);
        }
        catch(HttpRequestException e)
        {
            Console.WriteLine("Exception Caught!");
            Console.WriteLine($"Message: {e.Message}");
        }
    }
}
```

In this case, we send a JSON object as the body of our POST request.

### PutAsync

`PutAsync` works much like `PostAsync`, but it sends a `PUT` request instead. It's used when you want to update a resource at a specific URI with some new data.

Code: csharp

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        try
        {
            var json = "{\"id\":1,\"name\":\"John Doe Updated\"}";
            HttpContent content = new StringContent(json, Encoding.UTF8, "application/json");
            HttpResponseMessage response = await client.PutAsync("http://api.example.com/data/1", content);
            response.EnsureSuccessStatusCode();
            string responseBody = await response.Content.ReadAsStringAsync();
            Console.WriteLine(responseBody);
        }
        catch(HttpRequestException e)
        {
            Console.WriteLine("Exception Caught!");
            Console.WriteLine($"Message: {e.Message}");
        }
    }
}

```

In this example, we send a PUT request to update the resource at `http://api.example.com/data/1` with new data.

### DeleteAsync

Finally, `DeleteAsync` sends a `DELETE` request to a specified URI. It's typically used when deleting a resource at a specific URI.

Code: csharp

```csharp
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static readonly HttpClient client = new HttpClient();

    static async Task Main()
    {
        try
        {
            HttpResponseMessage response = await client.DeleteAsync("http://api.example.com/data/1");
            response.EnsureSuccessStatusCode();
            string responseBody = await response.Content.ReadAsStringAsync();
            Console.WriteLine(responseBody);
        }
        catch(HttpRequestException e)
        {
            Console.WriteLine("Exception Caught!");
            Console.WriteLine($"Message: {e.Message}");
        }
    }
}
```

In this case, we send a `DELETE` request to `http://api.example.com/data/1` to delete the resource.