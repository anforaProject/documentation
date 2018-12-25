# Development

## Client

The client uses _Vue.js_ as framework and _yarn_ as package manager. To develop it follow this steps:

1. Clone the repo

        https://github.com/anforaProject/client.git

2. Install dependencies

        yarn install

3. You'll need a connection to an existing server in order to interact with the app
The base url of the server is defined in the `.env` file under the name `VUE_APP_CLIENT_DOMAIN`.
By default it points to `https://anfora.social`. While this instance is up you can use the API of this server.

4. To run the development server you can run

        yarn serve

## Server 

The process of installation for both the development and production environement 
is the same. Follow the instuctions in the [installation page](/getting-started)