---
title: "Elixir/Phoenix - UUID como chave primária"
date: 2020-05-21T18:29:29-03:00
draft: true
tags: [elixir, phoenix, uuid]
---

## Mas porque usar um UUID ao invés de um INT como chave primária?

Esse texto não é sobre isso. Existem vários argumentos contra e a favor do uso do UUID como chave primária e a melhor solução sempre depende das variáveis do problema que você está tentando resolver. Mas, se você chegou a conclusão de que o UUID é a melhor opcão pro seu cenário...

## Como usar UUID como chave primária com Elixir/Phoenix

Como exemplo, vamos usar o _schema_ `User` e o primeiro passo vai ser definir o atributo `@primary_key` do [Ecto.Schema](https://hexdocs.pm/ecto/Ecto.Schema.html) nesse modulo.

```ex
defmodule MyApp.User do
  use Ecto.Schema

  @primary_key {:uuid, :binary_id, autogenerate: true}

  schema "users" do
    field :name, :string
    field :email, :string
  end
end

```

Em seguida, precisamos criar uma _migration_ pro nosso _schema_. Aqui, estamos desativando a geração automatica da _primary key_ e criando manualmente a coluna id do tipo **uuid**.

```ex
# migration para tabela users
def change do
    create table(:users, primary_key: false) do
      add :id, :uuid, primary_key: true
      add :name, :string
      add :email, :string
    end
end
```

## E quando _User_ for chave estrangeira de outra entidade?

Nessa caso, será necessário definir um outro atributo de [Ecto.Schema](https://hexdocs.pm/ecto/Ecto.Schema.html), o `@foreign_key_type`. Vamos supor que estamos desenvolvendo um _e-commence_ e que cada _order_ pertence a um _user_. O modulo `MyApp.Order` ficaria assim:

```ex
defmodule MyApp.Order do
  use Ecto.Schema

  @foreign_key_type :binary_id

  schema "orders" do
    field :products, {:array, :string}
    field :price, :float

    belongs_to :user, MyApp.User
  end
end
```

Na _migration_ do _schema_ `order`, você também vai precisar explicitar o tipo da chave estrangeira.

```ex
# migration para tabela orders
def change do
    create table(:orders) do
      add :products, {:array, :string}
      add :price, :float

      add :user_id, references(:users, type: :uuid), null: false
    end
end
```
