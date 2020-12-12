---
title: "Connecting php symfony with postgres"
date: 2020-12-11T19:21:39+05:30
draft: false
---

### Steps
- Enable ORM(doctrine) in symfony
- Configure symfony to connect with postgres
- Troubleshoot Errors

### Enable Doctrine ORM 
- Add doctrine package to our project
	```
		composer require doctrine/doctrine-bundle
    ```

###  Configure symfony to connect with postgres.
- Open config/packages/doctrine.yaml
	```
	doctrine:
    dbal:
        url: '%env(resolve:DATABASE_URL)%'
        driver: 'pdo_pgsql'
        charset: utf8
    ```
- Edit .env file and provide your connection string	
	```
	DATABASE_URL="postgresql://<pg_username>:<pg_password>@127.0.0.1:5432/<database_name>?serverVersion=<pg_serverVersion>&charset=utf8"
    ```

### Testing the connection 
- Step 1: Creating a controller method.
	```
	use Doctrine\DBAL\Driver\Connection;
	public function home(Connection $connection): Response {
        $employees = $connection->fetchAll("select * from employees");
        return $this->render("Home/home.html.twig", [
            'employees' => $employees
        ]);
    }
    ```
- Step 2: Creating a view to show the data from controller. Create in templates/Home/home.html.twig.
	```
	{% block body %}
	<h3> Employees </h3>
	<hr />
	<ul>
    {% for emp in employees %}
        <li>{{ emp.username }}</li>
    {% endfor %}
	</ul>
	{% endblock %}
    ```
- Step 3:  Result
    ```
        Employees
            - user
            - admin
            - user3
            - user4
    ```
