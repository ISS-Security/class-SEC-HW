# Authorization and Architecture

For this short assignment, you will simple use a memcache server to cache requests

## A. Memcache
Please refer to work you did for "Authorization and Architecture" assignment, part B
### 1. Register Credit Card application on Memcachier
- Create cache on Memcachier for your credit card service
  - Make a development cache
  - Make another production cache
- Store Memcachier credentials (server, username, password) in `config_env.rb`
### 2. Memcache user's index of credit cards in API service
- Modify the index route for your API
  - (From previous assignment) Recall from an earlier assignment that you made a `GET '/api/v1/credit_card?user_id=#'` route
  - Make sure this route checks the `user_id` is the same as the one in the JWT authentication token it receives
  - This time, make sure this route only returns the last 4 digits of credit cards (not full credit card)
- Memcache on index route of API service
  - Whenver the API's index route is called, store the results on Memcachier
  - Use `user_id` as the memcache key
  - Store JSON of user's credit cards (last 4 digits only) on Memcachier
  - Return index from API as normal
- Memcache on creation of new credit card
  - Whenever the API's create new card route is called, store new index for user on Memcachier
  - Try to reuse a method to create and store user's index
### 3. Check Memcache from Web App
- Move your Web App's index view to the home page
  - (From previous assignment) Make sure you have a view to show all credit cards for a user
  - Move the contents of this view to the home page
  - Logged in users should see all their credit cards on main home page
- Check Memcache for index view
  - Before calling the API for user's index of credit cards, check Memcachier service
- Does your home page (index) seem to load a bit faster now?
### 5. Validate and Deploy
- Deploy your modified Web API to Heroku
- Deploy both modified Web App to Heroku
### 6. Submission
  - Submit the URLs of both repos on Canvas
