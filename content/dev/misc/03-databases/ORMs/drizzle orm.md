#### Step 1 - Install **node-postgres** package

```bash
npm i drizzle-orm pg dotenv
npm i -D drizzle-kit tsx @types/pg
```

#### Step 2 - Setup connection variables

Create a `.env` file in the root of your project and add your database connection variable:

```bash
DATABASE_URL=
```


#### Step 3 - Connect Drizzle ORM to the database

Create a `index.ts` file in the `src` directory and initialize the connection:

```ts
import 'dotenv/config';import { drizzle } from 'drizzle-orm/node-postgres';const db = drizzle(process.env.DATABASE_URL!);
```

#### Step 4 - Create a table

Create a `schema.ts` file in the `src/db` directory and declare your table:

`src/db/schema.ts`

```ts
import { integer, pgTable, varchar } from "drizzle-orm/pg-core";

export const usersTable = pgTable("users", {  id: integer().primaryKey().generatedAlwaysAsIdentity(),  name: varchar({ length: 255 }).notNull(),  age: integer().notNull(),  email: varchar({ length: 255 }).notNull().unique(),});
```

#### Step 5 - Setup Drizzle config file

**Drizzle config** - a configuration file that is used by [Drizzle Kit](https://orm.drizzle.team/docs/kit-overview) and contains all the information about your database connection, migration folder and schema files.

Create a `drizzle.config.ts` file in the root of your project and add the following content:

drizzle.config.ts

```ts
import 'dotenv/config';import { defineConfig } from 'drizzle-kit';
export default defineConfig({  out: './drizzle',  schema: './src/db/schema.ts',  dialect: 'postgresql',  dbCredentials: {    url: process.env.DATABASE_URL!,  },});
```

#### Step 6 - Applying changes to the database

You can directly apply changes to your database using the `drizzle-kit push` command. This is a convenient method for quickly testing new schema designs or modifications in a local development environment, allowing for rapid iterations without the need to manage migration files:

```bash
npx drizzle-kit push
```

Alternatively, you can generate migrations using the `drizzle-kit generate` command and then apply them using the `drizzle-kit migrate` command:

Generate migrations:

```bash
npx drizzle-kit generate
```

Apply migrations:

```bash
npx drizzle-kit migrate
```


#### Step 7 - Seed and Query the databas

Let’s **update** the `src/index.ts` file with queries to create, read, update, and delete users

src/index.ts

```ts
import 'dotenv/config';
import { drizzle } from 'drizzle-orm/node-postgres';
import { eq } from 'drizzle-orm';
import { usersTable } from './db/schema';
  
const db = drizzle(process.env.DATABASE_URL!);

async function main() {
  const user: typeof usersTable.$inferInsert = {
    name: 'John',
    age: 30,
    email: 'john@example.com',
  };

  await db.insert(usersTable).values(user);
  console.log('New user created!')

  const users = await db.select().from(usersTable);
  console.log('Getting all users from the database: ', users)
  /*
  const users: {
    id: number;
    name: string;
    age: number;
    email: string;
  }[]
  */

  await db
    .update(usersTable)
    .set({
      age: 31,
    })
    .where(eq(usersTable.email, user.email));
  console.log('User info updated!')

  await db.delete(usersTable).where(eq(usersTable.email, user.email));
  console.log('User deleted!')
}

main();

```

