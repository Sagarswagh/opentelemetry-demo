# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

FROM node:20 AS builder

WORKDIR /app

# Dummy ARG to prevent cache
ARG CACHE_BUSTER=1
RUN echo $CACHE_BUSTER

COPY ./src/flagd-ui/package*.json ./

RUN npm ci

COPY ./src/flagd-ui/. ./

RUN npm run build

# -----------------------------------------------------------------------------

FROM node:20-alpine

WORKDIR /app

# Dummy ARG to prevent cache
ARG CACHE_BUSTER=1
RUN echo $CACHE_BUSTER

COPY ./src/flagd-ui/package*.json ./

RUN npm ci --only=production

COPY --from=builder /app/src/instrumentation.ts ./instrumentation.ts
COPY --from=builder /app/next.config.mjs ./next.config.mjs

COPY --from=builder /app/.next ./.next

EXPOSE 4000

CMD ["npm", "start"]
