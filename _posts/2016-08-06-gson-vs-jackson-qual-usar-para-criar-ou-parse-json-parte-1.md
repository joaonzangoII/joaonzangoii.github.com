---
layout: post
title: "Gson vs Jackson: Qual usar para criar ou parse Json Parte 1"
excerpt: "Como usar as duas mais poderosas livrarias para parsear Json num programa usando JAVA"
modified: {}
categories: articles
tags:
  - "Java"
  - "Json"
  - "Programming"
  - "Thinkbots"
comments: true
share: true
published: true
---

No mundo da programação de software livre existem sempre curiosos a desenvolvedores a criarem livrarias para facilitar a sua propria vida tanto como a vida de outros desenvolvedores de software e programadores.

Desta feita faremos uma pequena comparação entre duas poderosas livrarias usadas para parsear JSON que significa Notação de objeto JavaScript (<em>JavaScript Object Notation</em>)

E de salientar que ainda existem outras livrarias usadas para parsear json como:

- Yidong Fang’s JSON.simple <a href="https://github.com/fangyidong/json-simple">(https://github.com/fangyidong/json-simple)</a>
- Oracle’s JSONP <a href="https://jsonp.java.net">(https://jsonp.java.net)</a>

Tendo ja usado as duas livrarias na qual faremos as nossas comparaçoes, posso ja adiantar que a difrerenca entre as duas e mais notavel na forma como as duas foram escritas e desenvolvidas do que em termos de velocidade apesar que os numeros mostrarao algo totalmente diferente baseados em certos benchmarks ja feitos.

Em primeiro lugar vamos ver exemplos de como as duas livrarias sao usadas antes de comecar a comparacao em termos de velocidade e de como cada se sai em termos de parsear grandes arquivos JSON.

## Exemplo 1: Usando o Jackson

Para  testar o codigo e necesarrio criar um arquivo JSON chamado grandeJson.json e depois copiar para que ele use os APIs do Jackson. Podes muito bem usar os seus dados proprios em vez de usar os ja recem criados. Podem tambem ser usador criadores de JSON online.

{% highlight java %}

  package atarito.nzango.json.exemplo;

  import com.fasterxml.jackson.annotation.JsonInclude.Include;
  import java.io.File;
  import java.io.IOException;
  import java.util.Map;
  import org.codehaus.jackson.map.ObjectMapper;
  import org.codehaus.jackson.map.annotate.JsonSerialize;


  /**
   *
   * @adaptado do exemplo de steve jin (http://www.doublecloud.org)
   */

  public class JacksonDemo
  {
    private static ObjectMapper mapper = new ObjectMapper();

    public static void main(String[] args) throws IOException
    {
      parseJson();
      saveJson();
      perfTest();
    }

    public static void parseJson() throws IOException
    {
      String jsonStr = "{ \"autor\": \"Victor Hugo Mendes\", \"titulo\" : \"A juventude e a leitura\", \"obj\" : {\"objint\" : {}} }";

      // parsing JSON to generic object
      Object obj = mapper.readValue(jsonStr, Object.class);
      // java.util.LinkedHashMap
      System.out.println("obj type: " + obj.getClass().toString());
      System.out.println("obj: " + obj);

      // parsing JSON to Map object
      Map m = mapper.readValue(jsonStr, Map.class);
      System.out.println("map.size: " + m.size());
      for(Object key: m.keySet())
      {
        System.out.println("key:" + key);
      }

      // Parseando o JSON para um objecto concreto
      Livro livro = mapper.readValue(jsonStr, Livro.class);
      System.out.println("livro: " + livro);
      System.out.println("livro.autor: " + livro.autor);
      //com.google.gson.internal.LinkedTreeMap
      System.out.println("livro.obj class: " + livro.obj.getClass());
      System.out.println("livro.obj: " + livro.obj);
    }

    public static void saveJson() throws IOException
    {
      Livro livro = new Livro();
      livro.autor = "Victor Hugo Mendes";
      livro.titulo = "A juventude e a leitura";

      mapper.setSerializationInclusion(JsonSerialize.Inclusion.NON_EMPTY);
      String livroJson = mapper.writeValueAsString(livro);
      System.out.println("livroJson: " + livroJson);
    }

    public static void perfTest() throws IOException
    {
      long start = System.nanoTime();

      mapper.readValue(new File("src/main/resources/grandeJson.json"), Map[].class);

      long end = System.nanoTime();
      System.out.println("Tempo levado a parsear o JSON (nano segundos): " + (end - start));
    }
  }

  class Livro
  {
    public String autor;
    public String titulo;
    public Map obj;
  }
{% endhighlight %}


### Dependencias

- Maven
  {% highlight css%}
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.2.3</version>
      <type>jar</type>
    </dependency>
  {% endhighlight %}



**Referência**

- <a href="http://blog.takipi.com/the-ultimate-json-library-json-simple-vs-gson-vs-jackson-vs-json/">http://blog.takipi.com/the-ultimate-json-library-json-simple-vs-gson-vs-jackson-vs-json/</a>

- <a href="http://www.doublecloud.org/2015/05/how-to-pretty-print-json-with-gson-and-jackson/">http://www.doublecloud.org/2015/05/how-to-pretty-print-json-with-gson-and-jackson/</a>
