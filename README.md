# Requirement

Use case
- end user click button to send vote
- realtime boardcast stats to device, for last 10 mins graph ploting

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

## Assumptions

- in hong kong only
- expected response time to end user is 1s
- realtime time update of events t

# Design

## Model

Candidate
- id: String
- displayName: String

Vote
- candidate: Candidate.id 
- created: timestamp
- deivce: deviceId

## System Archtecture

![Architecture](https://www.planttext.com/plantuml/svg/T9DDRjim48NtFCNR5uvXT5UaGTnuqQGA4405iZ2o8Acnn9hc2FonsrwjYnwfLoWfoR2DkalElD5xyv7wy-ltVOZInxIpGZXk20_Ma8jO2-1MtNqn7BRBlhT6osZOtSxLdNZBoaZmfkqYU8FrEFEMHv81kTOumaTy_lfSpH_gUlvKvqxhPT-XjnnF2gpUttmBkUcJYsPNIihp8P0Nv1eK5o649nsbQosXKiTS5Si6dwILJfE_7gfI9T0CdbCqhiNZ2tgfjnIXP22mG-3aDSqUNzxrx2C6jKoH4RJCRgcVDtnmK3zWYvtkI6_FWWsuQl9xybW3Ox3f_1zabZPZk55M0ig-Fm49N0BU6BvWKeoUwi61t3uNbgyrAkFIzp1-u6YLSxP68tISR0D5o7SRoL88s30wzkOdkoZji61cShQCjfmQh4R0moRFx5h6CbePhWDUJO1JA7XtqicM6kJBsnjDsT3Zm7wRfPk2xzBk7DxYv8b_yM4o5s9wcRDPUGSVy4fgn2NoCHUdqVF6LChDbH8NiiKLtU8e5FSvQFuZ_Wem0000)

@startuml

node Client [
  <b>KioskApplication</b>
]
note right of [Client]
  ui engine = HTML/CSS/JS
  graph plot engine =  D3.js
  runtime = browser
end note


rectangle Lb [
  <b>Load Balancer</b>
]
note right of [Lb]
  - routing IP packet base on source-IP+port 
  - auto scale up/down by load check
end note

node Server [  
  <b>Server Application</b>
]
note left of [Server]
  app engine = socket.io
  runtime = nodeJS  
  env = AWS Linuxs
  application logic should be stateless fo horizontal scaling
end note

cloud Db [
    <b>Mongo Atlas</b>
]
note right of [Db]
  - using managed service for work offloading
end note


node Server1 [
    <b> Server Application </b>
]


Client -- Lb : Websocket
Lb -- Server : Websocket
Server -- Db

Lb -- Server1 : Websocket
Server1 -- Db


@enduml


