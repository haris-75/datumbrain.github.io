---
layout: post
title: MongoDB on Heroku Without Giving Verifying Payment Information
author: Nauman Zafar Chaudhry
date: "2018-10-12 13:00:00"
---

![MongoDB-on-heroku](/assets/posts/nodejs-mongodb-heroku.png)

Started learning node and have been writing a simple API using `express` for practice. After configuring some routes I wanted to get the API live and tried `heroku` for that.
API uses `MongoDB` via mongoose ORM. On Heroku you can setup a MongoDB database using addons like `mongolab` etc.



#### Problem
> Heroku requires your payment information for some addons even for sandbox account. Which you don't want to give when using a sandbox account or just learning something just like me.

#### Solution

> The solution in simple words is, set up a mongodb database but not using heroku.

#### How? `mlab.com` to Rescue

As you know all you need to connect to a db is connection string. For mongo you have connection string something like this.

```
mongodb://<user>:<password>@<hostname>:<port>/<database>
```

Head over to [https://mlab.com/](https://mlab.com/) and register a sandbox account

##### Steps
1. Register and verify your account.
2. Select the sandbox plan.
3. Create the database.
4. Create a new db user for this database.

In this page under your database name you will see a connection string mentioned with the name `MongoDB URI` which looks something like this. Yours will be different.

```
mongodb://<dbuser>:<dbpassword>@de243578.mlab.com:47178/todoapp
```

So for the following db details

dbuser | dbpassword
--- | ---
gunther | centralperk


```
mongodb://gunther:centralperk@de2435678.mlab.com:47178/todoapp
```

Now you have connection string ready to use in your code

As a last step, set this connection string to a `Heroku` environment variable so your code on heroku knows this connection string. `cd` into your project directory.

```
heroku config:set MONGOLAB_URI = "mongodb://gunther:centralperk@de243578.mlab.com:47178/todoapp"
```

You can access this connection string via `process.env.MONGOLAB_URI`.

In my instance I am using mongoose to connect to `MongoDB` so this is how my code looks like for the connection.

```javascript
const mongoose = require('mongoose')
mongoose.connect(process.env.MONGOLAB_URI || 'mongodb://127.0.0.1:27017/TodoApp');
```

With this in place, you have a mongodb on your heroku application!
