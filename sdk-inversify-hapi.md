### Installing SDK in **Node**  Web Application using Hapi framework and InversifyJS.

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
To use shortloop sdk, create a plugin inside ```/plugins/shortloop/index.ts``` the same way you do it for other plugins in your project. Refer below example. 
```js
import { HapiPlugin } from "../interfaces";
import * as Hapi from "@hapi/hapi";
import { ShortloopPlugin } from "@shortloop/node";

export default (): HapiPlugin => {
    return {
        register: async (server: Hapi.Server) => {
            await server.register([
                {
                    plugin: ShortloopPlugin,
                    options: {
                        url: "https://shortloop.company-name.com", // ShortLoop URL. (Provided by ShortLoop team.)
                        applicationName: "service-name", // your application name here
                        authKey: "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx", // ShortLoop Auth Key. (Provided by ShortLoop team.)
                        environment: "your-environment", // for e.g stage or prod
                    },
                }
            ]);
        },
        info: ShortloopPlugin.info,
    };
};
```
**4. Now just make sure you are registering the plugin in your project in your index.ts file**  
Your project will have some code similar to the below snippet which registers all plugins for the server object
```js
...
const pluginsPath = __dirname + '/libs/plugins/';
const plugins = fs.readdirSync(pluginsPath).filter(file => fs.statSync(path.join(pluginsPath, file)).isDirectory());
...
for (let pluginName of plugins) {
    const plugin: HapiPlugin = (require(pluginsPath + pluginName)).default();
    await plugin.register(this.server);
}
...
```

*Quickly test if project is building after configuring the Plugin :  (Maybe custom to your project)

```bash
npm build
```

After the changes, redeploy your Node Application.
