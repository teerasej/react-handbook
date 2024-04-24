
# Service Workers and Caching in React

Service workers are a powerful tool for creating offline-first web applications. They allow you to cache resources and intercept network requests, enabling you to serve assets from the cache when the network is unavailable. This can greatly improve the performance of your application and provide a better user experience.

## Step 

### 1. Setup 

Create a new React app using Create React App:

```bash
npx create-react-app service-worker-example
cd service-worker-example
```

### 2. install & setup service-worker plugin 

Workbox is a set of libraries that can simplify the process of working with service workers.

```bash
npm install --save-dev workbox-webpack-plugin
```

### 3. Configure Workbox in Webpack

Create the `config/webpack.config.js` file, require the workbox-webpack-plugin at the top and add the plugin to your plugins array.

```js
// config/webpack.config.js

const WorkboxWebpackPlugin = require('workbox-webpack-plugin');

module.exports = {
  //...
  plugins: [
    //...
    new WorkboxWebpackPlugin.GenerateSW({
      clientsClaim: true,
      skipWaiting: true,
    }),
  ],
};
```

### 4. Register the service worker

In your `src/index.js` file, register the service worker.

```js
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import reportWebVitals from './reportWebVitals';

if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js');
  });
}

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);

// If you want to start measuring performance in your app, pass a function
// to log results (for example: reportWebVitals(console.log))
// or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
reportWebVitals();

```

### 5. Cache and Retrieve Data

Create `public/service-worker.js` file, add event listeners for the install and fetch events to cache and retrieve data.

```js
// public/service-worker.js

self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open('my-cache').then((cache) => {
      return cache.addAll([
        '/',
        '/index.html',
          '/static/js/bundle.js',
          '/static/js/0.chunk.js',
          '/static/js/main.chunk.js',
          '/static/css/main.chunk.css',
          '/favicon.ico',
          '/logo192.png',
          '/logo512.png',
          '/manifest.json',
        // add more files to cache here
      ]);
    })
  );
});

self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

### 6. Test Office Functionality

Run the app in development mode:

```bash
npm start
```

#### Check that the service worker is registered and that the assets are being cached.

1. Open the app in your browser
2. Open Developer Tools (F12)
3. Navigate to the Application tab
4. From the left menu, open the Service Workers tab to check that the service worker is registered and running.
5. From the left menu, open the Cache Storage tab to check that the assets are being cached.

#### Simulate Offline Mode

1. Open the app in your browser
2. navigate to the Network tab in the developer tools. 
3. In the network condition below, select 'Network throttling' and choose 'Offline' from the dropdown.
4. Refresh the page and check that the assets are being served from the cache.
5. Notice that some assets may not be cached, such as API requests.

