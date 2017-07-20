---
layout: post
title: "Usar Php e NodeJs ao mesmo tempo no Heroku"
excerpt: "Heroku é uma plataforma de serviço em nuvem (PaaS) suportando várias linguagens de programação. Heroku é de propriedade da Salesforce.com . "
modified: 2017-07-20
categories: articles
tags:
  - "php"
  - "nodejs"
  - "Programming"
  - "Website"
  - "Webpage"
  - "Thinkbots"
comments: true
share: true
published: true
---

- Tens de informar o 'build api' que gostarias de usar varios buildpacks. Tenta

  ~~~~
    $ heroku buildpacks:clear
    $ heroku buildpacks:add heroku/php
    $ heroku buildpacks:add heroku/nodejs
  ~~~~

- Depois disso tens de verificar os slugs compilados via heroku run bash para verificar que os bins estão onde querias que eles estivessem
