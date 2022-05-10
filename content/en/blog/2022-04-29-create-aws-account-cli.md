---
date: 2022-04-29T20:48:54
title: "Create an AWS account from the command line"
subtitle: Automate all things
tags:
  - Developer
categories: [Developer, ctoclick]
---

This post is a basic copy of this [AWS page on how to create an AWS account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_create.html). But it took me a while to understand all the steps (because I cannot read), so I'm just writing so that the next time I can remember.

First things first. You need to have an organisation (sorry, living in UK, so I'll use the s) already created, and you need to have admin in the "management account" of the organisation. Technically, the requirements are a bit more relaxed: probably it is good enough to have organisation creation permissions, and it might be possible to do it on an account that is lower in the tree.

OK. Then, the next step is trivial:

```
$ aws organizations create-account --email <email> \
  --account-name <name of account>
```

for example

```
$ aws organizations create-account --email XXXX@XXXX.XXX \
  --account-name xowit-prod
```

You need a new email address. If you use gmail, one possibility is to use the [plus sign trick](https://gmail.googleblog.com/2008/03/2-hidden-ways-to-get-more-from-your.html), and basically reuse your account.

This will return a json object that contains an id in the format `car-<snip with loads of numbers and letters>`

You think that the account is created BUT IT IS NOT. My naive approach was to use that car thingy to describe the account. None of the following commands worked

```
aws organizations describe-account --account-id car-<snip with loads of numbers and letters>
aws organizations describe-account --create-account-request-id car-<snip with loads of numbers and letters>
```

You need to use a different command.

```
$ aws organizations describe-create-account-status --create-account-request-id car-<snip with loads of numbers and letters>
{
    "CreateAccountStatus": {
        "Id": "car-<snip with loads of numbers and letters>",
        "AccountName": "xowit-prod",
        "State": "SUCCEEDED",
        "RequestedTimestamp": 1650913730.571,
        "CompletedTimestamp": 1650913733.661,
        "AccountId": "1234567890"
    }
}
```

Yes. YES. You have created an account. 

Now what. [How do you access the account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_accounts_access.html)?


> When you create a new account, AWS Organizations initially assigns a password to the root user <...> randomly generated <...>. You can't retrieve this initial password. To access the account as the root user for the first time, you must go through the process for password recovery.


So you have a root user, but you shouldn't use it. 

And that, my friend is the more interesting part of this post. 

In that same page, you will see that if you create your account with the CLI, your new account has a role called `OrganizationAccountAccessRole`. And that role can be assumed by you.

So in the "old account" (management account), you must give permissions to assume the role in the new account. The new account already allows your account to assume permissions.

Let me say it again. You have a user in the management account. If you give permissions to that user has permissions to assume the role `OrganizationAccountAccessRole` in the new account, the new account already allows that and you will have access to the new account.

Even better, instead of doing the usual `aws sts assume-role`, you can have something like this in ~/.aws/config

```
[profile new_account]
role_arn = arn:aws:iam::<newaccountid>:role/OrganizationAccountAccessRole
source_profile = <profileoldaccount>
```

and with that, you can do this after creating some buckets

```
$ aws s3 mb test-xowit-org
$ aws s3 ls
2022-04-25 23:44:20 test-xowit-org
$ aws s3 mb s3://test-xowit-prod --profile xowit_prod
$ aws s3 ls --profile xowit_prod
2022-04-25 23:44:57 test-xowit-prod
```