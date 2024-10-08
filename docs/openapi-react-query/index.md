---
title: openapi-react-query
---
# Introduction

openapi-react-query is a type-safe tiny wrapper (1 kb) around [@tanstack/react-query](https://tanstack.com/query/latest/docs/framework/react/overview) to work with OpenAPI schema.

It works by using [openapi-fetch](../openapi-fetch/) and [openapi-typescript](../introduction) so you get all the following features:

- ✅ No typos in URLs or params.
- ✅ All parameters, request bodies, and responses are type-checked and 100% match your schema
- ✅ No manual typing of your API
- ✅ Eliminates `any` types that hide bugs
- ✅ Also eliminates `as` type overrides that can also hide bugs

::: code-group

```tsx [src/my-component.ts]
import createFetchClient from "openapi-fetch";
import createClient from "openapi-react-query";
import type { paths } from "./my-openapi-3-schema"; // generated by openapi-typescript

const fetchClient = createFetchClient<paths>({
  baseUrl: "https://myapi.dev/v1/",
});
const $api = createClient(fetchClient);

const MyComponent = () => {
  const { data, error, isLoading } = $api.useQuery(
    "get",
    "/blogposts/{post_id}",
    {
      params: {
        path: { post_id: 5 },
      },
    },
  );

  if (isLoading || !data) return "Loading...";

  if (error) return `An error occured: ${error.message}`;

  return <div>{data.title}</div>;
};
```

:::

## Setup

Install this library along with [openapi-fetch](../openapi-fetch/) and [openapi-typescript](../introduction):

```bash
npm i openapi-react-query openapi-fetch
npm i -D openapi-typescript typescript
```

::: tip Highly recommended

Enable [noUncheckedIndexedAccess](https://www.typescriptlang.org/tsconfig#noUncheckedIndexedAccess) in your `tsconfig.json` ([docs](/advanced#enable-nouncheckedindexedaccess-in-tsconfig))

:::

Next, generate TypeScript types from your OpenAPI schema using openapi-typescript:

```bash
npx openapi-typescript ./path/to/api/v1.yaml -o ./src/lib/api/v1.d.ts
```

## Basic usage

Once your types has been generated from your schema, you can create a [fetch client](../introduction.md), a react-query client and start querying your API.

::: code-group

```tsx [src/my-component.ts]
import createFetchClient from "openapi-fetch";
import createClient from "openapi-react-query";
import type { paths } from "./my-openapi-3-schema"; // generated by openapi-typescript

const fetchClient = createFetchClient<paths>({
  baseUrl: "https://myapi.dev/v1/",
});
const $api = createClient(fetchClient);

const MyComponent = () => {
  const { data, error, isLoading } = $api.useQuery(
    "get",
    "/blogposts/{post_id}",
    {
      params: {
        path: { post_id: 5 },
      },
    },
  );

  if (isLoading || !data) return "Loading...";

  if (error) return `An error occured: ${error.message}`;

  return <div>{data.title}</div>;
};
```

:::

::: tip
You can find more information about `createFetchClient` on the [openapi-fetch documentation](../openapi-fetch/index.md).
:::

