# nodejs-with-sse
html5 服务器推送事件

index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
    </head>
    <body>
        <div id="result"></div>
    <script>
        var source = new EventSource('/sse');
        source.onmessage = function(event) {
            console.log(event)
            console.log(JSON.parse(event.data).age)
            document.getElementById('result').innerHTML +=event.data + '<br>'
        }
        source.onopen = function() {
            console.log('打开了连接')
        }
        source.onerror = function() {
            console.log('err')
        }
    </script>
    </body>
</html>
```
nodejs(express)
```javascript
app.all('/sse', function(req, res) {
	console.log(req.headers['user-agent'])
	res.writeHead(200, {
		'Connection': 'keep-alive',
		'Content-Type': 'text/event-stream',
		'Cache-Control': 'no-cache'
	});
	var message = {
		name: 'fwh',
		age: 26
	}
	setInterval(function() {
		console.log('发送');
		res.write("id: " + Date.now() + "\ndata:{\"foo\":\"bar\",\"age\": \"26\"}\n\n");
	}, 50000)
})
```

