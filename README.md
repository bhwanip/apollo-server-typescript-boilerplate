## About

Merging Logic: https://github.com/kylealwyn/apollo-typescript-starter
and https://www.apollographql.com/blog/modularizing-your-graphql-schema-code-d7f71d5ed5f2/

Schema Spliting: https://github.com/bhirmbani/graphql-apollo-server-example

Mocha and Chai: 
https://dev.to/daniel_werner/testing-typescript-with-mocha-and-chai-5cl8

https://semaphoreci.com/community/tutorials/getting-started-with-node-js-and-mocha

Custom Directives:
https://github.com/alexanderVu/apollo-server-constraint-directive

https://github.com/profusion/apollo-validation-directives/blob/master/lib/auth.ts

https://github.com/profusion/apollo-validation-directives/blob/master/lib/hasPermissions.ts

A Apollo Server boilerplate with TypeScript support. Including development setup to ensure clean and working code before pushing.

## Included

The following development dependencies are included:

- [typescript](https://github.com/Microsoft/TypeScript)
- [graphql-codegen](https://github.com/dotansimha/graphql-code-generator)
- [prettier](https://github.com/prettier/prettier)
- [commitlint](https://github.com/marionebl/commitlint)
- [tslint](https://github.com/palantir/tslint)
- [husky](https://github.com/typicode/husky)
- [editorconfig](https://editorconfig.org/)

## Getting started

First clone the repository and install dependencies.

```bash
$ git clone https://github.com/markuswind/apollo-server-boilerplate
$ npm ci
$ npm run start
```

## Adding new (type safe) resolvers

Because we want to have type safe resolvers, it's the easiest to create new resolvers in order described below.

#### Type definitions

When adding new resolvers you should start with adding the `typeDefs` in your (new) `resolver.ts` file like following:

```ts
export const typeDefs = grapql`
  type Example {
    id: Int!
  }

  type Query {
    example(id: Int!): Example!
  }
`;
```

#### Generating types

The next step is updating the generated types like following:

```bash
$ npm run generate:types
```

#### Provider

After this you could create your update your (new) `provider.ts` file like following:

```ts
import { QueryExampleArgs } from './generated/graphql';

export class ExampleAPI extends RestDataSource {
  // ... constructor

  public async getExample(args: QueryExampleArgs) {
    // ... use typed args to return result
  }
}
```

**NOTE:** When adding a new provider you'll have to update the **context** in the `index.ts` file.

#### Resolvers

Finally you could create your typed resolvers in the `resolvers.ts` file like following:

```ts
import { IResolvers } from './generated/grapql';

export const resolvers: IResolvers = {
  Query: {
    example: (_, args, ctx) => ctx.dataSources.ExampleAPI.getExample(args)
  }
};
```
