# Credit Card API

In this assignment, you will be creating a web service that serves as an online API to the  credit card features you wrote earlier.

## A. Our First Service
### 1. File/Folder Structure
Create a new repo with the following structure, described further below:
```
├── Gemfile
├── Gemfile.lock
├── Procfile
├── README.md
├── app.rb
├── config.ru
├── lib/
├── spec/
```

- `lib/`: Put your earlier CreditCard and crypto code files in here -- do not change any files!
- `spec/`: put your old specs in here, but they must now work with your code in `lib/`
- `Gemfile`: list the gems your new service will need
- `Gemfile.lock`: autocreated by `bundle install`
- `Procfile`: what `rackup` will launch with the configurations in `config.ru`
- `config.ru`: which file and class will the service run?
- `README.md`: start a meaningful README describing service features, and your deployed Heroku application
- `app.rb`: your Sinatra service should be coded in here

### 2. CreditCard and Crypto code
Move your old project `*.rb` files in to the `lib/` folder of this repo, and your old specs into `spec/`. You must make sure your specs still run from your app's root directory. Try to see if you can reuse your `CreditCard` code without modifying it.

### 3. Sinatra app: `app.rb`

Create a class called `CreditCardAPI` inherited from `Sinatra::Base`. Your service should support two routes (URLs):
- `/`: the root route should return a message that your service is running.
- `/api/v1/credit_card/validate`: this route should validate credit card numbers using the Luhn algorithm. Simply create CreditCard object using your previous code (now in `lib/`) and validate it. Examples of requests it must service include:
  - `http://127.0.0.1:9393/api/v1/credit_card/validate?card_number=4024097178888052`
  This number should *not* get validated, and your service should return the following json:
  `{"card":"4024097178888052","validated":false}`
  - `http://127.0.0.1:9393/api/v1/credit_card/validate?card_number=4916603231464963`
  This number should get validated, and your route should return: `{"card":"4916603231464963","validated":true}`

### 4. Ship it!!!

We don't like it when our work isn't deployed. Once your two routes work, deploy it to Heroku. Name your deployed application appropriately (more on this later).

### 5. Submission

Submit your Github repo. But put a link in your `README.md` to your Heroku application so we can find it and try it out!


## B. Creating a Database

Important: We will not be deploying this part of your project. So please work in a git branch called `sqlite` and push that to github. Individual members should pull that branch down and branch off from there for their individual parts. We will **not** merge this with master for this part of the project. Instead, issue pull requests to `sqlite`.

### 1. Sqlite
Follow the instructions in class to create a local sqlite database called `db/dev.db`. Your table should should be called `Card` and should have all the items found in your CreditCard class. Some special things to note:

- Be very careful about using appropriate singular and plural name for credit cards in each of the three steps we discussed in class:
  - Step 1: n/a
  - Step 2:
    - the class name of your model should be `CreditCard` and the filename should be `credit_card.rb`
    - the name of the `db:create_migration` create task `create_credit_cards`
  - Step 3:
    - make sure the name of your migration file is `db/migrate/*_create_credit_cards.rb` but that the name of the migration class is `CreateCreditCards`
    - the name of the `create_table` parameter in the `#change` method should be `:credit_cards`
- When making the CreditCard model (step 2 of class instructions): we will make our earlier CreditCard object into an ActiveRecord
  - move `credit_card.rb` from `lib/` folder to a new `model/` folder
  - comment out the attributes (`name`, `owner`, `expiration_date`, and `credit_network`) -- we will define these in the migration file instead
  - comment out the `initialize` method -- ActiveRecord has its own initialization process that we don't want to interfere with
  - make the `CreditCard` class inherit from `ActiveRecord::Base`
  - note: the specs you wrote earlier will fail -- we will talk about fixing them later.

### 2. New Routes
####(1) Create a POST route that creates a new credit card on your database:

- The route should be: `POST /api/v1/credit_card`
  - e.g., `curl -X POST -d "{\"number\":\"5192234226081802\",\"expiration_date\":\"2017-04-19\",\"owner\":\"Cheng-Yu Hsu\",\"credit_network\":\"Visa\"}" http://127.0.0.1:9292/api/v1/credit_card`
- It should take a json request body with attributes `number`, `owner`, `credit_network` and `expiration_date`
- If the card number is not valid, halt with [HTTP status code](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes#2xx_Success) 400
- If succesful, it should return HTTP status `201` (you can simply use `status 201` to set a return status) for resource created, or else halt with `410` if there is a problem.

####(b) Create a GET index route that returns all the credit cards in the database
- Use your active record (CreditCard) to get `.all` the cards
- Return a json array of all cards
- Return a status of `200` if all goes well, or else `500` if there is any error

### 4. Don't Ship!

We are not proud of code that saves sensitive data in an unsecured database! Let's just make sure it works locally on your own machine. We will create a safer solution next week. Remember, keep your work in a feature branch called `sqlite` but not in master.

### 5. Submission

Submit a link to the `sqlite` branch of your Github repo.


## C. Hardening our Database

Important: Please work in a git branch called `hardened_db` and push that to github. Do not merge into master quite yet.

### 1. Resisting SQL Injection Attacks

You currently have 3 GET/POST routes that take parameters. Make them resistant to SQL injection attacks using Query Parameterization

- Do not directly pass any variables to `ActiveRecord` methods if the data came directly from user paramaters (`response.body` or `params[]`)
- Instead, use the references from our class notes to pass in query parameters
- If you cannot find relevant query parameters for any `ActiveRecord` methods, please discuss a solution on our Canvas website!

### 2. Record-Level Encryption

Our credit card numbers are currently in raw form on the database. Let's encrypt them!

- Change your migration to only store `:encrypted_number` rather than `:number`
  - also store a `:nonce`, which serves as a one-time initialization vector
  - delete `db/schema.rb` and `db/dev.db`
  - rerun `rake db:migrate`
- Add getter and setter methods for `number` to your `CreditCard` model
  - `def number=` should take a raw number, encrypt it with a key and nonce, and store it in `#encrypted_number`
  - `def number` should decrypt the `#encrypted_number` using a key and the record's appropriate nonce
  - you should get the key from `ENV` rather than hard-wiring it into your code

### 4. Don't Ship!

We will discuss setting up a better production database in class this coming week. So don't ship your code to Heroku quite yet!

### 5. Submission

Submit a link to the `hardened_db` branch of your Github repo.
