
Here is the folder structure for `backend`, it is using `yarn workspaces` which helps us split our  monorepo into packages such as DB, GraphQL. Which if required can be made into their own micro services.

``` 
backend
├── build
├── config
├── logs
├── packages
│   ├── db
│   │   └──prisma
│   ├── graphql
│   │   ├── api
│   │   ├── schema
│   │   └── types
│   └── utils
├── tests
│   ├── db
│   └── graphql
├── index.ts
└── package.json
```

##### DB

This workspace package contains the database abstractions. The database stack is [PostgreSQL](https://www.postgresql.org/) as relational database and [Prisma](https://www.prisma.io/) as an ORM, read more about DB package [here](./backend/packages/db/README.md)

##### GraphQL

The GraphQL package is organized as below:
``` 
graphql
├── schema
│   └── user                <---- some entity
│       ├── resolvers 
│       │     ├── types     <---- type resolvers
│       │     ├── queries   <---- query resolvers
│       │     └── mutations <---- mutation resolvers
│       ├── queries.graphql
│       ├── mutations.graphql
│       └── types.graphql
├── api
│   ├── queries             
│   └── mutations
├── types                   <---- graphql types
│   ├── schema           
│   └── resolvers
└── index.json
```

The schema splits each entity into it's own set of schema to modularize the codebase. The graphql package uses [schema stitching](https://www.apollographql.com/docs/apollo-server/features/schema-stitching) and [code generators](https://graphql-code-generator.com/) to construct the whole schema.

It is organized so because if you choose to split graphql into it's own set of microservices later, it should be relatively easier to do so as this should be easy to integrate with [Apollo Federation](https://www.apollographql.com/docs/apollo-server/federation/introduction)

Read more about GraphQL package [here](./backend/packages/graphql/README.md)

#### <a id='web' style="color: black;">Web</a>
Here is the folder structure for `web`, it is a standard [create-react-app](https://create-react-app.dev/) using [craco](https://github.com/gsoft-inc/craco) to override configs without ejecting

Web package uses [Material UI](https://material-ui.com/) heavily as it makes theming and customization very easy. PR's for any other UI kit are welcome 😃

``` 
web
├── build
├── public
├── src
│   ├── assets
│   ├── config
│   ├── constants
│   ├── global
│   ├── tests
│   ├── layout     <---- controls, pure components
│   ├── theme      <---- theme config
│   ├── graphql
│   │   └── operations.tsx     <---- generated graphql operations and types
│   ├── pages
│   │   └──  Home   <---- page component
│   │        ├── components <---- page specific components
│   │        └── hooks      <---- page specific custom hooks   
│   └── utils
├── tests
│   ├── db
│   └── graphql
├── index.ts
└── package.json
```


### 🏃 <a id="getting-started" style="color: black;">Getting Started</a>

**Setting up environment variables**

Before getting started, create `.env` files at both `backend/.env` as well as `web/.env` following the `.env.template` files located in those directories. 

**Install dependencies**

I recommend using `yarn` instead of `npm` as this project heavily uses `yarn workspaces`

Install [volta](https://docs.volta.sh/guide/getting-started), which should automatically install correct `node` and `yarn` version when you checkout the repository (check the root package.json for config)

```
yarn
```

<i>To install dependencies for `web` and `backend` automatically, a postinstall script has been added in the main `package.json`</i>

**Running web**

- Docker (recommended)

```
$ cd web
$ yarn dev
```

Once you're done working, use `yarn dev:down` command to stop the docker containers.

- Locally

```
$ cd web
$ yarn start:web
```

**Running backend**

- Docker (recommended)

```
$ cd backend
$ yarn dev
```

Once the container starts, you'll be inside the backend image. Now, simply migrate the db (only first time) and start the development server.

```
$ yarn migrate
$ yarn start
```

Once you're done working, exit out from the container and use `yarn dev:down` command to stop the docker containers.

- Locally

```
$ cd backend
$ yarn start
```

_Note: When running locally, you'll be required to run your own instance of postgres._

**Running backend-go**

If you don't have [`make`](https://en.wikipedia.org/wiki/Make_(software)) installed, commands are available in `Makefile`.

```
$ cd backend-go
$ make dev
```

Now from inside the container, you can run the tests or application like below:

```
$ make test
$ make run
```
