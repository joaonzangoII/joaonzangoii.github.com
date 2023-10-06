---
layout: post
title: "Fundamentos de Python - Introducão"
excerpt: "Curso básico de aprendizado de python"
modified: 2023-07-02
categories: articles
tags: 
  - "Python"
  - "Learning"
  - "Py"
  - "Matlab"
  - "Matlab"
comments: true
share: true
published: true
---
 
Erro ao instalar o eventmachine no MacOS quando tens instalado o Ruby 3 e usa o chip M1.

Ao instalar o eventmachine gem num Macbook/iMac com o Chip M1 tendo o ruby 3 instalado rodando o seguinte comando:

~~~~
gem install eventmachine 
~~~~

Aparece o seguinte erro durante a instalação:

~~~~
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /Users/[$USER]/.rvm/gems/ruby-3.1.2/gems/eventmachine-1.2.7/ext
/Users/[$USER]/.rvm/rubies/ruby-3.1.2/bin/ruby -I /Users/[$USER]/.rvm/rubies/ruby-3.1.2/lib/ruby/3.1.0 -r ./siteconf20221113-73057-dr6wt2.rb extconf.rb
-----
Using OpenSSL from pkg-config -I/opt/homebrew/Cellar/openssl@3/3.0.7/include  && -L/opt/homebrew/Cellar/openssl@3/3.0.7/lib && -lssl -lcrypto
~~~~

Para resolver o problema é so rodar o seguinte comando para que o mac utilize a versão openssl@1.1. 
~~~~
gem install eventmachine -- --with-openssl-dir=/opt/homebrew/opt/openssl@1.1
~~~~