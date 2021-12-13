---
title: Dos Protected Resource

description: 
weight: 1800
doctypes: [""]
toc: true
---

> Note: This feature is only available in NGINX Plus with AppProtectDos.

> Note: The feature is implemented using the NGINX Plus [NGINX App Protect Dos Module](https://docs.nginx.com/nginx-app-protect-dos/configuration/).


## Dos Protected Resource Specification

Below is an example of a dos protected resource.
```yaml
apiVersion: appprotectdos.f5.com/v1beta1
kind: DosProtectedResource
metadata:
  name: dos-protected
spec:
  enable: true
  name: "my-dos"
  apDosMonitor: 
    uri: "webapp.example.com"

```

{{% table %}}
|Field | Description | Type | Required |
| ---| ---| ---| --- |
|``enable`` | Enables NGINX App Protect Dos. | ``bool`` | No |
|``name`` | Name of the protected object, max of 63 characters. | ``string`` | No |
|``apDosMonitor.uri`` | The destination to the desired protected object. [App Protect Dos monitor](#dosprotectedresourceapdosmonitor) Default value: None, URL will be extracted from the first request which arrives and taken from "Host" header or from destination ip+port. | ``string`` | No |
|``apDosMonitor.protocol`` | Determines if the server listens on http1 / http2 / grpc. [App Protect Dos monitor](#dosprotectedresourceapdosmonitor) Default value: http1. | ``enum`` | No |
|``apDosMonitor.timeout`` | Determines how long (in seconds) should NGINX App Protect DoS wait for a response. [App Protect Dos monitor](#dosprotectedresourceapdosmonitor) Default value: 10 seconds for http1/http2 and 5 seconds for grpc. | ``int64`` | No |
|``apDosPolicy`` | The [App Protect Dos policy](#dosprotectedresourceapdospolicy) of the dos. Accepts an optional namespace. | ``string`` | No |
|``dosSecurityLog.enable`` | Enables security log. | ``bool`` | No |
|``dosSecurityLog.apDosLogConf`` | The [App Protect Dos log conf](/nginx-ingress-controller/app-protect-dos/configuration/#app-protect-dos-logs) resource. Accepts an optional namespace. | ``string`` | No |
|``dosSecurityLog.dosLogDest`` | The log destination for the security log. Accepted variables are ``syslog:server=<ip-address | localhost | dns-name>:<port>``, ``stderr``, ``<absolute path to file>``. Default is ``"syslog:server=127.0.0.1:514"``. | ``string`` | No |
{{% /table %}}

### DosProtectedResource.apDosPolicy

The `apDosPolicy` is a reference (qualified identifier in the format `namespace/name`) to the policy configuration defined as an `ApDosPolicy`.

### DosProtectedResource.apDosMonitor

This is how NGINX App Protect DoS monitors the stress level of the protected object. The monitor requests are sent from localhost (127.0.0.1).

### Invalid Dos Protected Resources

NGINX will treat a dos protected resource as invalid if one of the following conditions is met:
* The dos protected resource doesn't pass the [comprehensive validation](#comprehensive-validation).
* The dos protected resource isn't present in the cluster.

### Validation

Two types of validation are available for the Dos Protected resource:
* *Structural validation*, done by `kubectl` and the Kubernetes API server.
* *Comprehensive validation*, done by the Ingress Controller.
