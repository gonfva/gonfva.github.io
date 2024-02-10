---
date: 2024-02-10T10:30:00
title: Python 3.11 in Ubuntu Xenial 16.04
subtitle: Reviving an old AWS Deeplens
tags:
  - Developer
categories: [Developer]
---

I've got a Deeplens. It was a gift that I received when I left my previous company. I played with it for a while when I got it. 

But I must confess never much. 

Last year, Amazon [announced](https://aws.amazon.com/deeplens/faqs/) that it was retiring everything Deeplens related

> After January 31, 2024, all references to AWS DeepLens models, projects, and device information are deleted from the AWS DeepLens service. You can no longer discover or access the AWS DeepLens service from your AWS console and applications that call the AWS DeepLens API no longer work.

It's been inside a drawer for a while, but a few days ago I decided I could maybe give it an opportunity. It is not a very powerful device, but maybe it could act as a tiny Internet server (low power consumption) for a project I've got in mind. I've got a USB ethernet adapter, so I don't need the wifi which is said to be unreliable.

It runs Ubuntu Xenial 16.04, with [no possibility to upgrade to a more recent distribution](https://repost.aws/questions/QUj7PugVYbS3OraD9bhZuZdw/deeplens-ubuntu-20-04). 

I thought that was the end of my idea.

Xenial was launched in April 2016, 8 years ago!!!

It turns out there are still [security updates, and end of life is April 2026](https://wiki.ubuntu.com/Releases)!!!! That's amazing!!!!

So I managed to updated. 

And I thought "I can live with whatever is there, as long as it is updated regularly. Can I?"

```
$ python3
Python 3.5.2 (default, Nov 12 2018, 13:43:14) 
[GCC 5.4.0 20160609] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> quit()
```

Damn it, python3.5 is ancient. 

I want to use Django. I need [at least 3.8, preferably 3.10 for Django 5](https://docs.djangoproject.com/en/5.0/faq/install/). Yes, I know I can probably live with docker. But this machine is already restricted as it is, I would prefer to not use containers.

Let's Google. This `dadsnakes/ppa` looks promising ... [until you discover 16.04 is no longer supported](https://gist.github.com/ptantiku/aca8d955296d5dee01bd9ed1c3027d8c).

Is this a new dead end?

Small idea. Could I install it from source?

Well, it turns out you can, but you need to install OpenSSL v1.1.1 first. So for those impatient that arrive through Google to this page...

Here a recipe to install Openssl 1.1.1w

```
cd /tmp
wget https://github.com/openssl/openssl/releases/download/OpenSSL_1_1_1w/openssl-1.1.1w.tar.gz
tar -xvf openssl-1.1.1w.tar.gz 
cd openssl-1.1.1w/
./config shared --prefix=/usr/local/ --openssldir=/usr/local
make
sudo make install
```

And then a recipe for python 3.11 !!!
```
cd /tmp
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

So yes, python 3.11 on Ubuntu Xenial

I hope this helps. Please ping me if there is anything missing.