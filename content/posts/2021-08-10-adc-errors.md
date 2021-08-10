---
title: Application Default Credentials errors with Workload Identity
author: hbanken
type: post
date: 2021-08-10T15:11:00+00:00
url: /2021/08/10/application-default-credentials-errors/
categories:
  - cloudnative
  - kubernetes
  - gcloud

---

When me and my collegues at Q42/Hue inspect our applications and there error messages they produce, we sometimes see this gem:

```
Could not load the default credentials. Browse to https://cloud.google.com/docs/authentication/getting-started for more information.
```

This seems to be a nice error message, as it clearly explains what happens and where more
information can be found. However, the message and the background information do
not capture all peculiarities that can cause this error. Particularly, we see
this error often when new Kubernetes Pods start: the Metadata API
(169.254.169.254) is not yet ready to serve the Google Cloud Service Account
provided through Workload Identity.

Whenever you encounter such error, be sure to relax (you've done nothing wrong)
and optionally add some delays or retries/back-offs to reduce the noise of these
glitches.

Hopefully my post saves some fellow developers some headaches!
