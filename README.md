dns-graceful-stack-switch 
========

[![Build Status](https://travis-ci.org/floatdrop/dns-graceful-stack-switch.png?branch=master)](https://travis-ci.org/floatdrop/dns-graceful-stack-switch) [![NPM version](https://badge.fury.io/js/dns-graceful-stack-switch.png)](http://badge.fury.io/js/dns-graceful-stack-switch)

Monkey patch DNS lookup method for node.js.

### Why?

If you used node.js with disabled IPv4 - you got exception (ENETUNREACH) in most of network operations, but ```ping6 address``` working fine. 

To fix this error with minimal amount of code (you still can use [```dns.resolve6```](http://nodejs.org/docs/v0.8.25/api/dns.html#dns_dns_resolve6_domain_callback) and get valid IPv6 addresses) - monkey patched lookup method was written.

### How?

```javascript
// Monkey patch
require('dns-graceful-stack-switch')(6);
// Remove monkey patch
require('dns-graceful-stack-switch')(null, true);
```

This module returns ```function(defaultVersion, remove)```. 

 * ```defaultVersion``` - IP stack version that will be used first to lookup address. If it fails - another will be used. Defaults to `process.env.NODE_DNS_GRACEFUL_STACK_SWITCH_DEFAULT` and after that to `4`.
 * ```remove``` - remove monkeypatch. Defaults to `false`.

After executing dns.lookup will be loaded with ```require``` and ```lookup``` method will be replaced.


### Node.JS way

This bug was "[patched](https://github.com/joyent/node/commit/edd2fcccf022c7014b374674012283422faa1bed)" in Node.js, but magic option in ```net.connect``` (which gives you ability to write right http.Agent) released only in Node.js 0.11.6.

### To run the tests:

Unix/Macintosh:

    make test
