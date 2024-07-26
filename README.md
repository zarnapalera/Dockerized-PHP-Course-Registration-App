
Sample Web Application Deployment with Docker
This project demonstrates how to deploy a web application using Docker containers, including creating Docker images from scratch and utilizing Docker Compose for deployment. The application is a PHP-based course registration form that interacts with a MySQL database.

Table of Contents
Project Overview
Application Features
Getting Started
Project Structure
Repository Files
Author
Project Overview
The project consists of two key components:

Creating Docker Images: We build Docker images from the ground up for both a web application and a MySQL database.
Deployment: Using Docker Compose, we deploy the application to provide a fully isolated and reproducible environment.
The web application enables users to register for courses by filling out a form. The submitted data is then stored in a MySQL database.

Application Features
This simple application includes:

A web server running Apache2 with PHP 7.4
A MySQL database to handle registration data
Docker containers to ensure isolated and consistent environments
Deployment Instructions
Before you begin, ensure you have the following prerequisites installed:

Docker
Docker Compose
To deploy the application:

Clone the repository to your local machine.

Navigate to the project directory.

Build and launch the Docker containers using Docker Compose:

bash
Copy code
docker-compose up --build
Access the web application at http://localhost in your web browser.

Project Structure
bash
Copy code
.
├── docker-compose.yml
├── Dockerfile
├── index.php
└── README.md
Repository Files
Dockerfile

This file sets up the environment for the web server:

Dockerfile
Copy code
FROM php:7.4-apache

WORKDIR /var/www/html

COPY . /var/www/html/

RUN docker-php-ext-install mysqli

EXPOSE 80
docker-compose.yml

This configuration file defines the services for the application:

yaml
Copy code
version: '3.1'

services:
  web:
    build: .
    ports:
      - "80:80"
    depends_on:
      - db

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: example
      MYSQL_DATABASE: myDB
    volumes:
      - db_data:/var/lib/mysql

volumes:
  db_data:
index.php

The PHP script for form handling and database interaction:

php
Copy code
<?php
$servername = "db";
$username = "root";
$password = "example";
$dbname = "myDB";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER['REQUEST_METHOD'] == 'POST') {
    $name = $_POST['name'];
    $course = $_POST['course'];

    $sql = "INSERT INTO Students (name, course) VALUES ('$name', '$course')";

    if ($conn->query($sql) === TRUE) {
        echo "New record created successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}
?>

<!DOCTYPE html>
<html>
<body>

<h2>Course Registration Form</h2>
<form method="post">
  Name:<br>
  <input type="text" name="name" required>
  <br>
  Course:<br>
  <input type="text" name="course" required>
  <br><br>
  <input type="submit" value="Submit">
</form>

</body>
</html>

Author

Zarna Palera- https://github.com/zarnapalera/zarnapalera

