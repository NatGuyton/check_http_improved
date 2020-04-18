# check_http_improved

This came about due to a desire to have a decent multi-step http monitor from the command line, possibly used with nagios/icinga and that monitoring ecosystem, as well as a script monitor replacement for the http monitor in our antiquated SiteScope, which has multi-step capability, but does not speak TLS 1.1 or higher.

This should ideally be used with Python 3.4 or higher if you want SNI support.  

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
* Output formats in json (default), raw human readable, and nagios/icinga plugin format

## Usage

```
usage: check_http_improved [-h] [-o {raw,json,nagios}] [-m MONITOR]

optional arguments:
  -h, --help            show this help message and exit
  -o {raw,json,nagios}, --output {raw,json,nagios}
                        optional output formats
  -m MONITOR, --monitor MONITOR
                        monitor spec file, defaults to monitor.json
  -M MONITOR_JSON, --monitor_json MONITOR_JSON
                        json text of a monitor spec


./check_http_improved   (looks for and loads "monitor.json")

./check_http_improved  -m <json filename> -o nagios

./check_http_improved  -M '{"steps":[{"url":"https://example.com","contentcheck":"foo"}]}' -o nagios
```

## Monitor Spec - JSON Input or File 
```
{
  "name": "my monitor name",      (optional)
  "debug": "True",                (default "False")
  "steps": [
    { 
      "desc": "login to xyz",                         (optional)
      "url": "https://somesite.example.com",          (url or "previous location" if previous step had a Location redirect header)
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

- preset cookie capability?
- on Multistep, allow taking a form from one step and submitting it on the next step, optionally adding values, similar to https://mechanize.readthedocs.io/en/latest/
- proxy capability (std or socks proxy)
- consider kerberos support - https://github.com/requests/requests-kerberos
- see about generating HAR files for requests?   https://stackoverflow.com/questions/56442782/how-to-serialize-requests-response-object-as-har
