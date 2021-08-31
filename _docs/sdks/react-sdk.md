---
title: Unlaunch React SDK - Official Guide
description: This guide provides complete information about the Unlaunch React SDK
---
# Unlaunch React SDK

This guide provides complete information about the Unlaunch React SDK, which is a [client-side SDK](client-vs-server-side-sdks) for accessing feature flags. This guide will help you understand how to use this SDK in your React applications and use feature flags to control feature visibility. The Unlaunch React SDK requires React 16.3.0 or later (It uses the Context API, which requires at least 16.3.0)

Please see [Getting Started Guide](../getting-started/index) if you are new to Unlaunch and haven't created an account yet.

The React SDK is built on top of the [Unlaunch JavaScript library](javascript-library) and provides React API on top of the JavaScript library. Please read the [JavaScript library guide](javascript-library) to understand how it works.

## Import the SDK Library

The first step is to import the Unlaunch SDK as NPM dependency in your application. 

```xml
npm install --save unlaunch-react-sdk
```
## Initializing Unlaunch Client

After you install the dependency, initialize the React SDK. For this, you need your project's [client-side SDK key](sdk-keys), which is available in the Settings page on the Unlaunch Dashboard. Client-side SDK keys are **public** so it can be exposed to your users with no risk.

React SDK can be initialized the following ways.
 1. asyncWithUnlaunchProvider
 2. withUnlaunchProvider

These two methods rely on React's Context API so it would be easy to access flags from any level of component without passing flags as props. Both functions accept a ProviderConfig object used to configure the React SDK.

#### 1. asyncWithUnlaunchProvider

The asyncWithUnlaunchProvider (HoC) is an async function which is used to initialize React SDK and return a component in response. This function will initialize React SDK in the start of React lifecycle. When initialization is completed, it will save flag results and Unlaunch client object in `Context API`. This secures the results and make it available to any level of component tree.  

Due to the asynchronous nature of function, the rendering of your React app is delayed until initialization is completed. This process will take up some time initially. If you prefer to render your app first and process flag updates after rendering then `withUnlaunchProvider` should be used instead of asyncWithUnlaunchProvider.

```javascript
import { asyncWithUnlaunchProvider } from 'unlaunch-react-sdk';

(async () => {
  const unLaunchProvider = await asyncWithUnlaunchProvider({
    flag : ['flag-1','flag-1'] // Flag key set
    apiKey : '<PROVIDE_BROWSER_PUBLIC_KEY_FOR_YOUR_PROJECT>'
    identity : 'anonymous' // Use special anonymous identity which generates a unique UUID
    options = {
       offline: false,         
       requestTimeoutInMillis: 1000,
       logLevel: 'debug'  
    });

    render(
      <UnlaunchProvider>
        <YourApp />
      </UnlaunchProvider>,
      document.getElementById('reactDiv'),
    );
})();

```

#### 2. withUnlaunchProvider

The withUnlaunchProvider (HoC) function initializes the React SDK and wraps your root component in a Context.Provider. Flags will be initially undefined and in `componentDidMount` lifecycle it initialize React SDK and saves the Unlaunch client and flags results in `Context API`. As a result application will re render initially because of state change when component has been mounted.

```javascript
import { withUnlaunchProvider } from 'unlaunch-react-sdk';

export default withUnlaunchProvider({
  flag : ['flag-1','flag-1'] // Flag key set
  apiKey : '<PROVIDE_BROWSER_PUBLIC_KEY_FOR_YOUR_PROJECT>'
  identity : 'anonymous' // Use special anonymous identity which generates a unique UUID
  options: { /* ... */ }
})(YourApp);

```
#### 3. withUnlaunchConsumer

The return value of withUnlaunchConsumer is a wrapper function that takes your component and returns a React component injected with flags & unlaunchClient as props.

```js
import { withUnlaunchConsumer } from 'unlaunch-react-sdk';

const Home = ({ flags, unlaunchClient /*, ...otherProps */ }) => {
  // You can call any of the methods from the Unlaunch JavaScript SDK

  return flags.testFlag ? <div>Flag on</div> : <div>Flag off</div>;
};

export default withUnlaunchConsumer()(Home);

```
