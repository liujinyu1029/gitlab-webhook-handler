# gitlab-webhook-handler

[![NPM](https://nodei.co/npm/gitlab-webhook-handler.png?downloads=true&downloadRank=true)](https://nodei.co/npm/gitlab-webhook-handler/)
[![NPM](https://nodei.co/npm-dl/gitlab-webhook-handler.png?months=6&height=3)](https://nodei.co/npm/gitlab-webhook-handler/)

Modify **[rvagg/github-webhook-handler](https://github.com/rvagg/github-webhook-handler)** to fit Gitlab.

GitLab allows you to register **[Webhooks](https://gitlab.com/help/web_hooks/web_hooks)** for your repositories. Each time an event occurs on your repository, whether it be pushing code, filling issues or creating pull requests, the webhook address you register can be configured to be pinged with details.

This library is a small handler (or "middleware" if you must) for Node.js web servers that handles all the logic of receiving and verifying webhook requests from GitHub.


## Example

$ npm install gitlab-webhook-handler

```js
var http = require('http')
var createHandler = require('gitlab-webhook-handler')
var handler = createHandler({ path: '/webhook' })

http.createServer(function (req, res) {
  handler(req, res, function (err) {
    res.statusCode = 404
    res.end('no such location')
  })
}).listen(7777)

console.log("Gitlab Hook Server running at http://0.0.0.0:7777/webhook");

handler.on('error', function (err) {
  	console.error('Error:', err.message)
})

handler.on('push', function (event) {
  	console.log('Received a push event for %s to %s',
    event.payload.repository.name,
    event.payload.ref)
})

handler.on('issues', function (event) {
  	console.log('Received an issue event for %s action=%s: #%d %s',
    event.payload.repository.name,
    event.payload.action,
    event.payload.issue.number,
    event.payload.issue.title)
})
```

## 中文使用说明
**[使用 GitHub / GitLab 的 Webhooks 进行网站自动化部署](http://www.lovelucy.info/auto-deploy-website-by-webhooks-of-github-and-gitlab.html)**