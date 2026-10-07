<h1 align="center">Wondr Word · Amusement Park Booking</h1>

![Wondr Word](./Wonderland_Frontend/Images/image.png)

A Spring Boot REST API, with an HTML/CSS/JavaScript frontend, for an amusement park: visitors plan their trip, buy tickets for activities and learn about attractions; admins manage activities, customers and tickets.

**Portfolio:** [utkarash-thakur.vercel.app](https://utkarash-thakur.vercel.app)

## Highlights

- REST API for customers, activities and tickets, with separate `ADMIN` and `USER` roles.
- Spring Security with JWT: one filter issues the token at sign-in, another validates it on every request.
- Bean validation on request bodies, a `TicketDTO`, and a global exception handler that returns consistent error details.
- Pagination for admin and activity lists; filters and sorting for activities by price, name and date.
- Soft delete on users and activities, with created and updated timestamps on every record.

## Tech stack

| Area | Tools |
| --- | --- |
| Backend | Java 17, Spring Boot 3, Spring Web, Spring Data JPA, Lombok |
| Security | Spring Security, JWT |
| Database | MySQL |
| Docs and testing | springdoc-openapi (Swagger), Postman |
| Frontend | HTML, CSS, JavaScript |

## Data model

`Admin` and `Customer` (both extend `AbstractUser`) · `Activity` · `Ticket`

A customer has many tickets; each ticket is for one activity, with a visit date, number of people and price.

![ER diagram](./Wonderland_Frontend/Images/ER.jpg)

## API

**Public**
- `POST /customers/registerCustomer`: register a customer
- `POST /admin/registerAdmin`: register an admin (open only while no admin exists; after that, a signed-in admin must call it)

**Customer (`USER` role)**
- `GET /customers/signin`: sign in; the JWT comes back in the response header
- `PUT /customers/update/{customerId}`, `DELETE /customers/delete/{customerId}`, `GET /customers/{customerId}`
- `GET /customers/activity/all`, `/customers/activity/getActivitiesByCharge`, `/customers/activity/getAllActivitiesByDate`: browse activities
- `POST /customers/ticket/{customerId}/{activityId}`: book a ticket
- `PUT | GET | DELETE /customers/ticket/{customerId}/{ticketId}`: manage a ticket
- `GET /customers/ticket/history/{customerId}`, `/todayHistory/{customerId}`, `/fair/{customerId}`: history and total fare

**Admin (`ADMIN` role)**
- `GET /admin/signin`, `GET /admin/all`, `GET /admin/{adminId}`, `DELETE /admin/delete/{adminId}`
- `GET /admin/customers`, `GET /admin/customers/{customerId}`, `DELETE /admin/customers/delete/{customerId}`
- `POST /admin/activity/add`, `PUT /admin/activity/update/{activityId}`, `DELETE /admin/activity/delete/{activityId}`
- `GET /admin/activity/all` and filter and report endpoints (by charge, by date, by customer)
- `GET /admin/ticket/getAllTicket`, `GET /admin/ticket/{ticketId}`

## Project structure

```
WonderWorld_Park_BK/src/main/java/com/masai
├── controller/   Admin, Customer, Activity, Ticket controllers
├── service/      business logic (+ UserDetailsService for Spring Security)
├── repository/   Spring Data JPA repositories
├── model/        AbstractUser, Admin, Customer, Activity, Ticket, Role
├── security/     AppConfig, JWT generator and validator filters
├── DTO/          TicketDTO
└── Exception/    custom exceptions + GlobalExceptionHandler
Wonderland_Frontend/   HTML, CSS and JavaScript pages
```

## Run it locally

1. Install Java 17 and MySQL, then create the database:
   ```sql
   CREATE DATABASE wonder;
   ```
2. Set your MySQL username in `WonderWorld_Park_BK/src/main/resources/application.properties`. Tables are created on first start.
   The password is read from the `DB_PASSWORD` environment variable and is never stored in the repo. Set it in the same terminal before starting:
   ```bash
   export DB_PASSWORD=your_mysql_password       # macOS, Linux, Git Bash
   ```
   ```powershell
   $env:DB_PASSWORD = "your_mysql_password"    # Windows PowerShell
   ```
3. Start the API from `WonderWorld_Park_BK`:
   ```bash
   ./mvnw spring-boot:run
   ```
   It runs on `http://localhost:8888`.
4. Open `Wonderland_Frontend/index.html` in a browser for the frontend.

## Author

**Utkarash Thakur**, Backend Engineer · [Portfolio](https://utkarash-thakur.vercel.app) · [LinkedIn](https://www.linkedin.com/in/utkarash-thakur/)
