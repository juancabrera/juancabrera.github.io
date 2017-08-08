---
layout:     post
title:      How to deploy a Node Lambda function
date:       2016-11-17
categories: javascript node aws serverless
---

When you are building a Lambda function locally you will find that you need to upload the code *pretty* often in order to test it. Doing this manually could be quite a pain and time consuming. Here, I will show the easiest way I found to deploy a Node Lambda function using `aws-sdk`.  


### 1. Create a `deploy.js` file in your project
Update the `region` and the function name:

```javascript
// deploy.js
const AWS = require('aws-sdk')
const fs = require('fs')

AWS.config.update({region: 'us-east-1'})

const lambda = new AWS.Lambda()
const lambdaFunctionName = 'yourFunctionName'

fs.readFile("./fn.zip", (err, data) => {
  if (err) throw err

  lambda.updateFunctionCode({
    FunctionName: lambdaFunctionName,
    ZipFile: data,
    Publish: true
  }, err => {
    if (err) {
      console.log('ERROR: ', err)
    } else {
      console.log('uploaded!')
    }
  })
})
```

### 2. Add a `deploy` script to your `package.json`
It should look something like this:

```Javascript
"scripts": {
  "test": "echo \"Error: no test specified\" && exit 1",
  "deploy": "zip -r fn.zip index.js node_modules package.json; node deploy"
}
```
If your Lambda Function need more files just add them to the `zip` command.

You could also zip the files on the `deploy.js` file and then just run `node deploy` from the command line.

### 3. Deploy!
Then just run:

```bash
npm run deploy
```

#### AWS keys
You probably got this, but make sure you have your AWS keys as environment variables. These should be on your `~/.bashrc` file and should look something like:

```shell
export AWS_ACCESS_KEY_ID="YOURACCESSKEYID"
export AWS_SECRET_ACCESS_KEY="YOURSECRETACCESSKEY"
```
