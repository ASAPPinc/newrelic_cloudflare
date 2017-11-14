# NOTE: This fork is intended to make 

# Description

Cloudflare Plugin for Newrelic
Fetches metrics from Cloudflare and aggregates the data, providing one dashboard that covers multiple Cloudflare sites.
The code currently only supports Cloudflare Enterprise plans, as that is the only plan level that offers per-minute reporting.
Can also collect performance data from local RailGun servers.

- Total Bandwidth Mbps  (Total, Cached, Uncached, Railgun)
- Total Requests/s (Total, Cached, Uncached)
- Bandwidth and Requests by Site
- Cache Ratio
- Origin bandwidth and requests
- Pageviews and Googlebot request
- Threats
- Origin vs Railgun Bandwidth
- Railgun Compression
- Railgun WAN Mpbs

----

# Requirements

- Node.JS/NPM
- Cloudflare Enterprise account

----

# Deploying to Lambda

- Clone this repository.
- Inside this directory, run `npm install`
- After the dependencies have been successfully installed, run `zip -r ../newrelic_cloudflare.zip cloudflare_monitor.js node_modules/*`
- Create a new Lambda function and deploy the file generated above and add the following environment variables:
    - CLOUDFLARE_KEY: An API key retrieved from Cloudflare.
    - CLOUDFLARE_EMAIL: The Cloudflare email associated with the API key.
    - PLUGIN_HOST: The name of the host where the plugin is running. If running on Lambda, use "lambda.amazonaws.com".
    - NEW_RELIC_LICENCE_KEY: The licence key retrieved from New Relic.
- Set "Runtime" to Node.js 6.10
- Set Handler to cloudflare_monitor.start
- Click "Test" (or "Save and test") and configure a dummy Test event to verify the setup works.
- After creating the function and verifying that it runs successfully, create a trigger to make it run on a schedule:
    - Click on the "Triggers" tab.
    - Click on "+ Add trigger".
    - Click on the empty box on the left.
    - Select "CloudWatch Events".
    - Select "Create a new rule"
    - Give the rule a descriptive name like "NewRelicCloudflareEvery<X>Minutes", substituting <X> with the frequency with which you wish this function to run.
    - Select "Schedule expression" and enter "rate(<X> minutes)", again replacing <X> by the desired frequency.
    - Click "Submit".
- Click "Save"

---

# License

As-Is. Use at your own risk etc.

----