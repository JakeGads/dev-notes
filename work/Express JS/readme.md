# Express JS

As a note I have a lot of experience with express and the docs are short and suite due to the fact that it is a quick and easy.

## Simple setup

```js
// importing library
const express = require('express');
// starting the api
const app = express();

// register routes
app.get('/', (req, res) => {
  res.send('Hello World!')
})

// assign it a port (you may want to export the port so save it to a variable)
app.listen(3000, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})
```

## Req(uests)

"""<br>
The req object represents the HTTP request and has properties for the request query string, parameters, body, HTTP headers, and so on. In this documentation and by convention, the object is always referred to as req (and the HTTP response is res) but its actual name is determined by the parameters to the callback function in which you’re working.
<br><br>
The req object is an enhanced version of Node’s own request object and supports all built-in fields and methods.<br>"""

### Req.app

allows you to access the app that this request is registered too

### Req.baseUrl

gets the back path of the url for instance if you go to `/greeting/jp` and use baseURL you will receive `/greeting` as a response