# Spring Data JPA exercise 

## 1. MySQL database

Create the MySQL database.

```
cd docker
docker compose up
```

## 2. API documentation

Implement the following REST endpoints for creating and retrieving football teams.
Create an entity class for football team.

### a. Create a football team

#### Request

`POST api/teams`

```
{
 "name": "PSG",
 "city": "Paris",
 "coach": "Luis Enrique"
}
```

#### Responses

    201 CREATED

    400 BAD REQUEST 
    - when data is missing
    - when the name of the team is not unique
    - when there are already 2 teams in a city

### b. Retrieve all football teams

#### Request

`GET api/teams`

#### Responses
 
    200 OK

    [ {
	   "id": 1,
	   "name": "PSG",
	   "coach": "Luis Enrique",
	   "city": "Paris"
    }, ...]

### c. Upload file with team players (@Async, @Transactional, commons-csv)

In the folder resources/files there are csv files for creating
the players of a football team.
- Make an entity class FootballPlayer. Use the enum Position. A
footballplayer is member of a football team.
- Create the endpoint to upload the csv file.
- Use Apache commons-csv to process the csv file.
- The file is processed in an asynchronous way.
- A football player's e-mail address should be unique.
- Create a unit test for the repository method to retrieve a football player by its e-mail address.
- The process runs in a transaction - if there is an error in the file, 
all sql statements are rollbacked.

#### Request

`POST api/teams/{teamId}/players`

You should be able to add a file to your request!

#### Responses

    202 ACCEPTED
    if the file is accepted for processing
    
    404 NOT FOUND
    if there is no team with the given teamId

### d. Retrieve all football players (Pageable)

Create an endpoint to retrieve all football players in a
paged way.

#### Request

`GET api/players?page=3&size=4`
`GET api/players?page=0&size=8&sort=position,desc`


#### Responses

    200 OK 


     {
     "content": [
     {
     "id": 5,
     "name": "William Davis",
     "email": "william.davis@parisfc.com",
     "position": "DEFENDER",
     "team": "PSG",
     "city": "Paris"
     },
     {
     "id": 6,
     "name": "Michael Wilson",
     "email": "michael.wilson@parisfc.com",
     "position": "DEFENDER",
     "team": "PSG",
     "city": "Paris"
     }
     ],
     "pageable": {
     "pageNumber": 2,
     "pageSize": 2,
     "sort": {
     "empty": true,
     "unsorted": true,
     "sorted": false
     },
     "offset": 4,
     "paged": true,
     "unpaged": false
     },
     "totalElements": 11,
     "totalPages": 6,
     "last": false,
     "size": 2,
     "number": 2,
     "sort": {
     "empty": true,
     "unsorted": true,
     "sorted": false
     },
     "numberOfElements": 2,
     "first": false,
     "empty": false
     }


### d. Search football players (Specification<FootballPlayer>)

Create an endpoint to search for football players.

#### Request

`GET players/search`
```
{
"name": "liam",
"position": "DEFENDER"
}
```

#### Responses

    200 OK 


     [
	{
		"id": 5,
		"name": "William Davis",
		"email": "william.davis@parisfc.com",
		"position": "DEFENDER",
		"team": "PSG",
		"city": "Paris"
	},
	{
		"id": 10,
		"name": "James Williams",
		"email": "james.williams@parisfc.com",
		"position": "DEFENDER",
		"team": "PSG",
		"city": "Paris"
	}
    ]

### e. Retrieve a team with all football players

Create a bi-directional lazy fetched relationship between team
and player.

#### Request

`GET api/teams/{teamId}`


#### Responses

    200 OK 
        {
        "name": "PSG",
        "coach": "Somebody",
        "city": "Paris",
        "players": [
        {
        "name": "James Wilson",
        "shirtNumber": 5
        },
        {
        "name": "William Johnson",
        "shirtNumber": 13
        },
        {
        "name": "Richard Williams",
        "shirtNumber": 12
        },
        ...
        ]
        }

