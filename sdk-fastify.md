### Installing SDK in **Node**  Web Application using Fastify framework.

**1. `@shortloop/node` can be installed like any other npm package through `npm`:**    
TODO: update the version
```bash
npm install @shortloop/node@0.0.8-beta
```

**2. Once the package is installed, you can import the library following syntax**

For Vanilla JS projects
```js
const { fastifyShortloopPlugin } = require("@shortloop/node");
```

For Typescript projects
```js
import { fastifyShortloopPlugin } from '@shortloop/node';
```

**3. Initialize the shortloop sdk plugin**  
- To use shortloop plugin, you’ll need to register it with following options as shown in below example.  
- Note: Register it after you add any content parsers or any plugin which helps you parse the request body. 

For Vanilla JS projects
```js
...
const fastify = require("fastify")({ logger: true });
...

fastify.addContentTypeParser('text/html', htmlBodyParser);
fastify.register(multipart, { addToBody: true });
// any other parsers
...

fastify.register(fastifyShortloopPlugin, {
    url: "https://shortloop.company-name.com", // ShortLoop URL. (Provided by ShortLoop team.)
    applicationName: "service-name", // your application name here
    authKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", // ShortLoop Auth Key. (Provided by ShortLoop team.)
    environment: "your-environment", // for e.g stage or prod
});
```
For Typescript projects
```js
...
import fastify from 'fastify'
...

const server = fastify()
server.addContentTypeParser('text/html', htmlBodyParser);
server.register(multipart, { addToBody: true });
// any other parsers
...

server.register(fastifyShortloopPlugin, {
    url: "https://shortloop.company-name.com", // ShortLoop URL. (Provided by ShortLoop team.)
    applicationName: "service-name", // your application name here
    authKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", // ShortLoop Auth Key. (Provided by ShortLoop team.)
    environment: "your-environment", // for e.g stage or prod
});
```
*Quickly test if project is building after configuring the Plugin :  (Maybe custom to your project)

```bash
npm build
```

After the changes, redeploy your Node Application.
