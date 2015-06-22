# Simple RESTful Client

Simple C# class to provide an easy way of accessing RESTful API's. Based on the code of @AdamNThompson, this version provide extras like the HTTP PUT verb, possiblity to add headers, new constructors and some little code improvements.

## Use

Just create an instance of the `RestClient` class, assign the value of your endpoint (the endpoint is the URL of the REST service you are attempting to call), and call the MakeRequest method.

Basic GET call. 

```cs
string endPoint = @"http://jsonplaceholder.typicode.com/posts";
var client = new RestClient(endPoint);
var json = client.MakeRequest();
```

## More examples

using headers for Basic access authentication

.....

PUT example using `System.Web.Script.Serialization` and try catch

```cs
try
{
    string endPoint = @"http://jsonplaceholder.typicode.com/posts/1";
    
    dynamic data = new { 
        id = 1,
        title = "foo",
        body = "bar",
        userId = 1 
    };

    var serializer = new JavaScriptSerializer();
    var json = serializer.Serialize(data);

    var client = new RestClient(endPoint, HttpVerb.PUT, "application/json", json);
    return client.MakeRequest();
}
catch (WebException webExcp) 
{
    Console.WriteLine("A WebException has been caught.");
    // Write out the WebException message.
    Console.WriteLine(webExcp.ToString());
    // Get the WebException status code.
    WebExceptionStatus status =  webExcp.Status;
    // If status is WebExceptionStatus.ProtocolError, 
    //   there has been a protocol error and a WebResponse 
    //   should exist. Display the protocol error.
    if (status == WebExceptionStatus.ProtocolError) {
        Console.Write("The server returned protocol error ");
        // Get HttpWebResponse so that you can check the HTTP status code.
        HttpWebResponse httpResponse = (HttpWebResponse)webExcp.Response;
        Console.WriteLine((int)httpResponse.StatusCode + " - "
           + httpResponse.StatusCode);
    }
}
catch (Exception e) 
{
    // Code to catch other exceptions goes here.
}
```

> Note some services return an HTTP status code even when the request went through, for example when searching for a resource but none are found the server, instead of returning a response with data, may return `404 Not Found` error. This errors must be handle inside the try catch.

