# Express URL shortener API

This is a simple URL shortener API to be used as part of a course for `PodCodar`. The objective here is to create a simple express app to be used in a infraestructure course.

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
