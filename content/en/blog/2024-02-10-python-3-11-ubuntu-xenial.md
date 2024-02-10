---
date: 2024-01-04T10:30:00
title: Python 3.11 in Ubuntu Xenial 16.04
subtitle: Reviving an old AWS Deeplens
tags:
  - Developer
categories: [Developer]
---

I've got a Deeplens. It was a gift from when I left my previous company. I played with it for a while when I got it but I must confess never much. 

Last year, Amazon [announced](https://aws.amazon.com/deeplens/faqs/) that it was retiring everything Deeplens related

<blockquote>
After January 31, 2024, all references to AWS DeepLens models, projects, and device information are deleted from the AWS DeepLens service. You can no longer discover or access the AWS DeepLens service from your AWS console and applications that call the AWS DeepLens API no longer work.
</blockquote>

It's been inside a drawer for a while, but a few days ago I decided I could maybe give it an opportunity. It is not a very powerful device, but maybe it could act as an Internet server for a project I've got in mind.

First things first, it runs Ubuntu Xenial 16.04, with [no possibility to upgrade to a more recent distribution](https://repost.aws/questions/QUj7PugVYbS3OraD9bhZuZdw/deeplens-ubuntu-20-04). 

I thought that was it. Xenial was launched in April 2016, 8 years ago!!!

It turns out there are still [security updates, and end of life is April 2026](https://wiki.ubuntu.com/Releases)!!!! That's amazing!!!!

OK. Let's update. That looks fine.

I can live with whatever is there, as long as it is updated regularly. Can I?

```
$ python3
Python 3.5.2 (default, Nov 12 2018, 13:43:14) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()
```

Damn it, python3.5 is ancient. I want to use Django. I need [at least 3.8, preferably 3.10 for Django 5](https://docs.djangoproject.com/en/5.0/faq/install/). Yeah, I know I can probably leave with docker. But this machine is already restricted as it is, I would prefer to not use containers.

Let's Google. This `dadsnakes/ppa` looks promising ... [until you discover 16.04 is no longer supported](https://gist.github.com/ptantiku/aca8d955296d5dee01bd9ed1c3027d8c).

Is this a dead end?

Could I install it from source?

Well, it turns out you can, but you need to install OpenSSL v1.1.1 first. So for those impatient that arrive through Google to this page...

First a recipe for Openssl

```
cd /tmp
wget https://github.com/openssl/openssl/releases/download/OpenSSL_1_1_1w/openssl-1.1.1w.tar.gz
tar -xvf openssl-1.1.1w.tar.gz 
cd openssl-1.1.1w/
./config shared --prefix=/usr/local/
make
sudo make install
```

And then a recipe for python 3.11 !!!
```
wget https://www.python.org/ftp/python/3.11.8/Python-3.11.8.tgz
tar -xvf Python-3.11.8.tgz
cd Python-3.11.8/
./configure --with-openssl=/tmp/openssl-1.1.1w --enable-optimizations
make
sudo make install
```

And yes

```
aws_cam@AWSDeepLens:~$ cat /etc/os-release 
NAME="Ubuntu"
VERSION="16.04.4 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.4 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
VERSION_CODENAME=xenial
UBUNTU_CODENAME=xenial
aws_cam@AWSDeepLens:~$ python3 --version
Python 3.11.8
```

I hope this helps. Please ping me if there is anything missing.