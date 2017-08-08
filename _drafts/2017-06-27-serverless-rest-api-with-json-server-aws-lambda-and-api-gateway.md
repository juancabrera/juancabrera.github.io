---
layout:     post
title:      Serverless REST API with json-server, AWS Lambda and API Gateway
date:       2017-06-27
categories: javascript node aws serverless
---

[`json-server`](https://github.com/typicode/json-server){:target="_blank"} is a great tool to get a mock REST API running in no time. I thought it would be even better to get it running on a server-less infrastructure so as a front-end developer I won't have to deal with all the "server stuff" and the endpoint will be accesible for many developers.

This is not meant to say that this is better than using services like [apiary](https://apiary.io/){:target="_blank"} or similar. The porpose of this is to show that getting a REST API up for mocking or prototyping is fairly easy and quick.

## The Code
I will assume you already have the Lambda function created, if not you can [follow this to see how to create one](http://docs.aws.amazon.com/lambda/latest/dg/getting-started.html){:target="_blank"}. Let's say the funcion's name is `jsonServer`

### Lambda function
This is the file where we export our Lambda function (`export.handler`). 
Notice that we also import `app.js`, this is where the REST API express server runs.

{% highlight javascript %}
// index.js

const awsServerlessExpress = require('aws-serverless-express')
const app = require('./app')

const server = awsServerlessExpress.createServer(app)

exports.handler = (event, context) => awsServerlessExpress.proxy(server, event, context)
{% endhighlight %}

### REST API server
This is where we start our REST API server export our express app that then it's included on `index.js`. If you want to know more about how `json-server` works you can check their [Github](https://github.com/typicode/json-server){:target="_blank"}. For the sample `json` file we can use

{% highlight javascript %}
// app.js

const express = require('express')
const jsonServer = require('json-server')
const path = require('path')

const app = express()
const router = jsonServer.router(path.join(__dirname, 'db.json'))
const middlewares = jsonServer.defaults()

app.use(middlewares)
app.use('/test', (req, res) => res.json({test: "TRUE"}))
app.use(router)

module.exports = app
{% endhighlight %}

### db.json
This JSON file will eventually get exposed to a REST API, let's use the same `json-server`'s example file:

{% highlight json %}
// db.json

{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
{% endhighlight %}
