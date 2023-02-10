### Installing SDK in **Node js**  Web Application.

**1. `@shortloop/node` can be installed like any other npm package through `npm`:**

```bash
npm install @shortloop/node@0.0.2
```

**2. Once the package is installed, you can import the library using ES5 or ES6 syntax**

```js
const { ShortloopSDK } = require('@shortloop/node')
```
<p align="center">
OR
</p>

```js
import { ShortloopSDK } from '@shortloop/node'
```

**3. Initialize the shortloop sdk**  
To use shortloop/node sdk, you’ll need to initialize it with options - url and applicationName as shown in below example.
```js
const app = express();
const sdk = ShortloopSDK.init({
    url: 'https://shortloop.company-name.com', // the deployed shortloop url here.
    applicationName: 'service-name', // your application name here.
});
app.use(express.json()); 
app.use(express.text()); //...any other middlewares for body parsing
app.use(sdk.capture());
```

*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
npm build
```

After the changes, redeploy your Node Application.
