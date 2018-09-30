# check_http_improved

This came about due to a desire to have a decent multi-step http monitor from the command line, possibly used with nagios/icinga and that monitoring ecosystem, as well as a script monitor replacement for the http monitor in our antiquated SiteScope, which has multi-step capability, but does not speak TLS 1.1 or higher.

## Features

* Monitoring specs defined in JSON
* Multi-step http/https checking with session cookiejar
* Content checking
* Response time thresholds (default 10 sec)
* SSL verify disable option or specify alternate CA file
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
      "params": {"var1":"val1","var2":"val2"},        (optional form data)
      "auth": ["username","password"],                (optional)
      "headers": {"X-head-1": "foo", "X-2": "bar"},   (optionally add request headers)
      "timeout": 20,                                  (default 10 seconds, decimals acceptable, ie 0.5)
      "conn_timeout": 3.5,                            (optionally can specify separate connection and read timeouts instead of the overall timeout - defaults 3.5, 10 if one not given)
      "read_timeout": 10,
      "data": "...",                                  (optional POST data)
      "json": "...",                                  (optional JSON POST data, sets content-type)
      "status_pass": 401,			      (default 200, but can set expected alternate status)
      "contentcheck": "Some text in the response",    (optional)
      "cert": ["/path/cert_file","/path/key_file"],   (optional SSL client cert)
      "disable_sni_warning": "True",                  (default False)
      "ssl_verify": "/path/to/ca_bundle",             (default True, also can be "False", or path to ca file; the default is from python package Certifi, not OS. https://certifiio.readthedocs.io/en/latest/)
      "stop_on_fail": "False"                         (default True)
    },
    { ... }, { ... }
  ]
}
```

## Note to self/roadmap

- json output
- preset cookie capability?
- specify IP for URL / do not use DNS (needs transport adapter, similar to https://github.com/RhubarbSin/example-requests-transport-adapter/blob/master/adapter.py)
- on Multistep, allow taking a form from one step and submitting it, optionally adding values
- proxy capability (std or socks proxy)

