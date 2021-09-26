# Uber Clone App - Bundle

## getting started
```shell script
./run
```
executing the run script, all servers associated with this project will run<br>

## using the app

# Uber Clone App - Core Service

## getting started
```shell script
npm i
npm start
```

the start script will execute the HTTP and GraphQl servers.

## to access auth token from HTTP server
to create a new user:
````shell script
method: POST
endpoint: /auth/register
body: {email, password}
````
this will return "Created" <br><Br>

to log into an existing user:
```shell script
method: POST
endpoint: /auth/login
body: {email, password}
```
this will return OK (200), and an auth token in the header

## to access the GraphQL server
after logging in and receiving an auth token, access the GraphQL Sandbox at 
[localhost:8081/graphql](http://localhost:8081/graphql), and add an Authorization header containing your auth token

<img src="https://i.imgur.com/1fylVfq.png" alt="MarineGEO circle logo" style="height: 100px; width:100px;"/> <br>

you can check your connection to the server with a health check query
```graphql
query Query {
  health
}
```

## to use this service
this core service utilizes three other services.
* pricing 
* booking
* trip
<br><br>

### Order of Operations
#### beginTripSession
```graphql
mutation Mutation {
  beginTripSession {
    success {
      id
      status
    }
    error {
      message
    }
  }
}
```
this should return a response like

````json
"beginTripSession": {
      "success": {
        "id": "97411c44-29b2-4c87-8b63-7e210240e275",
        "status": null
      },
      "error": null
    }

````
keep record of the id

#### getTripEstimate
```graphql
mutation{
    getTripEstimate(pickup: {lat: 1, lng:1}, dropoff: {lat: 1, lng:1}) {
      success {
        estimate
      }
      error {
        message
      }
    }
}
```
this should return a response like 
```json
"getTripEstimate": {
      "success": {
        "estimate": 74.91
      },
      "error": null
    }
```

#### bookTrip
```graphql
mutation {
    bookTrip(booking: {pickup: {lat: 1, lng:1}, dropoff: {lat: 1, lng:1}}){
      success {
        status
      }
      error {
        message
      }
    }
  }
```
this should return a response like
````json
"bookTrip": {
      "success": {
        "status": "Created"
      },
      "error": null
    }
````
#### tripSub
utilizing the id returned from the beginTrip mutation
```graphql
subscription  {
    tripSub(tripId: "97411c44-29b2-4c87-8b63-7e210240e275") {
      success {
        message
        location {
          lat
          lng
        }
        tripId
      }
      error {
        message
      }
    }
  }
```

