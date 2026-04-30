---
title: "Drizzle - ORM"
tags: ["database", "orm"]
draft: true
---

# What is an ORM?





# Instalation

`npm install drizzle-orm`: ORM itself

`npm install drizzle-kit --save-dev`: migrations + schema tooling

`npm install pg`: postgreSQL driver

# Configuration

The configuration lives at root:  `drizzle.config.ts`:


```ts
import { defineConfig } from "drizzle-kit";
export default defineConfig({
  dialect: "postgresql",
  schema: "./lib/db/schema.ts",
  out: "./drizzle",
  dbCredentials: {
    url: process.env.DATABASE_URL!,
  },
});
```

# Schema definition

The schema definition is done in the specified schema location

```ts
import { pgTable, serial, text, timestamp } from "drizzle-orm/pg-core";

export const posts = pgTable("posts", {
  id: serial("id").primaryKey(),
  title: text("title").notNull(),
  content: text("content"),
  createdAt: timestamp("created_at").defaultNow(),
});
```

This file becomes the single source of truth, defining:
1. Database structure
2. Typescript types
3. Migrations

## Sintax

Types: 
- serial: auto-increment integer
- text, integer, timestamp

Constraints:
- `.notNull()`
- `.references(() => users.id )`: foreign key reference
- `.primaryKey()`



# Migrations

### Generation `npx drizzle-kit generate`
### Apply `npx dirzzle-kit apply`

# Using at runtime

Create in `src/db/index.ts` the following file:

```ts
import { drizzle } from "drizzle-orm/node-postgres";
import { Pool } from "pg";
import * as schema from "./schema";

const pool = new Pool({
  connectionString: process.env.DATABASE_URL,
});

export const db = drizzle(pool, { schema });
```

Using it later:

```ts
import { db } from "@/db";
import { posts } from "@/db/schema";

export async function getPosts() {
  return db.select().from(posts);
}
```
```ts
await db.insert(posts).values(
    {title: "Hello", content: "My first post",},
    {title: "Goodbye", content: "My last post",}
);

await db
  .update(posts)
  .set({ content: "Deleted post" })
  .where(posts.title.eq("Temporary message"));

await db
    .delete(posts)
    .where(posts.content.eq("another"))


```

## Sintax

| Operators        | syntax        |
| :--------------- | :------------ |
| equals           | .eq(value)    |
| not equlas       | .notEq(value) |
| greater than     | .gt(value)    |
| less than        | .lt(value)    |
| greater or equal | .gte(value)   |
| less or equal    | .lte(value)   |

### Joins

```ts
import { posts } from "./db/schema";

const postsWithAuthors = await db
  .select({
    postTitle: posts.title,
    authorName: users.name,
  })
  .from(posts)
  .leftJoin(users, users.id.eq(posts.authorId));
```

### Aggregates, Counts, Limits

- `db.fn.count(column)`: SQL COUNT
- `.orderBy(column.asc()/desc())`: Sorting
- `.limit(n)`: limit nº rows



