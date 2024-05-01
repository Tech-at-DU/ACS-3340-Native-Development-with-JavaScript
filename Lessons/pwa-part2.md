# PWAs and Service Workers

The premise of service workers is to provide control for asset caching to solve problems where connections may be lost or unavailable. A service worker can provide chached data when there is no connection. 

A service worker acts as a proxy server allowing you to replace responses with cahced data. 

Service worker code must be served from an HTTPS connection. This is a security requiement.

## Using Service workers

1. Register your service worker with something like: `navigator.serviceWorker.register('service-worker.js')` - https://developer.mozilla.org/en-US/docs/Web/API/ServiceWorkerContainer/register
2. An install event is first sent to a service worker. Use this to populate an indexDB and cache assets. During this step your app is proparing to make data available offline. 
3. After install completes the service worker is considered "installed". A previous service worker may be controlling pages at this stime. The new one will not activate until all pages are closed. 
4. Once all pages are closed the new service worker will receive an activate event. The primary use of activate is to clean up resources used by a previous service worker. 

Events used by service workers: 
- install 
- activate 
- message 

Functional Events 
- fetch
- sync 
- push 



