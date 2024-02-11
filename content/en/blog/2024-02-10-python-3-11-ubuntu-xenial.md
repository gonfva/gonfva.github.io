---
date: 2024-02-10T10:30:00
title: Python 3.11 in Ubuntu Xenial 16.04
subtitle: Reviving an old AWS Deeplens
tags:
  - Developer
categories: [Developer]
---

## Deeplens, is it dead?

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

Damn it, **python3.5**. That's ancient. 

I want to use Django. I need [at least Python 3.8, preferably 3.10 for Django 5](https://docs.djangoproject.com/en/5.0/faq/install/). Yes, I know I can probably get away with docker. But this machine is already restricted as it is, I would prefer to not use containers.

Let's Google. 

Hmm. This `dadsnakes/ppa` looks promising ... [until you discover 16.04 is no longer supported](https://gist.github.com/ptantiku/aca8d955296d5dee01bd9ed1c3027d8c).

Is this a new dead end?

Small idea. Could I install it from source?

## Installation from source

Well, it turns out you can, but you need to install a few packages first.

First, **OpenSSL v1.1.1**. The version installed in the system is 1.0.2g, not good enough for Python. You need 1.1.1. 

I went for installing openssl from source as well. Here a recipe to install Openssl 1.1.1w

```
cd /tmp
wget https://github.com/openssl/openssl/releases/download/OpenSSL_1_1_1w/openssl-1.1.1w.tar.gz
tar -xvf openssl-1.1.1w.tar.gz 
cd openssl-1.1.1w/
./config shared
make
sudo make install
```

It seems for python we need the source. I discovered this late, and I didn't want to recompile, so I did the following:

```
sudo mv /tmp/openssl-1.1.1w /usr/src/
cd /usr/src/openssl-1.1.1w
mkdir lib
cp ./*.{so,so.*,a,pc} ./lib
```

OK. openssl done. Next.

You will also need **sqlite development library** (libsqlite3-dev). 

Here there is a misconfiguration in the packages (`libsqlite3-dev : Depends: libsqlite3-0 (= 3.11.0-1ubuntu1) but 3.11.0-1ubuntu1.1 is to be installed` ), dev requires almost the same version, so I just hacked my way out. Not ideal, but sue me:

```
wget http://launchpadlibrarian.net/241463272/libsqlite3-0_3.11.0-1ubuntu1_amd64.deb
wget http://launchpadlibrarian.net/241463275/libsqlite3-dev_3.11.0-1ubuntu1_amd64.deb
sudo apt install ./libsqlite3-0_3.11.0-1ubuntu1_amd64.deb
sudo apt install ./libsqlite3-dev_3.11.0-1ubuntu1_amd64.deb
```

And you may also need a few extra packages. But those comes from the apt repos fine
```
sudo apt install libffi-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libreadline-gplv2-dev libncursesw5-dev uuid-dev
```

Tkinter is a different beast. I thought tk-dev would be enough, but I was getting missing

```
checking for stdlib extension module _tkinter... missing
```

It turns out passing environment variables (see below) seem to fix it.

OK. You've done all the pre-requisites. Funnily enough they are not hard pre-req. They won't crash the compilation. They will just appear as a bunch of things like `Could not build the ssl module!` and `fatal error: ffi.h: No such file or directory` in the logs but the process will keep working without those modules. 

But after the pre-requisites, the recipe for **python 3.11** itself.

```
cd /tmp
wget https://www.python.org/ftp/python/3.11.8/Python-3.11.8.tgz
tar -xvf Python-3.11.8.tgz
cd Python-3.11.8/
export TCLTK_CFLAGS=-I/usr/include/tcl8.6
export TCLTK_LIBS="-ltcl8.6 -ltk8.6"
./configure --with-openssl=/usr/src/openssl-1.1.1w --enable-optimizations
make
sudo make install
```

And yes, python 3.11 on Ubuntu Xenial. 

```
$ python3 --version
Python 3.11.8
```

```
$ pip3 --version
pip 24.0 from /usr/local/lib/python3.11/site-packages/pip (python 3.11)
```

Remember that when installing from source, you're responsible of updating when a new version appears (there is no package manager for them),

I hope this helps. Please ping me if there is anything missing.