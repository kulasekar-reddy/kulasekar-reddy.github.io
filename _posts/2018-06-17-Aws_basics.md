---
layout: post
title: AWS Basics
description: "How to use AWS free instances from local"
created: 2018-06-18
comments: true
share:    true
tags: [aws,linux]
image:
  background: wood.jpg
---

Basic commands used in AWS 

### connecting to remote AWS
```
ssh -i "<pemfile>.pem" ec2-user@<servername>
```

pemfile and servername you will get from AWS console under EC2 section

### Copying local file/folder to remote AWS machine

```
scp -i <pemfile>.pem -r <file/folder name>  ec2-user@<severname>:~/.
```

The above command will copy file to ```/home/ec2-user``` folder in your AWS instance