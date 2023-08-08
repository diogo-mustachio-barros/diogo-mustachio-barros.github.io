---
# hidden: true
number: 1
title: "API-EstadoServicoML"
techs: 
  - Java
  - Maven
status: complete
toc: true
repo: https://gitlab.com/diogofpbarros/api-estadoservicoml
---

<!-- ## Abstract -->
Java API library adapting Lisbon's subway system API for waiting times, line state and station 
  information. Meant for integration in other applications by providing a simple 1-to-1 conversion
  of the original API calls to Java ones. 

## Motivation
Since forever I've been obsessed with the subway, it was like an underground world. I would try to 
  visualize the network of tunnels going left, right, up and down. It has always amazed me the 
  whole logistics of subways going back and forth maintaining a steady pace to guarantee a good
  service.

When I went to university the subway became part of my daily commute. This presented an opportunity
  to obsess again. This time I started timing every travel time between stations and line changes.
  Excel was my first tool to analyze every bit of data gathered. In a week, I had all the timings
  down to a *science*.

However, it was still rudimentary. The time it would take for the next subway to arrive, and the
  timing between subways in different lines when changing were still unpredictable. 
  ***Could I track the subways in realtime?***

Surprisingly, yes! There was no app at the time, but there was an API that with some clever usage
  could estimate with a degree of precision where every train was. Thus, my first project emerged.
  The first step was to adapt a REST API into a Java abstraction that was easier to use going 
  forward.

## Design
The idea is very simple: an abstraction layer where every API call is translated into a Java 
  method which does the correct HTTP request, and every API object will have a corresponding 
  Java object to give back to its client.
  There are many improvements/optimizations that we can do, but at this time the objective is for
  it to work without hassles.

## Implementation
Originally this was a barebones Java project, but the need for JSON parsing/reading (from API
  responses) meant Maven had to be involved.

The main class to use is `MetroAPIAdapter` which exposes all API calls:
- Line state
  - `getEstadoLinhaTodos()` - for every line
  - `getEstadoLinha(<metro line>)` - for a single line
- Train times
  - `getTempoEsperaTodos()` - for every station in every line
  - `getTempoEsperaLinha(<metro line>)` - for every station in a given line
  - `getTempoEsperaEstacao(<stop id>)` for a single station (by id)
- Station information
  - `getInfoEstacaoTodos()` - for every station in every line
  - `getInfoEstacao(<stop id>)` - for a single station
  - `getInfoDestinosTodos()` - for all destinations (final stations)
- Departure times
  - `getInfoIntervalosLinhaDiaHora(<metro line>, <day>, <hour>)` - for all stations in a given 
    line, day and hour
  - `getInfoIntervalosLinhaDia(<metro line>, <day>)` - for all stations in a given line and day

{: .notice}
**PS:** this was my first 'project' and the original API was also in Portuguese so I just copied
  the names as-is. If you, the reader, want to use this and would prefer a proper implementation
  with better names don't hesitate to contact me!

Each method uses an `HttpURLConnection` object to prepare and execute REST calls. There is no 
  persistent connection so each method call is a new connection.

The API makes use of a config file `config.properties` that contains three values:
- Base URL for HTTP requests
- API key
- API secret

When there is no config file, one is created with only the base URL set. API key and secret must
  be set manually for API calls to succeed (check the **How to use** section for steps on 
  generating these 
  values). Every time an API call is made, a check is made to ensure there is a valid 
  authentication token for HTTP requests.

Because of this token, any API call can throw a `TokenGenerationException` if it fails to 
  generate a token (for example if given incorrect API key and/or secret). Other than that,
  the other exceptions might only be thrown the first time `MetroAPIAdapter` is instantiated,
  when it is needed to read from the config file. 

## How to use

Before using this API adapter you must first do one of the following:
  - Clone (or download) the [repository](https://gitlab.com/diogofpbarros/api-estadoservicoml) 
    and build the `.jar` using Maven with `maven package`
  - Download the `.jar` file directly from the repository [here](https://gitlab.com/diogofpbarros/api-estadoservicoml/-/blob/master/metroapi-1.0.0-.jar)

After adding the `.jar` to your build path you can start using it by filling the 
  `config.properties` file and instantiating the `MetroAPIAdapter` object with `getInstance()`
  method. 

{: .notice--danger}
Remember to get your API key and secret from the [API website](https://api.metrolisboa.pt/store/site/pages/list-apis.jag)
  by registering, creating an application, subscribing to the API and fill in the corresponding
  fields in the `config.properties` file. Failing to do so will prevent you from using the REST API.

You are now ready to start making API calls at your will.

{: .notice--warning}
The API endpoint is HTTPS, and sometimes methods might fail due to the SSL certificate. To fix 
  this I followed the solution at [Stack Overflow](https://stackoverflow.com/questions/1757295/using-https-with-rest-in-java).
  TL;DR is to manually download the API endpoint's SSL certificate and add it to the default 
  certificate store (or other if you wish).

<!-- ## Demo -->
An example of how to use the API to check the status of the Yellow line:
```java
try {
  MetroAPIAdapter api = MetroAPIAdapter.getInstance();
  MetroLineState state = api.getEstadoLinha(MetroLine.Amarela);
  
  System.out.println(state);
} catch (Exception e) {
  System.out.println("An error ocurred");
//			e.printStackTrace();
}
```