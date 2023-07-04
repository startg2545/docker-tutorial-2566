## Vercel Example#1 : with-docker

https://github.com/vercel/next.js/tree/canary/examples/with-docker

### Create project

```bash
$ npx create-next-app --example with-docker nextjs-docker
```

### Adding Dockerfile

https://github.com/vercel/next.js/blob/canary/examples/with-docker/Dockerfile

### Build Docker image

```bash
$ docker build -t nextjs-docker .
```

### Run a container

```bash
$ docker run -p 3000:3000 nextjs-docker
```


### Edit API : <project-folder>/pages/api/hello.js

```javascript
export default function hello(req, res) {
  let students = [
    {
      id: '650610123',
      name: 'Peter Hamilton',
      gpax: 3.45,
      isMale: true,
      grades: {'259201':'A','259104':'B+','259106':'B+'},
      hobbies: ['gaming','running','K-drama'],
    },
    {
      id: '650610203',
      name: 'Thanya Taylor',
      gpax: 3.60,
      isMale: false,
      grades: {'259201':'B+','259104':'A','259106':'B'},
      hobbies: [],
    }
  ]
  res.status(200).json(students)
}
```

## Vercel Example#2 : api-route-rest

- https://github.com/vercel/next.js/tree/canary/examples/api-routes-rest
- https://geshan.com.np/blog/2023/01/nextjs-docker/

### Adding Dockerfile

```dockerfile
FROM node:18-alpine AS deps
RUN apk add --no-cache libc6-compat
WORKDIR /app

COPY package.json package-lock.json ./
RUN  npm install --production

FROM node:18-alpine AS builder
WORKDIR /app
COPY --from=deps /app/node_modules ./node_modules
COPY . .

ENV NEXT_TELEMETRY_DISABLED 1

RUN npm run build

FROM node:18-alpine AS runner
WORKDIR /app

ENV NODE_ENV production
ENV NEXT_TELEMETRY_DISABLED 1

RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 nextjs

COPY --from=builder --chown=nextjs:nodejs /app/.next ./.next
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package.json ./package.json

USER nextjs

EXPOSE 3000

ENV PORT 3000

CMD ["npm", "start"]

```

### Adding docker-compose.yml

```yaml
version: '3.8'
services:
  web:
    build:
      context: ./
      target: runner
    volumes:
      - .:/app
    command: npm run dev
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: development

```

### Build Docker image 

```bash
$ COMPOSE_DOCKER_CLI_BUILD=1 DOCKER_BUILDKIT=1 docker-compose build
```

### Run a container

```bash
$ docker-compose up [-d]
```