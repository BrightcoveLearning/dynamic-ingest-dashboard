<h2>Dynamic Ingest dashboard</h2>

<p>This section is a sample of using notifications to build a simple dashboard for the Brightcove Dynamic Ingest API. The handler parses notifications from the Dynamic Ingest API to identify processing complete notifications. It then adds the video id into an array in a JavaScript file. The dashboard itself is an HTML page that includes the array of processed video ids. It uses the ids to makes 2 requests to the [CMS API](//docs.brightcove.com/en/video-cloud/cms-api/getting-started/quick-start-cms.html) to get the video metadata and the array of sources (renditions). Whether renditions exist or not shows whether the ingest succeeded or failed. You can view working sample of the dashboard [here](//solutions.brightcove.com/bcls/di-api/di-log.html).</p>

<p>     Here is the high-level architecture of the app: </p>

<p>![ingest-dashboard-architecture](./assets/ingestion-dashboard-architecture.png)</p>

<h3>The app parts</h3>

<p>The handler for notifications is built in PHP - it looks for processing complete notifications and adds the video id to an array in a separate JavaScript file:</p>

<h4>Notification handler</h4>

<p><script src="https://gist.github.com/846bf4f1b8d52b442121.js"></script></p>

<h4>JavaScript file:</h4>

<p><script src="https://gist.github.com/c4b36c1f826762be75f9.js"></script></p>

<h4>Dashboard</h4>

<p>The dashboard include the JavaScript file, and uses additional JavaScript to fetch data from the CMS API and write the results into a table:</p>

<p><script src="https://gist.github.com/5f93d0f1fae7c4666867.js"></script></p>

<h4>Proxy</h4>

<p>The proxy, also built in PHP, takes the CMS API requests from the dashboard, gets an access token, and makes the API request, returning the data to the dashboard. The proxy is needed because client-side calls to the CMS-API are not allowed, as this would require exposing your client credentials:</p>

<p><script src="https://gist.github.com/b0d5f05a328840851ce5.js"></script></p>

<h4>Clear the log</h4>

<p>This simple PHP app just restores the JavaScript file to its original state, clearing out the old video ids:</p>

<p><script src="https://gist.github.com/d013a4d44236054a2201.js"></script></p
