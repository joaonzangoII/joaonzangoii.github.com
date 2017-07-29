---
layout: post
title: "Como Implementar o Rabbitmq"
excerpt: "Como fazer o download e usar o rabbitmq. Como Laravel usando o pacote mookofe/tail de Victor Cruz"
modified: 2017-07-20
categories: articles
tags:
  - "php"
  - "Laravel"
  - "RabbitMQ"
  - "AMQP"
  - "Programming"
  - "Thinkbots"
  - "IvangoTi"
comments: true
share: true
published: true
---

**Introdução**

A implementação de filas pode ser muito complicadas de manter e gerir, mas o que são <b>filas</b>, <b>AMQP</b> or <b>RabbitMQ?</b>

Nos dias de hoje estamos sempre precisando de novos requisitos para a construção dos sistemas.
Um dos maiores requisitos é a comunicação assíncrona e replicação de sistemas,
para isto, a utilização de filas é bastante comum.

**O que é o AMQP?**

O AMQP (Advanced Message Queuing Protocol) é um protocolo de comunicação em rede,
tal qual o HTTP, que permite que aplicações se comuniquem.

Desde os anos 1970 que existem soluções criadas para mensageria que visam resolver problemas de integração de variados serviços e fornecedores. Sem o uso de um middleware (serviço intermediário) para mensagens, a integração heterogènea de sistemas mostrou-se ser um
processo muito caro e complexo de se implementar.

O AMQP tem muitas vantagens, mas duas são mais importantes: produzir um padrão aberto para protocolos de mensageria e permitir a interoperabilidade entre muitas tecnologias e plataformas.

**O Broker e o seu papel**

Os Brokers de mensagens recebem mensagens de seus criadores (aplicações que publicam mensagens) e as enviam até seus consumidores (aplicações que processam mensagens).

Se formos a usar o corpo humano como exemplo, podemos dizer que os brokers funcionariam como o Sistema Nervoso Central e as aplicações seriam os membros do corpo (braços, pernas, cabeça e outros).

**RabbitMQ**

RabbitMQ é um software de código aberto (open source) que foi implementado para suportar um protocolo de mensagens denominado Advanced Message Queuing Protocol (AMQP). Através da solução, é possível criar uma aplicação para lidar com o tráfego de mensagens que estão no cerne de sistemas de informação.

A ideia do RabbitMQ é disponibilizar uma estrutura que facilite fluxos de mensagens, sobretudo em grandes aplicações, para a comunicação entre todos os processos.

A solução foi desenvolvida  em 2007 pela empresa Rabbit Technologies Ltd., Em 2010, foi adquirida por SpringSource, uma divisão da VMware.

Outras características do RabbitMQ:

- É desenvolvido em Erlang.
- É considerado rápido e confiável.
- Compatível com os principais sistemas operacionais.
- Suporta diversas plataformas de desenvolvimento. Bibliotecas de conexão com o RabbitMQ estão disponíveis em diversas linguagens de programação.

**Baixar e usar o RabbitMQ**

**Baixar**

Abra um terminal e digite os seguintes comandos:

~~~~ bash
sudo apt-add-repository 'deb http://www.rabbitmq.com/debian/ testing main'
curl http://www.rabbitmq.com/rabbitmq-signing-key-public.asc | sudo apt-key add -
sudo apt-get update
sudo apt-get install rabbitmq-server

sudo rabbitmq-plugins enable rabbitmq_management
sudo service rabbitmq-server restart
wget http://localhost:15672/cli/rabbitmqadmin
chmod +x rabbitmqadmin
sudo mv rabbitmqadmin /usr/local/sbin
~~~~

**Usar o RabbitMQ em PHP**

Vamos usar  php-amqplib e o [Composer](https://getcomposer.org/doc/00-intro.md)
para gerir as dependencias do projecto.

- Adicione o ficheiro composer.json no seu projecto.

```composer
  {
    "require": {
        "php-amqplib/php-amqplib": ">=2.6.1"
    }
  }
```

- Caso tenhas o composer instalado no programa digite o seguinte commando em seu terminal

```bash
  composer.phar install
```

- Com a livraria php-amqplib/php-amqplib instalada podemos escrever algum código.

- Crie dois ficheiros enviar.php e receber.php

- Em enviar.php adicione o seguinte código.

```php
  <?php
    //Incluir as classes necessárias
    require_once __DIR__ . '/vendor/autoload.php';
    use PhpAmqpLib\Connection\AMQPStreamConnection;
    use PhpAmqpLib\Message\AMQPMessage;

    // Criar a conecção ao servidor
    $connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
    $channel = $connection->channel();

    //Para enviar temos de declarar uma fila e para onde vamos enviar as mensagens e depois podemos publica-las

    $channel->queue_declare('ola', false, false, false, false);
    $msg = new AMQPMessage('Hello World!');
    $channel->basic_publish($msg, '', 'hello');

    echo " [x] Sent 'Olá Mundo, Olá Angola!'\n";

    // No fim fechamos o canal (channel) e a conecção
    $channel->close();
    $connection->close();
```
- Em receber.php adicione o seguinte código.

```php
  <?php
    //Incluir as classes necessárias
    require_once __DIR__ . '/vendor/autoload.php';
    use PhpAmqpLib\Connection\AMQPStreamConnection;

    $connection = new AMQPStreamConnection('localhost', 5672, 'guest', 'guest');
    $channel = $connection->channel();

    $channel->queue_declare('ola', false, false, false, false);

    echo ' [*] Esperando por mensagens. Para sair pressione CTRL+C', "\n";

    $callback = function($msg) {
      echo " [x] Recebido ", $msg->body, "\n";
    };

    $channel->basic_consume('ola', '', false, true, false, false, $callback);

    while(count($channel->callbacks)) {
      $channel->wait();
    }
```

- Volte para o terminal e execute:


```bash
php send.php
```

- E

```bash
php receive.php
```

- Para ouir as filas

``` bash
sudo rabbitmqctl list_queues
```


Para usar o RabbitMQ com o Laravel basta seguir os passos em <a
href="https://github.com/mookofe/tail">
https://github.com/mookofe/tail</a> todos os detalhes estão bem explicitos


**Referência**

- [https://victorcruz.me/rabbitmq-client-for-laravel](https://victorcruz.me/rabbitmq-client-for-laravel)
- [http://www.pognao.com.br/2013/04/rabbitmq.html](http://www.pognao.com.br/2013/04/rabbitmq.html)
- [http://www.devmedia.com.br/introducao-ao-amqp-com-rabbitmq/33036](http://www.devmedia.com.br/introducao-ao-amqp-com-rabbitmq/33036)
- [https://www.portalgsti.com.br/rabbitmq/sobre](https://www.portalgsti.com.br/rabbitmq/sobre/)
- [https://www.rabbitmq.com/tutorials/tutorial-one-php.html](https://www.rabbitmq.com/tutorials/tutorial-one-php.html)
