# Lesson 1: Introduction to Progressive Web Apps
This lesson provides an introduction to Progressive Web Apps and the browser APIs they use. 

**Objective:** Understand the fundamentals of PWAs, including their architecture, and begin building a basic PWA using service workers and the web manifest file.

## Part 1: Introduction to PWAs (30 minutes)
- **Topics Covered:**
  - What are PWAs and why are they important?
  - Core components of PWAs: Service Workers, Manifest, and HTTPS.
  - Comparison with traditional web apps and native apps.
- **Activity:** Discuss real-world examples of PWAs and their benefits.

### Lesson 1 Part 1: Introduction to PWAs

#### Why Progressive Web Apps Are Important

Progressive Web Apps (PWAs) bridge the gap between web experiences and native applications by combining the best of both worlds. They are important for several reasons:

1. **Accessibility and Reach:** PWAs can be accessed through a web browser, which makes them platform-independent and reachable by a broader audience without the need for downloading and installing from an app store.
2. **Performance:** PWAs are designed to be exceptionally fast and responsive, even on low-quality networks. They leverage modern web capabilities to deliver an app-like experience.
3. **Offline Capabilities:** With service workers, PWAs can work offline or on low-quality networks, improving accessibility and user experience.
4. **User Engagement:** PWAs can be installed on the user's home screen, send push notifications, and access device features, which increases user engagement.
5. **Cost-Effective:** Developing a PWA is often more cost-effective than developing separate native apps for different platforms (iOS, Android, etc.), as they are built using common web technologies.

#### Sample PWAs for Exploration
- **Twitter Lite:** A faster, data-friendly version of Twitter that provides a robust experience with instant loading and lower data consumption.
- **Starbucks Coffee:** Offers an ordering system similar to their native app, allowing users to browse the menu, customize orders, and add items to their cart, all usable offline.
- **Uber:** The PWA allows users to book rides even on low-speed networks, demonstrating how PWAs can perform critical tasks efficiently.
- **Forbes:** A great example of a PWA for content delivery, offering users a fast-loading experience with offline reading capabilities.
- **Pinterest:** Boasts an increased engagement rate through their PWA, which loads quickly and offers a smooth, grid-based layout akin to their app.

#### Testing PWA Features in the Browser
To test the features of a PWA directly in the browser use the following tools and methods:

1. **Chrome DevTools:** 
   - Open any PWA in Google Chrome.
   - Right-click and select "Inspect" to open DevTools.
   - Go to the "Application" tab to explore and test Service Workers, Manifest, and local storage functionalities.

