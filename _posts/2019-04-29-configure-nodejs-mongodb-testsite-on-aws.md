---
layout: post
title: Configure a Node.js and MongoDB test site on aws
tag: [fundamental, penetration]
---

In daily practice, we normally want to setup a testing target as realistic as possible. For my case, I want to have a SQL-injection vulnerable website ready to be exploited. Some configurations could be annoying, however, this short write up should cover some common issues.

### My website setup

My website is a testing target for SQL-injection, and I want to make it more relevant to common technologies used by organizations and medium sized companies. **Node.js** and **MongoDB** are the tech of choices here.

- [Mongoose](https://mongoosejs.com/)
- [Express](https://expressjs.com/)
- [Pug](https://pugjs.org/)

My OS for vpc is [Ubuntu 18.04 LTS](http://releases.ubuntu.com/18.04/), installation process may varies for different AMIs, configurations should be the same.

Providing both webpages and RESTful API as SQLi mounting points, it is deliberately configured to be a vulnerable website to SQLi.

### MongoDB configuration

MongoDB configuration should be easy unless you want to connect to it using [Compass](https://docs.mongodb.com/compass/master/connect/) for management. For your VPC, setup your security group to allow all inbound and outbound **TCP** connection on port range 27017-27039 (as shown on aws)

First, update the repository

```shell
$ sudo apt update
```

Second, install MongoDB

```shell
$ sudo apt install -y mongodb
```

Third, stop MongoDB systemctl because we want to run it with some additional limitation

```shell
$ sudo systemctl stop mongodb
```

Now you want to configure the mongodb configuration file to **NOT** bind the ip

```shell
$ sudo vim /etc/mongodb.conf
```

And using \# to comment out *bind_ip=127.0.0.1*

```shell
#bind_ip=127.0.0.1
```

Then you want to create the /data/db dir

```shell
$ cd /
$ sudo mkdir data
$ sudo chown ubuntu data
$ sudo chgrp ubuntu data
$ cd data
$ mkdir db
```

Now you are good to go with additional limitation, your mongodb could be connect remotely with compass.

```shell
$ mongod --bind_ip_all
```

### Node.js installation

[Tutorial: Setting Up Node.js on an Amazon EC2 Instance](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html) is a super simple and straight forward tutorial on setup node.js, remember to install the latest LTS version of node.js and you are good to go.

### Testing website port configuration

It is crucial for someone who want to simulate a real life scenario to have the testing website running on port 80 :smirk:

Two lines of shell code can help you with it, and you can find explanation in [this great answer on stackoverflow](https://stackoverflow.com/questions/23281895/node-js-eacces-error-when-listening-on-http-80-port-permission-denied)

```shell
$ sudo apt-get install libcap2-bin
$ sudo setcap cap_net_bind_service=+ep `readlink -f \`which node\``
```

And that's it, with these configuration, a test site with management connection setup shall be pretty simple.