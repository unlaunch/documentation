---
title: Unlaunch React.js SDK - Official Guide
description: This guide provides complete information about the Unlaunch React SDK
---
# Unlaunch React.js SDK

This guide provides complete information about the Unlaunch React.js SDK and documents all of the methods available in our client-side React SDK. This guide help to understand how to use its functions and how to integrate it in your application to call feature flags.

{% include alert-note.html type="info" content="The Unlaunch React SDK uses Javascript client side SDK under the hood." %}

{% include alert-note.html type="warning" content="The Unlaunch React SDK requires React 16.3.0 or later. This SDK uses the Context API, which requires React version 16.3.0 or later." %}

## Import the SDK Library

The first step is to import the Unlaunch SDK as npm dependency in your application. 

For Npm, 
```xml
npm install --save unlaunch-react-sdk
```
## Initializing Unlaunch Client

After you install the dependency, initialize the React SDK. For this, you need your project's client-side ID, which is available in Unlaunch's Settings page. Client-side IDs are public so it can be exposed in your client-side code with no risk.

React SDK can be initializes in two ways:
 1. asyncWithUnlaunchProvider
 2. withUnlaunchProvider

These two methods rely on React's Context API so it would be easy to access flags from any level of component without passing flags as props. Both functions accept a ProviderConfig object used to configure the React SDK.

#### asyncWithUnlaunchProvider

The asyncWithUnlaunchProvider (HoC) is an async function which is used to initialize React SDK and return a component in response. This function will initialize React SDK in the start of React lifecycle. When initialization is completed, it will save flag results and Unlaunch client object in `Context api`. This secures the results and make it available to any level of component tree.  

Due to the asynchronous nature of function, the rendering of your React app is delayed until initialization is completed. This will take up some time initially which will be around `200 milliseconds`, but mostly completed earlier. If you prefer to render your app first and process flag updates after rendering then `withUnlaunchProvider` should be used instead of asyncWithUnlaunchProvider.

```javascript
import { asyncWithUnlaunchProvider } from 'unlaunch-react-sdk';

(async () => {
  const LDProvider = await asyncWithLDProvider({
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
#### withUnlaunchProvider

The withUnlaunchProvider (HoC) function initializes the React SDK and wraps your root component in a Context.Provider. Flags will be initially undefined and in `componentDidMount` lifecycle it initialize React SDK and saves the Unlaunch client and flags results in Context API. As a result application will re render initially because of state change when component has been mounted.

```javascript
import { withUnlaunchProvider } from 'unlaunch-react-sdk';

export default withLDProvider({
  flag : ['flag-1','flag-1'] // Flag key set
  apiKey : '<PROVIDE_BROWSER_PUBLIC_KEY_FOR_YOUR_PROJECT>'
  identity : 'anonymous' // Use special anonymous identity which generates a unique UUID
  options: { /* ... */ }
})(YourApp);

```
