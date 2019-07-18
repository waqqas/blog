---
published: false
title: "Continuous Remote syncing"
cover_image: "https://raw.githubusercontent.com/maxime1992/my-dev.to/master/blog-posts/manage-dev-to-blog-posts-with-continuous-deployment/assets/github-travis-dev-to.png"
description: "tags: ssh, sync"
series:
canonical_url:
---

I have been working on a project where the main development environment was linux

- Install watchman
- Generate ssh key-pair
- Install key-pair on remote system
- Watch filesystem
- Install Watchman

## Install watchman

https://facebook.github.io/watchman/docs/install.html

## Generate SSH keys (replace with relevant details)

`ssh-keygen -t rsa -b 4096 -C waqqas.jabbar@gmail.com`

Don’t give any password

`ssh-copy-id waqqas@192.168.1.26`

## Watch filesystem

Create file sync.sh with following code in your home directory
~/sync.sh:

```
for i in $@
do
   rsync $i waqqas@remote:/home/waqqas/code/$i
done
```

## Watch filesystem

`watchman watch code/`

`watchman — trigger . rsync — sh ~/sync.sh`
