---
layout: post
title:  "How to limit Amazon S3 access to one bucket only"
date:   2014-04-05
categories: amazon
tags: s3
---
{% include JB/setup %}

If you want to limit the access of a specific user/group to only one S3 bucket you should use the following IAM policy:

{% highlight json %}
{
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:ListBucket",
        "s3:GetBucketLocation",
        "s3:ListBucketMultipartUploads"
      ],
      "Resource": "arn:aws:s3:::itnighq",
      "Condition": {}
    },
    {
      "Effect": "Allow",
      "Action": [
        "s3:AbortMultipartUpload",
        "s3:DeleteObject",
        "s3:DeleteObjectVersion",
        "s3:GetObject",
        "s3:GetObjectAcl",
        "s3:GetObjectVersion",
        "s3:GetObjectVersionAcl",
        "s3:PutObject",
        "s3:PutObjectAcl",
        "s3:PutObjectAclVersion"
      ],
      "Resource": "arn:aws:s3:::itnighq/*",
      "Condition": {}
    },
    {
      "Effect": "Allow",
      "Action": "s3:ListAllMyBuckets",
      "Resource": "*",
      "Condition": {}
    }
  ]
}
{% endhighlight %}

Interstingly enough to access a bucket, you also need the right to list all the buckets.

