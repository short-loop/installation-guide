---
redirect_to: "https://docs.shortloop.dev/nodejs/nestjs"
canonical_url: "https://docs.shortloop.dev/nodejs/nestjs"
---

### Installing SDK in **Nest js**  Web Application.

**1. `@shortloop/node` can be installed like any other npm package through `npm`:**

```bash
npm install @shortloop/node@0.0.7
```

**2. Once the package is installed, add the following piece of code on top of you root level app.module.ts file**
```js
import { MiddlewareConsumer } from '@nestjs/common';
import { ShortloopSDK } from '@shortloop/node';
```

**3. Initialize the shortloop sdk**  
To use shortloop/node sdk, you’ll need to initialize it with options - url and applicationName and use it as middleware inside your AppModule Class 
```js
configure(consumer: MiddlewareConsumer) {
  ShortloopSDK.init({
    url: 'https://shortloop.company-name.com', // ShortLoop URL. (Provided by ShortLoop team.)
    applicationName: 'service-name', // your application name here
    authKey: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', // ShortLoop Auth Key. (Provided by ShortLoop team.)
    environment: 'your-environment', // for e.g stage or prod
  });
  consumer.apply(ShortloopSDK.capture()).forRoutes('*');
}
```
*After adding the above, your app.module.ts file should look something like this :*

```js
... 
import { MiddlewareConsumer } from '@nestjs/common';
import { ShortloopSDK } from '@shortloop/node';
...
...

...
@Module({
  imports: [...],
  controllers: [AppController],
  providers: [AppService],
})
export class AppModule {
  configure(consumer: MiddlewareConsumer) {
    ShortloopSDK.init({
      url: 'https://shortloop.company-name.com', // ShortLoop URL. (Provided by ShortLoop team.)
      applicationName: 'service-name', // your application name here
      authKey: 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx', // ShortLoop Auth Key. (Provided by ShortLoop team.)
      environment: 'your-environment', // for e.g stage or prod
    });
    consumer.apply(ShortloopSDK.capture()).forRoutes('*');
  }
}

```
*Quickly test if project is building after configuring SDK :  (Maybe custom to your project)
```bash
npm build
```

After the changes, redeploy your Nest Application.
