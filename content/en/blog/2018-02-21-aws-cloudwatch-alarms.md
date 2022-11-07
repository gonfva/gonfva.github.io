---
layout: post
previous: https://gonfva.medium.com/aws-cloudwatch-alarms-bf71d5bcb5a8
title: "AWS CloudWatch alarms"
subtitle: Insufficient is wrong
date: 2018-02-21T18:00:00
tags:
  - Developer
categories: [Developer, got ya]
---

One of the things less obvious for anyone trying to setup Cloudwatch alarms is the treatment of missing points.

You create an alarm, to detect too many 5xx errors in your ELB. But you don’t want it to be triggered too much, so you ask for at least one error per minute during 5 minutes. And you discover that the error gets triggered whenever during the last 5 minutes there was either zero, 2 or more. The alarm would be insufficient_data if zero in the last 5 minutes, or OK if 1 in the last 5 minutes.

And the problem is the below “TreatMissingData”.

```
"ELBMany500Alarm": {
  "Type": "AWS::CloudWatch::Alarm",
  "Properties": {
  "Namespace": "AWS/ApplicationELB",
  "AlarmName": { "Fn::Join" : ["", [ { "Ref" : "AWS::StackName"}, "-high5xx"]]},
  "AlarmDescription": { "Fn::Join" : [" ", [ "Too many 5xx errors in", { "Ref" : "AWS::StackName"}]]},
  "ComparisonOperator": "GreaterThanThreshold",
  "Threshold": "1",
  "EvaluationPeriods": "5",
  "Period": "60",
  "MetricName": "HTTPCode\_Target\_5XX\_Count",
  **"TreatMissingData": "notBreaching",**
  "Statistic": "Sum",
  "OKActions": [],
  "AlarmActions": [ { "Ref": "NotificationARN" } ],
  "Dimensions": [ {
  "Name": "LoadBalancer",
  "Value": { "Fn::GetAtt" : [ "LoadBalancer" ,"LoadBalancerFullName"] }
  }]
}
```

You need to tell Cloudwatch that no errors is OK. Which is a bit counter-intuitive.

So you start getting a bit nervous when you see in the web console that there are alarms with state INSUFFICIENT.

![](/img/1*iaXpA1Y7m_BsifhsuIc_LA.png)

And you start wondering why your AWS managed ElasticSearch cluster inside your own VPC has the alarms with insufficient data.

If you face something like that, consider if your dimensions are correctly defined.

```
"ESAlarmClusterRedHealth": {
"Type": "AWS::CloudWatch::Alarm",
"Properties": {
"Namespace": "AWS/ES",
"AlarmName": { "Fn::Join" : ["", [ { "Ref" : "AWS::StackName"}, "-hth-r"]]},
"AlarmDescription": { "Fn::Join" : [" ", [ "Cluster red in", { "Ref" : "AWS::StackName"}]]},
"ComparisonOperator": "GreaterThanThreshold",
"Threshold": "0",
"EvaluationPeriods": "3",
"Period": "60",
"MetricName": "ClusterStatus.red",
"Statistic": "Minimum",
"TreatMissingData": "notBreaching",
"OKActions": [],
"AlarmActions": [ { "Ref": "NotificationARN" } ],
"Dimensions": [ {
"Name": "DomainName",
"Value": {"Ref": "ElasticsearchDomain"}
},
**{
"Name": "ClientId",
"Value": {"Ref": "AWS::AccountId"}
}**]
}
}
```

It turns out that the DomainName is not enough, and you need to pass the ClientId too.

You got the memo. Cloudwatch alarms in insufficient state are probably wrong.
