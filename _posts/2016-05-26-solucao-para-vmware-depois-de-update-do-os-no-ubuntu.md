---
layout: post
title: "Solução para o Vmware depois de update do OS no Ubuntu"
excerpt: "Como solucionar o problema do vmware player 11 depois de fazer o update do OS"
modified: {}
categories: articles
tags:
  - "ubuntu"
  - "vmware"
  - "Thinkbots"
"<!-- image":
  feature: "so-simple-sample-image-1.jpg"
  credit: WeGraphics
  creditlink: "http://wegraphics.net/downloads/free-ultimate-blurred-background-pack/ -->"
comments: true
share: true
published: true
---

Encontrei a solução:
https://communities.vmware.com/thread/509225

Para resolver o problema e so seguir os seguntes passos como Root (Em um terminal):

- Passo 1: log in as root (e.g. sudo -s)
- Passo 2: Enter your Root password.
- Passo 3: Enter these commands:

  - curl http://pastie.org/pastes/9934018/download -o /tmp/vmnet-3.19.patch
  - cd /usr/lib/vmware/modules/source
  - tar -xf vmnet.tar
  - patch -p0 -i /tmp/vmnet-3.19.patch
  - mv vmnet.tar vmnet.tar.SAVED
  - tar -cf vmnet.tar vmnet-only
  - rm -r vmnet-only
  - vmware-modconfig --console --install-all

O VMware vai compilar o vmnet module for Kernel 3.19. (Por favor verifique se tens o  DKMS instalado)

Espero que esta solução ou hack funcione para ti. No meu caso esses passos resolveram o meu problema com o VMware Player 11.

**Referência**
- http://ubuntuforums.org/showthread.php?t=2275220