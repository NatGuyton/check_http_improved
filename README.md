# check_http_improved

This came about due to a desire to have a decent multi-step http monitor from the command line, possibly used with nagios/icinga and that monitoring ecosystem, as well as a script monitor replacement for the http monitor in our antiquated SiteScope, which has multi-step capability, but does not speak TLS 1.1 or higher.

## Features

* Monitoring specs defined in JSON
* Multi-step http/https checking with session cookiejar
* Content checking
* Response time thresholds (default 10 sec)
* SSL Client certs
* BASIC Auth
* Form parameters
* HTTP Headers

## Usage

./check_http_improved   (currently loads hardcoded "monitor.json")

## JSON Spec
```
{
  "name": "my monitor name",      (optional)
  "debug": "True",                (default "False")
  "checks": [
    { 
      "desc": "login to xyz",                         (optional)
      "url": "https://somesite.example.com",
      "method": "get",                                (default "get", other options "post","put","options", etc)
      "allow_redirects": "True",                      (default False)
      "auth": ["username","password"],                (optional)
      "headers": {"X-head-1": "foo", "X-2": "bar"},   (optionally add request headers)
      "timeout": 20,                                  (default 10 seconds, decimals acceptable, ie 0.5)
      "status_pass": 401,			      (default 200, but can set expected alternate status)
      "contentcheck": "Some text in the response",    (optional)
      "cert": ["/path/cert_file","/path/key_file"],   (optional SSL client cert)
      "stop_on_fail": "False"                         (default True)
    },
    { ... }, { ... }
  ]
}
```

## Note to self/roadmap

- json output
- preset cookie capability?
- do not verify https cert
- specify IP for URL / do not use DNS (needs transport adapter, similar to https://github.com/RhubarbSin/example-requests-transport-adapter/blob/master/adapter.py)


