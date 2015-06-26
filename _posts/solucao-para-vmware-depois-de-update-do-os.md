---
layout: post
title: "O que é o Ubuntu?"
excerpt: "O que é o ubuntu OS, venha saber um pouco mais acerca deste sistema operacional"
modified: {}
categories: articles
tags: 
  - "sample-post"
"<!-- image": 
  feature: "so-simple-sample-image-1.jpg"
  credit: WeGraphics
  creditlink: "http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/ -->"
comments: true
share: true
published: true
---

Encontrei a solucao:
https://communities.vmware.com/thread/509225

This can be fixed by running the following Passos as Root (in a terminal):

Passo 1: log in as root (e.g. sudo -s)
Passo 2: Enter your Root password.
Passo 3: Enter these commands: 

curl http://pastie.org/pastes/9934018/download -o /tmp/vmnet-3.19.patch

cd /usr/lib/vmware/modules/source
tar -xf vmnet.tar
patch -p0 -i /tmp/vmnet-3.19.patch
mv vmnet.tar vmnet.tar.SAVED
tar -cf vmnet.tar vmnet-only
rm -r vmnet-only
vmware-modconfig --console --install-all

VMware will now compile the vmnet module for Kernel 3.19. (please make sure you have DKMS installed)

Hope it will work for you. In my case this solved the problem with VMware Player 11.

Referencia: http://ubuntuforums.org/showthread.php?t=2275220