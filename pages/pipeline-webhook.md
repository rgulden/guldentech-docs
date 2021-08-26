# Configuring webhook

In your github repository, under Settings > Webhooks, you should see rancher created a webhook already for your pipeline.

The only issue is, Rancher populates it with the wrong URL for the hook. For hooks to work update the 100.x.x.x:5443 address with rancher.guldentech.com like seen below.

