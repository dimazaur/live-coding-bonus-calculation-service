# Live coding bonus calculation service

## Technical requirements

A RESTful application should provide the following functionality:
1. Adding one transaction for a user by name
2. Adding a list of transactions for a user by name
3. List all of the user's transactions
4. List all transactions for the user and for the time period
5. Calculate the total points for the user
6. Calculate points per user and time period

### Bonuses are awarded according to the formula:
For every $50 spent in one transaction, the user receives 1 point.
Additionally, for every $100 spent in one transaction, the user receives 2 more points.

## Continuous integration
Running CI on each commit
https://github.com/dimazaur/live-coding-bonus-calculation-service/actions
- Maven build with tests
- Docker image build

## Prerequisites
- JDK 17
- Maven
- MySQL database 

## MySQL configuration
File src/main/resources/application.properties \
Set environment variables MYSQL_DATABASE, MYSQL_USER, MYSQL_PASSWORD \
or write in file as a plain text 

## Local start
mvn spring-boot:run

---

# Docker
## Exposed HTTP port
8080

## Docker image build
mvn clean install \
docker build -t live-coding-bonus-calculation-service --no-cache .

## Docker run container
docker run -it --network=host -p 127.0.0.1:8080:8080 \
-e MYSQL_DATABASE='jdbc:mysql://mysql.local:3306/transactions' \
-e MYSQL_USER='root' \
-e MYSQL_PASSWORD='123456' \
--name live-coding-bonus-calculation-service live-coding-bonus-calculation-service

---

## Open API
http://localhost:8080/v3/api-docs/

## Swagger
http://localhost:8080/swagger-ui/index.html

---

# Testing web services
## Postman
Postman collection file `Transactions.postman_collection.json`

## Curl
### Add one transaction for the user AAA

`
curl --location --request POST 'http://localhost:8080/transaction' \
--header 'Content-Type: application/json' \
--data-raw '{
"userId": "AAA",
"amount": 75.00,
"timestamp": "2022-10-01 01:01:01"
}'
`

### Add list of transactions for the user AAA

`curl --location --request POST 'http://localhost:8080/transaction/list' \
--header 'Content-Type: application/json' \
--data-raw '[
{
"userId": "AAA",
"amount": 50.00,
"timestamp": "2022-08-01 01:01:01"
},
{
"userId": "AAA",
"amount": 75.00,
"timestamp": "2022-08-02 02:02:02"
},
{
"userId": "AAA",
"amount": 150.00,
"timestamp": "2022-08-03 03:03:03"
}
]'`


### List all transactions for the user AAA

`curl --location --request GET 'http://localhost:8080/transaction/list/AAA'`

### List all transactions for the user AAA and the time period

`curl --location --request GET 'http://localhost:8080/transaction/date/AAA?dateFrom=2022-08-01T00:00:00&dateTo=2022-08-30T00:00:00'`

### Calculate total points for the user AAA

`curl --location --request GET 'http://localhost:8080/points/total/AAA'`

### Calculate points for the user AAA and period of time

`curl --location --request GET 'http://localhost:8080/points/AAA?dateFrom=2022-08-01T00:00:00&dateTo=2022-10-30T00:00:00'`
