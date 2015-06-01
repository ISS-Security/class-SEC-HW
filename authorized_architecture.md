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
