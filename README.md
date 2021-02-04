# Basics Tutorial GraphQL

I saved the code from tutorial on: <https://www.howtographql.com/graphql-js/1-getting-started/>

So I can use this as a reference for further programming with GraphQL.

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

    query {
      user { id name }
    }

### With count and orderBy

    query {
      feed(orderBy: { createdAt: asc }) {
        count
        links {
          id
          description
          url
        }
      }
    }

With `curl`

    curl 'http://localhost:4000/' -H 'Content-Type: application/json' \
    --data-binary '{"query":"query{feed(orderBy:{createdAt:asc}){count links{id description url}}}"}'

### With filter and postedBy

    query {
      feed(filter: "an", take: 2, skip: 1) {
        links {
          id
          description
          url
          postedBy {
            id
            name
          }
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
      signup(name: "Alice", email: "andries@filmer.nl", password: "graphql") {
        token
        user {
          id
        }
      }
    }


## Authentication

When using the **signup mutation** or **login mutation** look at the response `bearer` token.
Copy this ____TOKEN____ to HTTP HEADERS in the browser when running the server <http://localhost:4000/>

    {
      "Authorization": "Bearer __TOKEN__"
    }

### Login

    mutation {
      login(email: "andries@filmer.nl", password: "graphql") {
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
