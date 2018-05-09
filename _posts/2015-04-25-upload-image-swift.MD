---
layout: post
title: "用 swift 上传文件到 Rails 服务器"
---

###1. 用Base64 编码，在 Rails 解码

{% highlight swift %}
var imageData = UIImageJPEGRepresentation(image, 0.9)
var base64String = imageData.base64EncodedStringWithOptions(.allZeros)
{% endhighlight %}
然后按照普通字符串传到rails 服务器就行了，但是在服务器端可能要加上表示这是一个图像的头文件， 类似`"data:image/jpg;base64," + Base64string`

###2. 用二进制数据

{% highlight swift %}
var imageData = UIImageJPEGRepresentation(image, 1.0);
//var request = createRequest(userid: "ss", password: "ss", email: "ss", data: imageData)
let boundary = generateBoundaryString()
let url = NSURL(string: "http://localhost/api/test")
let request = NSMutableURLRequest(URL: url!)
request.HTTPMethod = "POST"
request.setValue("multipart/form-data; boundary=\(boundary)", forHTTPHeaderField: "Content-Type")
let body = NSMutableData()
let filename = "abc"
let mimetype = "multipart/form-data; boundary=\(boundary)"
body.appendData("--\(boundary)\r\n".dataUsingEncoding(NSUTF8StringEncoding)!)
body.appendData("Content-Disposition: form-data; name=image; filename=abc.jpg\r\n".dataUsingEncoding(NSUTF8StringEncoding)!)
body.appendData("Content-Type: \(mimetype)\r\n\r\n".dataUsingEncoding(NSUTF8StringEncoding)!)
body.appendData(imageData)
body.appendData("\r\n".dataUsingEncoding(NSUTF8StringEncoding)!)
body.appendData("--\(boundary)--\r\n".dataUsingEncoding(NSUTF8StringEncoding)!)
request.HTTPBody = body
NSURLConnection.sendAsynchronousRequest(request, queue: NSOperationQueue.currentQueue()) { (reponse, data, error) -> Void in
    //NSLog(reponse.description)

}
{% endhighlight %}
