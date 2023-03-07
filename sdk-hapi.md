### Installing SDK in **Node**  Web Application using Hapi framework.

**1. `@shortloop/node` can be installed like any other npm package through `npm`:**
TODO: Update to latest release version
```bash
npm install @shortloop/node@0.0.7
```

**2. Once the package is installed, you can import the library following syntax**

For Vanilla JS projects
```js
const { ShortloopPlugin } = require("@shortloop/node");
```

For Typescript projects
```js
import { ShortloopPlugin } from "@shortloop/node";
```

**3. Initialize the shortloop sdk plugin**  
To use shortloop plugin, you’ll need to register it with following options as shown in below example. Register it after you initialize the server object - the same way you register other plugins in your project 
```js
...
const server = Hapi.server({
    port: 3000,
    host: "localhost",
});
...
await server.register({
    plugin: ShortloopPlugin,
    options: {
        url: "https://shortloop.company-name.com", // ShortLoop URL. (Provided by ShortLoop team.)
        applicationName: "service-name", // your application name here
        authKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", // ShortLoop Auth Key. (Provided by ShortLoop team.)
        environment: "your-environment", // for e.g stage or prod
    },
});
```

*Quickly test if project is building after configuring the Plugin :  (Maybe custom to your project)

```bash
npm build
```

After the changes, redeploy your Node Application.
