
**The Birth**

Do you know that the term Micro-Frontends first came up in   
_**ThoughtWorks Technology Radar**_ in 2016 and now it's being used by the rest of the frontend world?

After the discovery of SPA’s, this monolith single page frontend application became default for the many projects. But over time, the frontend layer, often developed by a separate team, grows and gets more difficult to maintain.   
  
But what if, we can logically break that gigantic application into small, manageable pieces and be able to develop it independently by separate teams. That’s it !! That’s the micro-frontends !! After a lot of success with microservices architecture, people extended this concept to the frontend world as well and called it ‘micro-frontends’…

---

**The Rise**

Zack Jackson, a Webpack maintainer, invented a masterpiece called Module Federation, which became a flagship feature of Webpack 5 in 2020. The Module Federation Plugin changed the world of Micro-Frontends to a totally new level. From 2021, Thoughtworks started to recommend Module Federation for Micro-Frontends implementations.

  
(Module Federation: It enables loading separately compiled applications at runtime and allows sharing of common dependencies.)

---

**The Benefits**

- Speed of Development
- Scalability of product
- Technology Independence
- Autonomous teams

---

**The Implementation**  
 

Yes, lets cut to the chase.   
How this exactly works? Or How can you implement it?  
  
So, the microfrontend project consists of different MFE’s (microfrontends) and a container/host to load all these different MFE’s. 

The main key in the implementation is the integration between the container/host and these different remote Micro-Frontends. There are two ways to integrate Micro-Frontends:

**Build-Time integration**

This is what most of the code implemented today. The container will install the components as libraries, similar to the libraries you install from npm. The issues with this approach are syncing different versions of libraries and build issues. Also, it is tough to use different technologies. Also, the size of the final package will be big because it contains all dependencies. Moreover, you have to deploy again for any changes in dependencies. Also, there is a tight coupling between the container and all Micro-Frontends.

  
**Run-Time integration**

Run-Time integration has three types of Compositions:

1. _Server-Side Composition:_ In this case, all the functionality will be in the backend that decides which Micro-Frontend to load. The server will decide which URL to route the request to. This is a standard server-side composition.
2. _Edge-Side Composition:_ In this approach, you make use of CDN (Ex: AWS CloudFront) and Lambda@Edge. The orchestration will happen on the CDN itself, instead of the Client-Side or Server-Side. 
3. _Client-Side Composition:_ In this case, the container is built and deployed separately. Each Micro-Frontend can be exposed as a separate package that the container/host can fetch the needed Micro-Frontend. The container and the Micro-Frontends are completely decoupled. They do not share build or deployment and can use different technologies. The container can decide on the version of the Micro-Frontend to deploy.

Ugh, now which type of integration to use: build-time or run-time?

With build time, we can publish the micro frontend independently but can not release the app without being dependent on the other micro frontends. To overcome this problem, it is advisable to compose micro frontends at run time.

**Run-time Integration using Modular Federation Plugin**

We can go step-by-step:

- First, decide the host/container app and list of remote MFEs
- In the remote MFEs, decide which modules(files) you want to expose(make available) to other projects
- Setup module federation in that remote MFE’s _webpack.config.js_ file.
- In the container/host, decide which files you want to get from remote MFEs
- Setup module federation in the container’s _webpack.config.js_ to fetch those files
- In the container/host, refactor the entry point so that fetch all files asynchronously

Let's understand with example. Consider, the Products is a Microfrontend running independently on 8081 port and is consumed by a container running on 8080 port.  Then their webpack configuration will look something like below.

Example of _**webpack.config.js**_ in remote _**MFE**_:

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
 mode: 'development',
 devServer: {
   port: 8081,
 },
 plugins: [
   new ModuleFederationPlugin({
     name: 'products',
     filename: 'remoteEntry.js',
     exposes: {
       './ProductsIndex': './src/index',
     },
   }),
   new HtmlWebpackPlugin({
     template: './public/index.html',
   }),
 ],
};
```

  
Example of _**webpack.config.js**_ in _**container**_:

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ModuleFederationPlugin = require('webpack/lib/container/ModuleFederationPlugin');

module.exports = {
 mode: 'development',
 devServer: {
   port: 8080,
 },
 plugins: [
   new ModuleFederationPlugin({
     name: 'container',
     remotes: {
       products: 'products@http://localhost:8081/remoteEntry.js',
     },
   }),
   new HtmlWebpackPlugin({
     template: './public/index.html',
   }),
 ],
};
```


For more resources on Micro-Frontends to go in-depth:

[https://martinfowler.com/articles/micro-frontends.html](https://martinfowler.com/articles/micro-frontends.html)

[https://module-federation.github.io/blog/get-started](https://module-federation.github.io/blog/get-started)

[https://micro-frontends.org](https://micro-frontends.org/) 

[https://www.thoughtworks.com/radar/techniques/micro-frontends](https://www.thoughtworks.com/radar/techniques/micro-frontends)