# Dynamic Ingest dashboard

<p>This is a sample of using notifications to build a simple dashboard for the Brightcove Dynamic Ingest API. The handler parses notifications from the Dynamic Ingest API to identify processing complete notifications. It then adds the video id into an array in a JavaScript file. The dashboard itself is an HTML page that includes the array of processed video ids. It uses the ids to makes 2 requests to the CMS API to get the video metadata and the array of sources. Whether renditions exist or not shows whether the ingest succeeded or failed. You can view working sample of the dashboard <a href="//solutions.brightcove.com/bcls/ingest-dashboard/di-log.html">here.</a>.</p>

##Requirement##

All files in the **ingest-dashboard** folder must be deployed to one folder on a local or remote web server running PHP.

##Changes you must make to the app!##
<p>You must make the following changes to the app files before it will work for your account:</p>

- in `/ingest-dashboard/brightcove-learning-proxy.php`: insert the appropriate client_id and client_secret values in lines 24 and 25:

  ```
   $client_id     = "your_client_id_here";
   $client_secret = "your_client_secret_here";
  ```

- in `/ingest-dashboard/di-log.html`: insert your Video Cloud account id in line 63:

  ```
   var account_id = your_account_id_here,
  ```

- in your app to make Dynamic Ingest requests, include a `callbacks` array in the request body data, with the full URL to `callbacks-di.php` - for example:

  ```
        {"master":{"url":"videoURL"},"profile":"ingest-profile-name","callbacks": ["http://path_to_this_folder/ingest-dashboard/callbacks-di.php"]}
  ```

- if you use the `di-tester.html` to test this app, you will need to enter your Video Cloud account id and the path to `callbacks-di.php` in lines 135-6:

  ```
        account_id = your_account_id_here,
        callbackURL = '//path-to/callbacks-di.php',
  ```

- you must insure that the notification handler can write to `video-ids.js` on the server. For instance, you can SSH to the folder on the server and use `chmod 777 video-ids.js`

###Architecture###
<p>Here is the high-level architecture of the app: </p>

<p><img src="./assets/ingestion-dashboard-architecture.png"></p>

###The app parts###
<p>The handler for notifications is built in PHP - it looks for processing complete notifications and adds the video id to an array in a separate JavaScript file:</p>

####Notification handler####
<p><a href="https://github.com/BrightcoveLearning/dynamic-ingest-dashboard/blob/master/ingest-dashboard/callbacks-di.php">[View the code]</a></p>

####JavaScript file:####
<p>[View the code]<a href="https://github.com/BrightcoveLearning/dynamic-ingest-dashboard/blob/master/ingest-dashboard/video-ids.js">[View the code]</a></p>

####Dashboard####
<p>The dashboard include the JavaScript file, and uses additional JavaScript to fetch data from the CMS API and write the results into a table:</p>

<p><a href="https://github.com/BrightcoveLearning/dynamic-ingest-dashboard/blob/master/ingest-dashboard/di-log.html">[View the code]</a></p>

####Proxy####
<p>The proxy, also built in PHP, takes the CMS API requests from the dashboard, gets an access token, and makes the API request, returning the data to the dashboard. The proxy is needed because client-side calls to the CMS-API are not allowed, as this would require exposing your client credentials:</p>

<p><a href="https://github.com/BrightcoveLearning/dynamic-ingest-dashboard/blob/master/ingest-dashboard/brightcove-learning-proxy.php">[View the code]</a></p>

####Clear the log####
<p>This simple PHP app just restores the JavaScript file to its original state, clearing out the old video ids</p>

<p><a href="https://github.com/BrightcoveLearning/dynamic-ingest-dashboard/blob/master/ingest-dashboard/clear-log.php">[View the code]</a></p>
