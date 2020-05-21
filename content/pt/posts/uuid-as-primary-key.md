---
title: "Elixir/Phoenix - UUID como chave primária"
date: 2020-05-21T18:29:29-03:00
draft: true
---

Antes de mais nada...

## Porque usar um UUID ao invés de um INT como chave primária?

Assim como em qualquer dicussão, existem argumentos contra e a favor do uso de UUIDs (**U**niversally **U**nique **I**dentifier) como chaves primárias. Os **contra**, normalmente, argumentam sobre a performance e armazenamento do seu banco de dados. Os argumentos **a favor** costumam falar sobre não exposição de informação, ambientes distribuídos e, obviamente, unicidade.

Esse texto não é sobre isso. A melhor solução depende sempre depende do problema que você quer resolver e do contexto em que você se encontra. Mas se você chegou a conclusão de que o UUID é a melhor opcão para o seu cenário...

## Como usar UUID como chave primária com Elixir, Phoenix e Postgres


https://rclayton.silvrback.com/do-you-really-need-a-uuid-guid
https://kccoder.com/mysql/uuid-vs-int-insert-performance/
https://www.percona.com/blog/2019/11/22/uuids-are-popular-but-bad-for-performance-lets-discuss/
https://medium.com/@jdedek/using-uuids-as-primary-keys-ca1fb409bb7c
