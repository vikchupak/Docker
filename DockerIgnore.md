.dockerignore
```dockerignore
.idea
.vscode
.DS_Store
node_modules
dist
.git*
Dockerfile*
compose*.yml
README.md
```

Dockerfile
```Dockerfile
FROM node:18.16.1-alpine AS build

WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:18.16.1-alpine
WORKDIR /app
COPY package*.json ./
RUN npm install --only=production
COPY --from=build /app/dist ./dist
EXPOSE 3000

CMD ["node", "dist/main.js"]
```

***That in `.dockerignore` is `dist` won't conflict with `COPY --from=build /app/dist ./dist`?***

**No**, the `.dockerignore` file does **not** affect the `COPY --from=build` command.

---

### 🧠 Explanation:

Your `.dockerignore`:

```dockerignore
dist
```

This tells Docker:

> **When copying files from your local machine into the Docker build context, ignore `dist/`.**

But in your `Dockerfile`, this line:

```Dockerfile
COPY --from=build /app/dist ./dist
```

**copies from another image layer** (`build` stage), **not from your local machine** — so `.dockerignore` **doesn’t apply**.

---

### ✅ Summary:

| Action                  | Source                     | Affected by `.dockerignore`? |
| ----------------------- | -------------------------- | ---------------------------- |
| `COPY . .`              | Your local project folder  | ✅ Yes                        |
| `COPY --from=build ...` | Another Docker build stage | ❌ No                         |

So your `dist` in `.dockerignore` is totally fine and doesn't interfere with `COPY --from=build`.