2. **Lighthouse:**
   - [Lighthouse](https://chromewebstore.google.com/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk) is an extension for Chrome. You will need to install it first. 
   - Still within Chrome DevTools, click on the "Lighthouse" tab. 
   - Run a performance audit that includes checks for PWA criteria like performance, accessibility, best practices, and SEO.

3. **Offline Testing:**
   - Navigate to the "Application" tab in Chrome DevTools. "Choose Service Workers"
   - Check the "Offline" box to simulate network disconnection and observe how the PWA behaves.

### "Add to Home Screen" for Progressive Web Apps

The "Add to Home Screen" (A2HS) feature is a significant aspect of Progressive Web Apps (PWAs) that elevates the user experience to be more akin to that of native apps. This feature allows users to install a web app on their device's home screen, making it easily accessible without the need to open a browser and navigate to the website. Here's a breakdown of the importance and mechanics behind A2HS:

#### Importance of "Add to Home Screen"
- **User Engagement:** Apps on the home screen are more likely to be used regularly. A2HS makes web apps more visible and accessible, thereby increasing user engagement.
- **App-Like Experience:** By opening in a standalone window without the browser UI, the web app provides a user experience that closely mirrors that of a native app.
- **Performance Benefits:** Once added to the home screen, PWAs can leverage the full potential of service worker caching, making them extremely fast and reliable, even on flaky networks.

### Testing "Add to Home Screen"
To test the A2HS feature, you can use Chrome's DevTools to simulate the installation prompt and verify that the manifest is correct and the service worker operates as expected. Engaging with these settings effectively allows you to ensure your PWA is optimized for a seamless user experience.

#### Technical Requirements for "Add to Home Screen"
To enable A2HS, a PWA must meet certain criteria:
1. **Web Manifest File:** The PWA must have a manifest file with essential properties defined, such as `name`, `short_name`, `icons`, `start_url`, and `display`.
2. **Service Worker:** A service worker with a fetch event handler is required for offline support, which is a critical component for most A2HS setups.
3. **HTTPS:** The PWA must be served over a secure HTTPS connection to ensure that all transmitted data is encrypted.
4. **Engagement Metric:** The browser typically requires that the user has interacted with the domain for a minimum amount of time before prompting for A2HS.

### Implementing "Add to Home Screen"

#### Step 1: Define the Web Manifest
The web manifest is a JSON file that provides information about the app to the browser. It includes metadata needed for the add to home screen configuration.

```json
{
  "name": "Example PWA",
  "short_name": "Example",
  "icons": [
    {
      "src": "icon/lowres.webp",
      "sizes": "48x48",
      "type": "image/webp"
    },
    {
      "src": "icon/hd_hi.ico",
      "sizes": "72x72 96x96 128x128 256x256"
    },
    {
      "src": "icon/hd_hi.svg",
      "sizes": "any"
    }
  ],
  "start_url": "/index.html?source=pwa",
  "background_color": "#FFFFFF",
  "display": "standalone",
  "scope": "/",
  "theme_color": "#FFFFFF"
}
```

Read more about manifest.json here: https://developer.mozilla.org/en-US/docs/Web/Manifest

#### Step 2: Link the Manifest to Your HTML
Include the manifest in the `<head>` section of your HTML.

```html
<link rel="manifest" href="/manifest.json">
```

#### Step 3: Implement and Register a Service Worker
First, you need to register the service worker from your main JavaScript file. This is usually done when the page is loaded:

```javascript
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js')
    .then(function(registration) {
      // Registration was successful
      console.log('Service Worker registration successful with scope:', registration.scope);
    })
    .catch(function(err) {
      // Registration failed
      console.log('Service Worker registration failed:', err);
    });
}
```

### Creating a Basic Service Worker File
Next, you'll create a file named `service-worker.js` which will contain the logic for handling fetch events and caching. Here's a simple example:

```javascript
// Define a cache name
const CACHE_NAME = 'v1_cache';
// Define assets to cache
const urlsToCache = [
  '/',
  '/styles/main.css',
  '/script/main.js'
];

// Install a service worker
self.addEventListener('install', event => {
  event.waitUntil(
    caches.open(CACHE_NAME)
      .then(function(cache) {
        console.log('Opened cache');
        return cache.addAll(urlsToCache);
      })
  );
});

// Cache and return requests
self.addEventListener('fetch', event => {
  event.respondWith(
    caches.match(event.request)
      .then(function(response) {
        // Cache hit - return response
        if (response) {
          return response;
        }
        return fetch(event.request);
      }
    )
  );
});

// Update a service worker
self.addEventListener('activate', event => {
  const cacheWhitelist = [CACHE_NAME];
  event.waitUntil(
    caches.keys().then(cacheNames => {
      return Promise.all(
        cacheNames.map(cacheName => {
          if (cacheWhitelist.indexOf(cacheName) === -1) {
            return caches.delete(cacheName);
          }
        })
      );
    })
  );
});
```

### Explanation of the Service Worker Code
1. **Installation**: The service worker is installed, and during the installation phase, it opens a cache and adds specified assets to the cache. This includes the main page, CSS, and JavaScript files. This is done within the `install` event listener using `event.waitUntil()` to ensure that the service worker does not install until the files are successfully cached.

Read about `event.waituntil`: https://developer.mozilla.org/en-US/docs/Web/API/ExtendableEvent/waitUntil

2. **Fetching**: When the app requests resources (e.g., when loading a page), the service worker checks if those resources are in the cache. If they are, it serves them from the cache, providing fast load times and offline capabilities. If they're not in the cache, it fetches them from the network as usual.

Read about `caches`: https://developer.mozilla.org/en-US/docs/Web/API/caches

3. **Activation**: This phase is used to update the service worker. If there's a newer version of the service worker (or if the cache name changes), the old cache is cleared out. This is typically where you'd manage old caches and ensure that your app is using the latest assets.

This simple example covers the fundamental aspects of service workers, including registration, caching strategies, and basic lifecycle management, which are essential for building effective PWAs.

Read more about Service workers: https://developer.mozilla.org/en-US/docs/Web/Progressive_web_apps/Tutorials/js13kGames/Offline_Service_workers

