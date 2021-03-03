# docker-gridc-squid
Securely connect your private [Gridlastic][gridlastic] selenium grid to your test environment. Access web sites serving https via non caching Squid proxy.


## Quick start

Set environmental values of your Gridlastic credentials (available after grid start in your Gridlastic dashboard).

```
Linux/Mac
export GRIDC_ENDPOINT_SUBDOMAIN=<your Gridlastic Connect subdomain>
export GRIDC_USERNAME=<your Gridlastic grid username>
export GRIDC_ACCESS_KEY=<your Gridlastic grid access_key>
export GRIDC_REMOTE_PORT=9999


Windows (CMD)
set GRIDC_ENDPOINT_SUBDOMAIN=<your Gridlastic Connect subdomain>
set GRIDC_USERNAME=<your Gridlastic grid username>
set GRIDC_ACCESS_KEY=<your Gridlastic grid access_key>
set GRIDC_REMOTE_PORT=9999


Windows (Powershell)
$Env:GRIDC_ENDPOINT_SUBDOMAIN = "<your Gridlastic Connect subdomain>"
$Env:GRIDC_USERNAME = "<your Gridlastic grid username>"
$Env:GRIDC_ACCESS_KEY = "<your Gridlastic grid access_key>"
$Env:GRIDC_REMOTE_PORT = 9999
```
Port `9999` is just an example, select any port between `9000-9999` that is not in use by another tunnel or leave empty for a random port assignment.  


Start:

    $ docker-compose up


In your selenium code, create a selenium proxy like:

```
String proxy_server = "your_gridlastic_connect_subdomain.gridlastic.com:9999";
org.openqa.selenium.Proxy proxy = new org.openqa.selenium.Proxy();
proxy.setHttpProxy(proxy_server).setFtpProxy(proxy_server).setSslProxy(proxy_server);
capabilities.setCapability(CapabilityType.PROXY, proxy)
```

In this case the port `9999` was used but if you left it empty, retrieve the port assigned using the [Rest API][gridlastic-connect-api] like:

```
http://localhost:3030/api/tunnel?name=squid_proxy
```


and then in your selenium code, access a web site like:

    $ driver().get("https://google.com");



You can also access a local site like:


    $ driver().get("https://host.docker.internal");
    
When accessing a local https site serving valid or self signed ssl, the domain names do not match so the browser displays a certificate error message which can be resolved by adding flags to your selenium code like for Chrome:

    $ options.addArguments("ignore-certificate-errors");
    
Read more about using a [Squid proxy][gridlastic-squid-proxy] in your selenium testing.
 
## Feedback

Report issues/questions/feature requests at `support@gridlastic.com`.


[gridlastic]:       	https://www.gridlastic.com/
[gridlastic-squid-proxy]:       	https://www.gridlastic.com/gridlastic-connect.html#squid
[gridlastic-connect-api]:	https://www.gridlastic.com/gridlastic-connect.html#dynamic_tunnel_creation