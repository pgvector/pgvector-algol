# pgvector-algol

[pgvector](https://github.com/pgvector/pgvector) examples for Algol

Supports [Algol 68 Genie](https://jmvdveer.home.xs4all.nl/en.algol-68-genie.html)

[![Build Status](https://github.com/pgvector/pgvector-algol/actions/workflows/build.yml/badge.svg)](https://github.com/pgvector/pgvector-algol/actions)

## Getting Started

Follow the instructions for your database library:

- [Algol 68 Genie](#algol-68-genie)

## Algol 68 Genie

Enable the extension

```algol
pq exec (db, "CREATE EXTENSION IF NOT EXISTS vector");
```

Create a table

```algol
pq exec (db, "CREATE TABLE items (id bigserial PRIMARY KEY, embedding vector(3))");
```

Insert vectors

```algol
pq exec (db, "INSERT INTO items (embedding) VALUES ('[1,2,3]'), ('[4,5,6]')");
```

Get the nearest neighbors

```algol
pq exec (db, "SELECT id FROM items ORDER BY embedding <-> '[3,1,2]' LIMIT 5");
FOR i TO pq ntuples (db)
DO pq getvalue (db, i, 1);
  print((res, newline))
OD;
```

Add an approximate index

```algol
pq exec (db, "CREATE INDEX ON items USING hnsw (embedding vector_l2_ops)");
# or #
pq exec (db, "CREATE INDEX ON items USING ivfflat (embedding vector_l2_ops) WITH (lists = 100)");
```

Use `vector_ip_ops` for inner product and `vector_cosine_ops` for cosine distance

See a [full example](example.a68)

## Contributing

Everyone is encouraged to help improve this project. Here are a few ways you can help:

- [Report bugs](https://github.com/pgvector/pgvector-algol/issues)
- Fix bugs and [submit pull requests](https://github.com/pgvector/pgvector-algol/pulls)
- Write, clarify, or fix documentation
- Suggest or add new features

To get started with development:

```sh
git clone https://github.com/pgvector/pgvector-algol.git
cd pgvector-algol
createdb pgvector_algol_test
a68g example.a68
```
