![ipost-logo](https://github.com/FIPost/docs/blob/master/assets/logo-name.png?raw=true)
<h3 align="middle">
  <a href="https://github.com/FIPost/docs">Documentation</a>
  <a>•</a>
  <a href="https://github.com/FIPost/docs/blob/master/CONTRIBUTING.md">Contributing</a>
  <a>•</a>
  <a href="https://github.com/FIPost/docs/blob/master/CONTACT.md">Contact</a>
</h3>

# Ocelot API-Gateway

1. [About](#About)
2. [Setting Up](<#Setting Up>)
3. [Routes](#Routes)
4. [Authentication](#Authentication)
5. [Authorization](#Authorization)

## About

Ocelot is a library for ASP.NET that allows you to use an app as an API-Gateway. 

- [GitHub](https://github.com/ThreeMammals/Ocelot)
- [Documentation](https://ocelot.readthedocs.io/en/latest/index.html)

## Setting Up

To create an API-Gateway you need to perform the following steps:
- Create an empty ASP.NET Core Web API.
- Include Ocelot as a NuGet package (version 16.0.1 for ASP.NET Core 3.1).
- Program the application to use Ocelot.
- Create an ocelot.json file for route configuration.

[Getting Started](https://ocelot.readthedocs.io/en/latest/introduction/gettingstarted.html)

## Routes

Routes are what ocelot uses to set up endpoints and to connect those endpoints to the right microservices. Routes are defined in a json file called 'ocelot.json', the file has the following structure.

```json
{
    "Routes" : [
        {
            "DownStreamPathTemplate": "/service/endpoint",
            "DownStreamScheme": "http",
            "DownStreamHostAndPorts": [
                {
                "Host": "localhost",
                "Port": 5001
                }
            ],
            "UpStreamPathTemplate": "/gateway/endpoint",
            "UpStreamHttpMethod": []
        }
    ]
}

```

| Name | Description |
|---|---|
| DownStreamPathTemplate | The URL on the service where the gateway will redirect the request |
| DownStreamPathScheme | Either http or https depending on what the service is using |
| DownStreamHostAndPorts | The host name and port of the service. The host name could be the IP or localhost. If using in docker the host name needs to be the name of the container |
| UpStreamPathTemplate | The URL on the gateway the frontend will call |
| UpStreamHttpMethod | The methods that will be redirected by the gateway (GET, POST, PUT, DELETE, etc.). Leave empty to allow all |

To add a route add an extra entry to the ocelot.json file in the same structure as the snippet above.

[Ocelot Routing](https://ocelot.readthedocs.io/en/latest/features/routing.html)

## FAQ

**Running with docker** </br>
If docker is used for running the services, the host for the service is the name of the docker container. Using localhost will not find the service.

**HTTP 'OPTIONS' method** </br>
Some frontends will send a OPTIONS request before any other requests (GET, POST, etc.), this has to do with CORS. If you add OPTIONS to the UpStreamHttpMethod of the ocelot.json config file, you allow that method to be delegated.

[
    NOTE \[SJORS\]: usefull for later when authorization and authentication is added
    Authentication - (https://ocelot.readthedocs.io/en/latest/features/authentication.html)
    Authorization - (https://ocelot.readthedocs.io/en/latest/features/authorization.html)
]: #