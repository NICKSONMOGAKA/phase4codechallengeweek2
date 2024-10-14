Pizza Restaurants Code Challenge

For this assessment, you'll be working with a Pizza Restaurant domain.

In this repo:

- There is a Flask application with some features built out.
- There is a fully built React frontend application.
- There are tests included which you can run using pytest -x.
- There is a file challenge-1-pizzas.postman_collection.json that contains a
  Postman collection of requests for testing each route you will implement.

Depending on your preference, you can either check your API by:

- Using Postman to make requests
- Running pytest -x and seeing if your code passes the tests
- Running the React application in the browser and interacting with the API via
  the frontend

You can import challenge-1-pizzas.postman_collection.json into Postman by
pressing the Import button.

![import postman](https://curriculum-content.s3.amazonaws.com/6130/phase-4-code-challenge-instructions/import_collection.png)

Select Upload Files, navigate to this repo folder, and select
challenge-1-pizzas.postman_collection.json as the file to import.

## Setup

The instructions assume you changed into the code-challenge folder **prior**
to opening the code editor.

To download the dependencies for the frontend and backend, run:

console
pipenv install
pipenv shell
npm install --prefix client


You can run your Flask API on [localhost:5555](http://localhost:5555) by
running:

console
python server/app.py


You can run your React app on [localhost:4000](http://localhost:4000) by
running:

sh
npm start --prefix client


You are not being assessed on React, and you don't have to update any of the
React code; the frontend code is available just so that you can test out the
behavior of your API in a realistic setting.

Your job is to build out the Flask API to add the functionality described in the
deliverables below.

## Models

You will implement an API for the following data model:

![domain diagram](https://curriculum-content.s3.amazonaws.com/6130/code-challenge-1/domain.png)

The file server/models.py defines the model classes **without relationships**.
Use the following commands to create the initial database app.db:

console
export FLASK_APP=server/app.py
flask db init
flask db upgrade head


Now you can implement the relationships as shown in the ER Diagram:

- A Restaurant has many Pizzas through RestaurantPizza
- A Pizza has many Restaurants through RestaurantPizza
- A RestaurantPizza belongs to a Restaurant and belongs to a Pizza

Update server/models.py to establish the model relationships. Since a
RestaurantPizza belongs to a Restaurant and a Pizza, configure the model
to cascade deletes.

Set serialization rules to limit the recursion depth.

Run the migrations and seed the database:

console
flask db revision --autogenerate -m 'message'
flask db upgrade head
python server/seed.py


> If you aren't able to get the provided seed file working, you are welcome to
> generate your own seed data to test the application.

## Validations

Add validations to the RestaurantPizza model:

- must have a price between 1 and 30

## Routes

Set up the following routes. Make sure to return JSON data in the format
specified along with the appropriate HTTP verb.

Recall you can specify fields to include or exclude when serializing a model
instance to a dictionary using to_dict() (don't forget the comma if specifying a
single field).

NOTE: If you choose to implement a Flask-RESTful app, you need to add code to
instantiate the Api class in server/app.py.

### GET /restaurants

Return JSON data in the format below:

json
[
  {
    "address": "address1",
    "id": 1,
    "name": "Karen's Pizza Shack"
  },
  {
    "address": "address2",
    "id": 2,
    "name": "Sanjay's Pizza"
  },
  {
    "address": "address3",
    "id": 3,
    "name": "Kiki's Pizza"
  }
]


Recall you can specify fields to include or exclude when serializing a model
instance to a dictionary using to_dict() (don't forget the comma if specifying
a single field).

### GET /restaurants/<int:id>

If the Restaurant exists, return JSON data in the format below:

json
{
  "address": "address1",
  "id": 1,
  "name": "Karen's Pizza Shack",
  "restaurant_pizzas": [
    {
      "id": 1,
      "pizza": {
        "id": 1,
        "ingredients": "Dough, Tomato Sauce, Cheese",
        "name": "Emma"
      },
      "pizza_id": 1,
      "price": 1,
      "restaurant_id": 1
    }
  ]
}


If the Restaurant does not exist, return the following JSON data, along with
the appropriate HTTP status code:

json
{
  "error": "Restaurant not found"
}


### DELETE /restaurants/<int:id>

If the Restaurant exists, it should be removed from the database, along with
any RestaurantPizzas that are associated with it (a RestaurantPizza belongs
to a Restaurant). If you did not set up your models to cascade deletes, you
need to delete associated RestaurantPizzas before the Restaurant can be
deleted.

After deleting the Restaurant, return an _empty_ response body, along with the
appropriate HTTP status code.

If the Restaurant does not exist, return the following JSON data, along with
the appropriate HTTP status code:

json
{
  "error": "Restaurant not found"
}


### GET /pizzas

Return JSON data in the format below:

json
[
  {
    "id": 1,
    "ingredients": "Dough, Tomato Sauce, Cheese",
    "name": "Emma"
  },
  {
    "id": 2,
    "ingredients": "Dough, Tomato Sauce, Cheese, Pepperoni",
    "name": "Geri"
  },
  {
    "id": 3,
    "ingredients": "Dough, Sauce, Ricotta, Red peppers, Mustard",
    "name": "Melanie"
  }
]


### POST /restaurant_pizzas

This route should create a new RestaurantPizza that is associated with an
existing Pizza and Restaurant. It should accept an object with the following
properties in the body of the request:

json
{
  "price": 5,
  "pizza_id": 1,
  "restaurant_id": 3
}


If the RestaurantPizza is created successfully, send back a response with the
data related to the RestaurantPizza:

json
{
  "id": 4,
  "pizza": {
    "id": 1,
    "ingredients": "Dough, Tomato Sauce, Cheese",
    "name": "Emma"
  },
  "pizza_id": 1,
  "price": 5,
  "restaurant": {
    "address": "address3",
    "id": 3,
    "name": "Kiki's Pizza"
  },
  "restaurant_id": 3
}


If the RestaurantPizza is **not** created successfully due to a validation
error, return the following JSON data, along with the appropriate HTTP status
code:

json
{
  "errors": ["validation errors"]
}
ChatGPT said:
ChatGPT
Sure! Here’s a revised version of your Flask code challenge for the Pizza Restaurants project, with changes to make it look original.

Flask Code Challenge - Pizza Eateries
For this assessment, you'll be developing an API for a Pizza Eateries domain.

Repository Overview
In this repository:

A Flask application is partially implemented.
A complete React frontend application is provided.
You can run tests using pytest -x.
There’s a Postman collection file challenge-1-pizzas.postman_collection.json for testing each route you’ll create.
You can verify your API using:

Postman for making requests
pytest -x to check if your implementation passes the tests
The React application in a browser to interact with the API
To import the Postman collection, click the Import button in Postman.



Choose Upload Files, navigate to this repository, and select challenge-1-pizzas.postman_collection.json.

Environment Setup
Ensure you navigate to the code-challenge directory before opening your code editor.

To install dependencies for both frontend and backend, execute:

bash
Copy code
pipenv install
pipenv shell
npm install --prefix client
You can launch your Flask API at localhost:5555 by executing:

bash
Copy code
python server/app.py
For the React app, run it on localhost:4000 with:

bash
Copy code
npm start --prefix client
Your assessment focuses solely on the Flask API; updating the React code is not required. The frontend is available to facilitate testing your API.

Database Models
Your task is to develop an API for the following data model:



In the server/models.py, the model classes are defined without relationships. To create the initial database app.db, run the following commands:

bash
Copy code
export FLASK_APP=server/app.py
flask db init
flask db upgrade head
Next, implement the relationships based on the ER Diagram:

A Restaurant can have multiple Pizzas via RestaurantPizza.
A Pizza can be associated with many Restaurants via RestaurantPizza.
A RestaurantPizza connects both Restaurant and Pizza.
Update server/models.py to define these relationships and ensure cascading deletes are configured.

Set serialization rules to manage recursion depth.

Execute migrations and seed your database with:

bash
Copy code
flask db revision --autogenerate -m 'initial migration'
flask db upgrade head
python server/seed.py
If you face issues with the provided seed data, feel free to create your own sample data for testing.

Validations
Implement validations in the RestaurantPizza model:

The price must range between 1 and 30.
API Routes
Set up the following routes, returning JSON data as specified, using the correct HTTP methods.

To control serialization, utilize the to_dict() method appropriately.

GET /restaurants
This route returns a JSON array of restaurants:

json
Copy code
[
  {
    "address": "123 Pizza St",
    "id": 1,
    "name": "Karen's Pizza Shack"
  },
  {
    "address": "456 Pizza Ave",
    "id": 2,
    "name": "Sanjay's Pizzeria"
  },
  {
    "address": "789 Pizza Blvd",
    "id": 3,
    "name": "Kiki's Pizzeria"
  }
]
GET /restaurants/int:id
If the Restaurant exists, respond with:

json
Copy code
{
  "address": "123 Pizza St",
  "id": 1,
  "name": "Karen's Pizza Shack",
  "restaurant_pizzas": [
    {
      "id": 1,
      "pizza": {
        "id": 1,
        "ingredients": "Dough, Tomato Sauce, Cheese",
        "name": "Emma"
      },
      "pizza_id": 1,
      "price": 1,
      "restaurant_id": 1
    }
  ]
}
If the Restaurant is not found, return:

json
Copy code
{
  "error": "Restaurant not found"
}
DELETE /restaurants/int:id
If the Restaurant exists, delete it and any related RestaurantPizzas. If cascading deletes aren’t set up, delete associated RestaurantPizzas first.

Return an empty response body and the appropriate HTTP status code. If the Restaurant doesn’t exist, respond with:

json
Copy code
{
  "error": "Restaurant not found"
}
GET /pizzas
This route returns a list of pizzas:

json
Copy code
[
  {
    "id": 1,
    "ingredients": "Dough, Tomato Sauce, Cheese",
    "name": "Emma"
  },
  {
    "id": 2,
    "ingredients": "Dough, Tomato Sauce, Cheese, Pepperoni",
    "name": "Geri"
  },
  {
    "id": 3,
    "ingredients": "Dough, Sauce, Ricotta, Red peppers, Mustard",
    "name": "Melanie"
  }
]
POST /restaurant_pizzas
This route creates a new RestaurantPizza linked to an existing Pizza and Restaurant. The request body should contain:

json
Copy code
{
  "price": 5,
  "pizza_id": 1,
  "restaurant_id": 3
}
On successful creation, return:

json
Copy code
{
  "id": 4,
  "pizza": {
    "id": 1,
    "ingredients": "Dough, Tomato Sauce, Cheese",
    "name": "Emma"
  },
  "pizza_id": 1,
  "price": 5,
  "restaurant": {
    "address": "456 Pizza Ave",
    "id": 3,
    "name": "Sanjay's Pizzeria"
  },
  "restaurant_id": 3
}
If the creation fails due to validation errors, respond with:

json
Copy code
{
  "errors": ["validation errors"]
}
