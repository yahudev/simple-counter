# Simple Counter

## Requirement

Use case
- end user click button to send vote
- boardcast vote distribution for graph ploting to device, for last 10 mins 

Env
- devices on street
- kiosks with touch screen
- no abuse
- security protected

UI
- current vote of each candidate
- ploting graphs 

Operation
- performance. repsonse by 1s
- availability, high available
- scalability, horizonatally scalable; auto scale up/down base on load?

## Initiative

- enable *realtime* vote distribution boardcasting

## Assumptions

- in hong kong only
- expected response time to end user is 1s


# Design

## Model

Candidate
- id: String
- displayName: String

Vote
- candidate: Candidate.id 
- created: timestamp
- deivce: deviceId

To query sum of vote group by candidate:
```js
db.Vote.aggregate(
  [
    {
      $group : {
        _id : { candidate: $candidate },
        count: { $sum: 1 }
      }
    }
  ]
)
```




## System Archtecture

![Architecture](https://www.planttext.com/plantuml/img/TPJ1Rjim38RlVWgUkwP1i5lH5KsJmzgbG04Fwo7eWPOPMxCY6XATB7lwKRRJfcZccwXVvCzFbE-YO91kwxBCSqcmS9Qym3e3k2bkVrYEpRnjdJKebltDjBZDNbKf1C5MjG1lO3sSUTKZdOLlyFtdqsgwoFFfOwwXAc1RG-jOW7nbFQa2bb-lVaVT39qNkqsX8l0-KiZ8bv3IPraMo0ZwoX6iYgEX5MC9z-ZG6mhdtQoAv6G2WJknleA7PsZHD2HGO2HW3v6xO6ZoyFobvI3Ggx4JXcXGgwcD7GN0jeMkdPQyzr0SmANAfYRJd7ZiaUM3VWqmU1pN_y7cQ3Fu76J9QwhqnYPoydfb-GEULEWnxokGtsbW_fB3olhkJy9OSx1_sGkwlWKij8lAxiY3OUyLeqrTYpMDreZnf84I1Niiom5nty0J-ufXBWxZUSJB4V62dHkonsjOe2-jvgDB-9Lo9q4NqmriqMD5PG_M6e8D1zXpQ3JN9c5LpQWcC0f3fUOzCsXU4BH3MwOBKO-d5AdAfUBeuyEb3J2Fs46lL7D6u6AyqnP5AFWq187GzN1tgJTz1HAXO0obmIX7HnUcPexAiTRUtU5jWfpTMb6RZUUzexAYX6Lv3qAczkouEa1irN_ejXUd8tqwCBDtuF1vP60ahz_u92UJz0wkuOMAOKOoNMfmF7kwCOPqSrba7sIpCxhPKPZTgLNz5Vm3)


## Auto scaling

The application can be horizontally scaled since it's stateless. 

- Using EC2 Auto Scaling, Application Load Balancers, Cloud Watch. The application can be horiztonally auto-scaled up and down.
- Ensure session stickiness is enabled Application Load Balancers, websocket is supported by default in standard HTTP, HTTPS ports
- Health checks assure connections aren't routed to unhealthy instances, Setup scaling policies so that we can do auto scale up and down base on CPU utilization. Metrics that's used for CPU utilization monitoring comes from CloudWatch.
