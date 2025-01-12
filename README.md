# SW Architectures Project Documentation

## Team Members

| Student Name      | Email                 | Matricola |
|-------------------|-----------------------|-----------|
| Stefano Zogno     | 1001900@stud.unive.it | 1001900   |
| Alessandro Dussin | 881424@stud.unive.it  | 881424    |
| Lukas Hrmo        | 1001537@stud.unive.it | 1001537   |
| Fedor Kazin       | 908385@stud.unive.it  | 908385    |
| Lorenzo Facchin   | 903942@stud.unive.it  | 903942    |

---

## Architectures

![Alt text](sw_architecture.png)

The software is a modern web application, developed with the most popular and modern frameworks. For this project the service-based architecture was selected, divided into the following components:

1. **Client**:  
   The user interface of the application, developed with **Angular 17**.

2. **API Gateway**:  
   The mediator between the client and the services. All client requests are sent to this component, which redirects themti the correct service based on the URL. Then it receives a response from the service and returns it to the client. In addition the API Gateway verifies if the provided JWT Token is still valid for each request which requires the user to be authenticated, interacting with management service. Thanks to the API Gateway, we reduce the coupling in the architectures, since we have just one entrance point.

3. **Services**:
  - **Management**: used for everything concerning the session and user management. It provides API to create, edit, enable/disable user, login, logout, and change the password
    - Languages and frameworks: **PHP 8.3**, **Symfony 6.4**, **Doctrine ORM 3.2**
  - **Doctor**: used for everything concerning patient from the point of view of medical staff, namely doctors and nurses. In particular it provides API for managing medical procedures (create, edit, delete and list), patient vitals (create, edit, delete and list) and assigning patients to a doctor.
    - Languages and frameworks: **Java 17**, **Spring Boot 3.4.0**
  - **Emergency**: used to manage the emergencies, in particular it provides API for managing hospital beds, emergency visits, patient vitals and invoices.
    - Languages and frameworks: **Java 17**, **Spring Boot 3.4.0**
  - **Reception**: used by the secretaries and provides API for getting information about patients.
    - Languages and frameworks: **C# .NET 8.0**, **ASP.NET Framework**, **EntityCore ORM**
  - **Cronjob**: this service is a set of scripts that generate periodic reports and backup the database.
    - Languages and frameworks: **Python 3.12**

4. **Database**:  
   A shared **Postgres 16** database stores application data and is accessed by all services.

---

## How to Run the Application

Follow these steps to run the application:

### 1. Database

- Add a `.env` file in the root directory of the repository with the following content:
  ```env
  POSTGRES_USER=root
  POSTGRES_PASSWORD=root
  ```
- Run:
  ```bash
  docker-compose up --build -d
  ```
- Wait until the process finishes; it also creates the `net_storage` network.

### 2. API Gateway

- **For Windows Users**: Remove this line from the `docker-compose.yml`:
  ```yaml
  gateway -> platform -> linux/x86_64
  ```
- Run:
  ```bash
  docker-compose up --build -d
  ```

### 3. Management

- Run:
  ```bash
  docker-compose up --build -d
  ```
- Once completed, execute:
  ```bash
  docker exec --workdir /var/www/html service-management composer install --no-interaction --optimize-autoloader
  ```

### 4. Emergency

- **For Windows Users**: Remove this line from the `docker-compose.yml`:
  ```yaml
  service-emergency -> platform -> linux/x86_64
  ```
- Run:
  ```bash
  docker-compose up --build -d
  ```

### 5. Doctor

- **For Windows Users**: Remove this line from the `docker-compose.yml`:
  ```yaml
  service-doctor -> platform -> linux/x86_64
  ```
- Run:
  ```bash
  docker-compose up --build -d
  ```

### 6. Reception

- Run:
  ```bash
  docker-compose up --build -d
  ```

### 7. Cronjobs

- Run:
  ```bash
  docker-compose up --build -d
  ```

### 8. Client

- Run:
  ```bash
  docker-compose up
  ```

Once all services are running, you can access the application at:
[http://localhost:4200](http://localhost:4200)

---

## Credentials

The database is seeded with default users for each role. Use the credentials below to log in:

- **Admin**:
  - Username: `admin`
  - Password: `P4ssword1@`

- **Doctor**:
  - Username: `doctor1` (Replace `1` with `2`, `3`, `4`, etc., for other doctors)
  - Password: `P4ssword1@`

- **Nurse**:
  - Username: `nurse1` (Replace `1` with `2`, `3`, `4`, etc., for other nurses)
  - Password: `P4ssword1@`

- **Secretary**:
  - Username: `secretary1` (Replace `1` with `2`, `3`, `4`, etc., for other secretaries)
  - Password: `P4ssword1@`

- **Patient**:
  - Username: `patient1` (Replace `1` with `2`, `3`, `4`, etc., for other patients)
  - Password: `P4ssword1@`

**Note**: New users are created with the default password `P4ssword1@`. They are required to change their password upon
first login.

---

## STEPS TO TRY APPLICATION

See the file PDF uploaded 

## Architecture Overview

During the initial discussion, we selected fault tolerance, reliability, and testability as our key characteristics. Also we decided we still should pay some attention to elasticity and evolutionary. Deployability, scalability, modularity, and perfomance were decided to be of the lesser importance. 

Neverthenless, we have chosed to utilise the service-based architecture even if it provides the lower elasticity. To compensate for that the system is divided into several subsystems, each of them acting as a separate service. Similarly to the microservices architecture, it is possible to run a service independentely of other services if necessary. Thus a fault in one service does not lead to the shutdown of the whole system, meaning it provides higher fault tolerance. The only bottleneck is the singular database. It is required to maintain a consistency in data between different services, so it was decided to be a necessary risk. 

An API service serves as an API layer between the main components and the user interface, in our case the client component. 

The services on the inside follow the layered monolithic architecture.




