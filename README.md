

# Money Lens

**Money Lens** is a simple financial advisor app built to demonstrate the [Microservice Architecture Pattern] using Spring Boot, Spring Cloud, and Docker.

<br>

![](https://cloud.githubusercontent.com/assets/6069066/13864234/442d6faa-ecb9-11e5-9929-34a9539acde0.png)
![Money Lens](https://cloud.githubusercontent.com/assets/6069066/13830155/572e7552-ebe4-11e5-918f-637a49dff9a2.gif)

## Functional services

**Money Lens** is decomposed into three core microservices. All of them are independently deployable applications organized around certain business domains.

<img width="880" alt="Functional services" src="https://cloud.githubusercontent.com/assets/6069066/13900465/730f2922-ee20-11e5-8df0-e7b51c668847.png">

#### Account service
Contains general input logic and validation: incomes/expenses items, savings, and account settings.

| Method | Path                    | Description                                  | User authenticated | Available from UI |
|--------|-------------------------|----------------------------------------------|--------------------|-------------------|
| GET    | /accounts/{account}     | Get specified account data                   |                    |                   |
| GET    | /accounts/current       | Get current account data                     | ×                  | ×                 |
| GET    | /accounts/demo          | Get demo account data (pre-filled incomes/expenses items, etc) |                    | ×                 |
| PUT    | /accounts/current       | Save current account data                    | ×                  | ×                 |
| POST   | /accounts/              | Register new account                         |                    | ×                 |

#### Statistics service
Performs calculations on major statistics parameters and captures time series for each account. Data points contain values normalized to base currency and time period. This data is used to track cash flow dynamics during the account lifetime.

| Method | Path                    | Description                                  | User authenticated | Available from UI |
|--------|-------------------------|----------------------------------------------|--------------------|-------------------|
| GET    | /statistics/{account}   | Get specified account statistics             |                    |                   |
| GET    | /statistics/current     | Get current account statistics               | ×                  | ×                 |
| GET    | /statistics/demo        | Get demo account statistics                  |                    | ×                 |
| PUT    | /statistics/{account}   | Create or update time series data point for specified account |                    |                   |

#### Notification service
Stores user contact information and notification settings (reminders, backup frequency, etc.). A scheduled worker collects required information from other services and sends e-mail messages to subscribed customers.

| Method | Path                                | Description                                  | User authenticated | Available from UI |
|--------|-------------------------------------|----------------------------------------------|--------------------|-------------------|
| GET    | /notifications/settings/current     | Get current account notification settings    | ×                  | ×                 |
| PUT    | /notifications/settings/current     | Save current account notification settings   | ×                  | ×                 |

#### Notes
- Each microservice has its own database, so there is no way to bypass the API and access persistence data directly.
- MongoDB is used as a primary database for each of the services.
- All services communicate with each other via REST API.

## Infrastructure
[Spring Cloud](https://spring.io/projects/spring-cloud) provides powerful tools for developers to quickly implement common distributed systems patterns.

<img width="880" alt="Infrastructure services" src="https://cloud.githubusercontent.com/assets/6069066/13906840/365c0d94-eefa-11e5-90ad-9d74804ca412.png">

### Config service
[Spring Cloud Config](http://cloud.spring.io/spring-cloud-config/spring-cloud-config.html) is a horizontally scalable centralized configuration service for distributed systems.

...

## Important endpoints
- http://localhost:80 - Gateway
- http://localhost:8761 - Eureka Dashboard
- http://localhost:9000/hystrix - Hystrix Dashboard (Turbine stream link: `http://turbine-stream-service:8080/turbine/turbine.stream`)
- http://localhost:15672 - RabbitMQ management (default login/password: guest/guest)

## Contributions are welcome!

**Money Lens** is open source and would greatly appreciate your help. Feel free to suggest and implement any improvements.

