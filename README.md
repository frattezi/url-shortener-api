# Express URL shortener API

This is a simple URL shortener API to be used as part of a course for `PodCodar`. The objective here is to create a simple express app to be used in an infrastructure course.

## Techs

- ExpressJs
- Prisma
- Nodemon
- sqlite

### Setup

```sh
# Install dependencies
yarn

# Setup database and tables
yarn migrate

# Start app with nodemon
yarn start
```

## Routes

**GET** `/shorten?={url-to-shorten}`
Return a shortened url based on the `url-to-shorten`

Sample `curl`:

```sh
curl "localhost:3333/shorten?url=google.com.br"
```

Sample response:

```sh
# Success
{"shortenedURL":"f540e22ae2","originalURL":"google.com.br","createdAt":"2023-01-08T14:56:57.760Z"}

# Error
{"error": "description"}
```

---

**GET** `/get?shortenedURL={shortened-url}`

Sample `curl`:

```sh
curl "localhost:3333/get?shortenedURL=f540e22ae2"
```

Sample response:

```sh
# Success
{"shortenedURL":"f540e22ae2","originalURL":"google.com.br","createdAt":"2023-01-08T14:56:57.760Z"}

# Error
{"error": "description"}
```

---

## PS

If nodemon presents any errors detatching a previous execution from the 3333 port, you can run:

```bash
kill -9 $(lsof -t -i:3333)
```

---

## How to setup a project like this

To setup a fresh project with:

- Typescript
- Express
- Nodemon
- Prisma

Follow this setup:

```bash
# on a folder of your choice
mkdir ts-express-api
cd ts-express-api

# creates a new package.json file
npm init

yarn add -D typescript
npx tsc --init
```

Follow the step-by-step initialization from `npm init`, you can let the default value for all options if you want.

```bash
# let's install our dependencies
yarn add express sqlite3 @prisma/client cors
yarn add -D nodemon @types/express @types/sqlite3 @types/cors ts-node 

```

Create a nodemon file with the following (file should be in the root of the project named `nodemon.json`):

```json
{
  "watch": [
    "src"
  ],
  "ext": "ts,json",
  "ignore": [
    "src/**/*.spec.ts"
  ],
  "exec": "ts-node ./src/server.ts"
}
```

In your `package.json` file, change the `scripts` key to the following:

```json
"scripts": {
    "start": "nodemon",
    "build": "tsc",
    "migrate": "npx prisma migrate dev --name init"
}
```

Basically, we just setup the:

- Node support
- Typescript support
- nodemon setup

And that's the initial modules we need to run our server, so let's do that. Create a new directory called `src` in your project root, inside this new folder create a `server.ts` file with the following content:

```ts
import express from "express";
import cors from "cors";
import { Router, Request, Response } from "express";

const app = express();
const route = Router();

app.use(cors());
app.use(express.json());

route.get("/", async (req: Request, res: Response) => {
  console.log("Request on /");
  res.json({ message: "Ok" });
});

app.use(route);
app.listen(3333, () => "server running on port 3333");
```

Go on your terminal and run the following:

```bash
# inside the project folder
yarn start

# in a new terminal, type
curl localhost:3333
```

The `curl` command should now be able to communicate with your API, which is listening on port `3333`!

### Extra, setup Prisma

At the root of the project, run:

```bash
# setup prisma using sqlite
npx prisma init --datasource-provider sqlite
```

Now, Prisma just created a new folder called `prisma`, in the `schema.prisma` file, you can define your database freely as needed.
After setting up your schemas, run:

```bash
# if you don't have prisma installed, use npx with it
# generate your prisma client
prisma generate

# run your first migration
prisma migrate dev --name init
```

Now you have an Express app with Prisma setup and running! Create your own endpoints and try Prisma out!
Please check the [prisma](https://www.prisma.io/docs) documentation for more information on how to use it in your project
