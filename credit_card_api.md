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
