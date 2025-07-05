FROM node:22-slim AS base

ENV PNPM_HOME="/pnpm"
ENV PATH="$PNPM_HOME:$PATH"

RUN corepack enable
COPY . /app
WORKDIR /app

FROM base AS prod-deps
RUN pnpm install --prod --frozen-lockfile

FROM base AS build
RUN pnpm install && pnpm run build

FROM base
COPY --from=build /app/dist /app/dist
COPY --from=prod-deps /app/node_modules /app/node_modules

# Install git
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

EXPOSE 3002

CMD ["node", "dist/src/index.js"]
