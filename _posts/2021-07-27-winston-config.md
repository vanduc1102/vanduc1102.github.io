---
layout: post
title: Winston3 configuration with namespace
date: 2021-07-27 00:14:27 +0700
categories: [nodejs]
tags: [nodejs, typescript]
---

## Configuration

Here is my Winston@3 configuration that I am using in my current project

```ts
import winston from "winston";

const { format } = winston;

const { LOG_LEVEL, APP_ENV } = process.env;

const isDevelopment = APP_ENV == "local";
const isTesting = APP_ENV == "test";

const logFormatter = isDevelopment
	? format.combine(
			winston.format.timestamp(),
			winston.format.colorize(),
			winston.format.printf((info: any) => {
				const { level, message, timestamp, namespace, stack, ...restMeta } =
					info;
				const displayNamespace = namespace ? `[${namespace}] -` : "";
				const stackMessage = stack ? `\n${stack}` : "";
				const otherMetaMessage =
					Object.keys(restMeta).length > 0
						? `\n${JSON.stringify(restMeta)}`
						: "";
				return `${timestamp} ${displayNamespace} ${level}: ${message} ${otherMetaMessage}${stackMessage}`;
			})
	  )
	: format.combine(winston.format.timestamp(), winston.format.json());

const logger = winston.createLogger({
	level: LOG_LEVEL || "debug",
	format: logFormatter,
	transports: [
		new winston.transports.Console({
			silent: isTesting,
		}),
	],
});

export const Logger = (namespace?: string) => {
	if (namespace) {
		return logger.child({ namespace });
	}
	return logger;
};
```

This setup will produce log messages in `text format` for local development and `json format` for production setup with exception trace.

## How to use

in `server.ts`

```ts
import Koa from "koa";

import { createApp } from "./app";
import { Logger } from "./utils/loggers";
const port = parseInt(process.env.PORT as string, 10) || 3000;

const LOGGER = Logger("server");

const server = (app: Koa) => {
	return app.listen(port, () => {
		LOGGER.info(`> Ready on http://localhost:${port}`);
	});
};

server(createApp());
```

in `product.service.ts`

```ts
const LOGGER = Logger("productService");
export default class ProductService {
	doThis() {
		LOGGER.info("do this");
	}

	toThat() {
		LOGGER.info("do that");
	}
}
```
