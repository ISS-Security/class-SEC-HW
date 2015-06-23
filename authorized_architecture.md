# Authorization and Architecture

In these assignments, we will start bringing tying up all the different parts of our credit card application's security architecture.

## A. Secure but Separate

We will start this phase of our application development by adding TLS security and splitting our app!

### 1. User-Service Confidentiality via TLS
- Use the `rack-ssl-enforcer` gem to enforce an SSL connection to your user-facing application
- Configure the `Rack::SslEnforcer` before configuring `Rack::Session::Cookie`
  - Remember to set the session secret right after configuring `Rack::SslEnforcer`
  - Use the message key (symmetric) that you've used before as your session secret
  - Reuse the same session secret when configuring `Rack::Session::Cookie`

### 2. Split Into Two Services
- Split your credit card service into two services based on different data models
  - Copy everything relating to the credit card API into a new repo
    - This web API will be a Sinatra app that is similar to what we had before we started adding `User` authentication logic and forms
    - Copy your CreditCard model and migrate it in the new repo; there should be no other model associated with this API
    - Copy your old specs into this repo and make sure all tests pass
    - Make sure there are no views associated with your CreditCard API: it can only be accessed by RESTful HTTP requests
    - Give your API an appropriate name and URL on Heroku that developers can use to access it
    - You do not have to enforce SSL on the API for now
  - Copy your user authentication application into a new repo
    - This Sinatra application will become the front-end web app of your credit card application
    - Give this Sinatra app a Heroku name and URL that is friendly to end users (i.e., a brand name) and related to credit cards.
    - Make sure this app has its `User` model and all views
    - Make sure that registration and email confirmation works as before
    - Make sure that SSL enforcement works on this app
  - Treat the two new applications separately for now
    - You do not have to call the API from the user application just yet
    - Make sure each repo has its corresponding keys (environment variables) and models (migrated)
### 3. Deployment
- Make sure both repos are on master branch when you are ready to deploy
- Deploy both your credit card API and your credit card user application on Heroku
### 4. Submission
  - Submit the URLs of both repos on Canvas


## B. API Keys and Authorization

Recall that last time we split our service into two parts: a web app with user authentication, and a RESTful API. This time we will integrate our web app and API into a combined service architecture with authorization.

### 1. Modify your credit card API service to handle user IDs
- Add a `user_id` column to your API's database
  - See the slide titled 'API User Trail' in our class handout to see how to reset a Postgres table and migrate again
  - Note: if you configure `tux` and `hirb` gems to be available to all environments in your Gemfile, you can access your Postgres database using `heroku run tux` from the command line
- Modify two earlier routes you made for your API service:
  - POST to `/api/v1/credit_card?user_id=#` should create a new credit card with the `user_id` value (#) in its own column of the database entry
  - GET `/api/v1/credit_card?user_id=#` should only return credit cards for user with specified (#) id from credit card database

### 2. Require user authorization for web app views
Note: Ignore JWT authorization for this part (we will do it next step)
- In your credit card web app, create two new views only for logged in users:
  - create a new credit card: take required user input and create new credit card for that user
  - view all credit cards: show a table with all credit cards created by that user
  - views should talk to the API service (`HTTParty` requests) to create/retrieve credit cards
- Make sure both views require user to be logged in (see 'User Authorization' slide from class handout)
  - user id number of `@current_user` for POST and GET requests to API service.

### 3. Require JWT based authorization between web app and API services
- Create private/public RSA keypair for web app
  - store web app's private and public keys as `config_env` variables for web app
  - store web app's public key only as `config_env` variable for API
- Authorize requests to API
  - Web app should send RSA-signed JWT in `authorization` header of HTTP POST/GET requests to API
  - API should validate HTTP requests
    - Make sure JWT in `HTTP_AUTHORIZATION` header of HTTP request is RSA-signed by web app
    - Make sure that user's id number in `sub` of payload matches the `?user_id=` parameter of HTTP request
    - Return 401 error if either condition fails

### 4. Validate that API works with authorization
- Make sure that web app can be used to create and view records from API only when logged in
- Make sure that you can access API (POST/GET) using command line `curl` using correct `authorization` header

### 5. Deployment
- Make sure both repos are on master branch when you are ready to deploy
- Deploy both your credit card API and your credit card web app on Heroku

### 6. Submission
  - Submit the URLs of both repos on Canvas


## C. Single-Sign-On Using Github OAuth

### 1. Register a 'Developer Application' on Github
(refer to class notes on AOauth for week 15)
- Step A of Oauth: Register a developer application on Github
- Set your application name and server URL
  - For development purposes, you may use http://localhost:9292 as your server
  - Change the server to your Web App's heroku server for production
- Choose a callback URL for your Web App
### 2. Create a 'Login with Github' option for your Web App
- Put a 'Login with Github' link on your login view
  - Set the link to point to Github's `login/auth` URL
  - Set the `client_id` attribute of the link to what Github provided you
  - Set the `scope` attribute of the link to `user:email`
### 3. Create callback routes
- Make a `/callback` route for your Web App where Github can return users
- POST your your `client_id` and `client_secret` to Github's `login/auth` URL to get an `access_token`
- GET the user's information from Github (`login` and `email`)
### 4. Github Single-Sign-On
- If user's Github `login` doesn't exist in your database, make a new user using `login` as username and `email`
- You may put anything you want as a new password (Github user will never know it)
- Login user using their new username and email
- For this assignment, you do not have to worry about email/login collision with prior accounts
### 5. Validate and Deploy
- Make sure your can login with your Github account
- Change your Github developer application to point to your Heroku Web App before deployment
- Deploy both your new credit card Web App (no changes to API)
### 6. Submission
  - Submit the URLs of both repos on Canvas
