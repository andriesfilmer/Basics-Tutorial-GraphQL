# Basics Tutorial GraphQL

Done the tutorial on: <https://www.howtographql.com/graphql-js/1-getting-started/>

I will use this as a reference for further programming with GraphQL.

## Install

    git clone https://github.com/andriesfilmer/hackernews-node.git
    cd hackernews-node
    npm install

Run node server

    node src/index.js

Run prisma

    npx prisma generate

Run prisma studio

    npx prisma studio

## query's

    query { info }

First do a signup mutation

    query {
      user { id name }
    }

### Filters

    query {
      feed(filter: "QL") {
        id
        description
        url
        postedBy {
          id
          name
        }
      }
    }

### Pagination

    query {
      feed(take: 1, skip: 1) {
        id
        description
        url
      }
    }

### Sorting

    query {
      feed(orderBy: { createdAt: asc }) {
        id
        description
        url
      }
    }

### Counting

    query {
      feed {
        count
        links {
          id
          description
          url
        }
      }
    }

## Mutations

    mutation {
      post(url: "www.prisma.io", description: "Prisma replaces traditional ORMs") {
        id
      }
    }

    mutation {
      signup(name: "Alice", email: "alice@prisma.io", password: "graphql") {
        token
        user {
          id
        }
      }
    }


## Authentication

When using the **signup mutation** look at the response `bearer` token.
Copy this __TOKEN__ to HTTP HEADERS in the browser when running the server <http://localhost:4000/>

    {
      "Authorization": "Bearer __TOKEN__"
    }

### Login

    mutation {
      login(email: "alice@prisma.io", password: "graphql") {
        token
        user {
          email
          links {
            url
            description
          }
        }
      }
    }


## Subscriptions

### Link add subscription

Open a browser on `localhost:4000` and create a subscription.

    subscription {
      newLink {
        id
        url
        description
        postedBy {
          id
          name
          email
        }
      }
    }

Open a other browser and make a mutaion, while watch the subscription in the other browser.

    mutation {
      post(url: "www.graphqlweekly.com", description: "Curated GraphQL content coming to your email inbox every Friday") {
        id
      }
    }

### New link vote subscription

    subscription {
      newVote {
        id
        link {
          url
          description
        }
        user {
          name
          email
        }
      }
    }

    mutation {
      vote(linkId: "__LINK_ID__") {
        link {
          url
          description
        }
        user {
          name
          email
        }
      }
    }
