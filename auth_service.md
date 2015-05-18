# Authentication Service

In these assignments, you will be creating enabling authentication for users using various approaches.

## A. Basic User Authentication

We will begin to create our first user authentication features.  Let's start with a basic login feature!

### 1. Create a User model
- Make a `User` class that is an `ActiveRecord` and create a migration for it. Attributes should include `username`, `password`, `email`, `fullname`, `address` (single line address for now), `dob` (date-of-birth)
- Hashing and Encryption
  - Make sure that password is salted and hashed using scrypt algorithm
  - Make sure the full name, address, and dob are encrypted, in case our database is compromised.
  - Can you DRY (don't-repeat-yourself) your code by creating an encrypt/decrypt method that you can reuse for the gett/setter methods of the encrypted attributes?
- Validations
  - Validate that all fields are given.
  - Validate the format of the `email` however you like.
- Authentication: create methods for `password_matches?` and `authenticate` as we saw in class.

### 2. Simple Authentication
- Create views for for:
 - home page (describes your API)
 - registration
 - login
- Create a registration page
  - Users can create new accounts by entering all attributes of the `User` model.
  - Redirect them to the login page when done registering.
- Create a login feature that allows users to login using their username and password.
  - Hold their user `id` in a session variable.
  - Authenticate users *before* processing any route they go to.
- Create a login bar in the layout for all pages
  - Login bar should show registration/login/logout links *as appropriate*
  - Allow non-logged in users to register/login (but not logout).
  - Allow logged in users to logout (but not register/login).

### 3. Deployment
- Merge your work to `master` branch
- Deploy your working project to Heroku

### 4. Submission
- Submit the URLs of your Github repo (make sure your Heroku URL is in the README)


## B. Token Based Authentication

We will now transform our authentication service to be fully stateless on the server side by not saving any information about login or registration status

### 1. Use flash messages for all errors and notices
- Create flash message styles in CSS
- Add flash message to layout (perhaps as a partial view)
- Make sure relevant user interactions produce a flash message (notice/error)

### 2. JWT Authentication
- Use cookies to store JSON Web Token (JWT) of logged in user's `id`
  - On login, send back JWT session cookie with HMAC (SHA256) signature
  - Upon return by logged in user, validate HMAC of JWT cookie
- Ensure that restarting server does not disrupt user's session

### 3. Use email verification to complete registration process
- When new user registers, send encoded JWT in email
  - Create encrypted JWT Token
    - first create JWT with payload and signature
    - then encrypt token into URL safe string
  - Send email verification with token
    - Use Pony gem and SendGrid service to send email
    - Send token as part of link back to your service
      - use `request.base_url` to get name of your server
      - send token as a parameter in URL (e.g., `register?token=...`
  - Do not store any temporary user state on server
- Create user account when email link is clicked
  - Make sure that token parameter is valid
  - Make sure that user is created appropriately

NOTE: you may send any (encrypted) information you want in the email token, including passwords. However, if you do not send all the information, you must get it from the user when they return by clicking on the emailed URL. Do NOT store any temporary data in your database or as session pool variables.

### 4. Deployment
- Merge your work to `master` branch
- Deploy your working project to Heroku

### 5. Submission
- Submit the URLs of your Github repo (make sure your Heroku URL is in the README)
