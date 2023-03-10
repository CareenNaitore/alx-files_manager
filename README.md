0. Redis utils

Inside the folder utils, create a file redis.js that contains the class RedisClient.

RedisClient should have:

the constructor that creates a client to Redis:
any error of the redis client must be displayed in the console (you should use on('error') of the redis client)
a function isAlive that returns true when the connection to Redis is a success otherwise, false
an asynchronous function get that takes a string key as argument and returns the Redis value stored for this key
an asynchronous function set that takes a string key, a value and a duration in second as arguments to store it in Redis (with an expiration set by the duration argument)
an asynchronous function del that takes a string key as argument and remove the value in Redis for this key
After the class definition, create and export an instance of RedisClient called redisClient.

1. MongoDB utils

Inside the folder utils, create a file db.js that contains the class DBClient.

DBClient should have:

the constructor that creates a client to MongoDB:
host: from the environment variable DB_HOST or default: localhost
port: from the environment variable DB_PORT or default: 27017
database: from the environment variable DB_DATABASE or default: files_manager
a function isAlive that returns true when the connection to MongoDB is a success otherwise, false
an asynchronous function nbUsers that returns the number of documents in the collection users
an asynchronous function nbFiles that returns the number of documents in the collection files
After the class definition, create and export an instance of DBClient called dbClient.

2. First API
mandatory
Inside server.js, create the Express server:

it should listen on the port set by the environment variable PORT or by default 5000
it should load all routes from the file routes/index.js
Inside the folder routes, create a file index.js that contains all endpoints of our API:

GET /status => AppController.getStatus
GET /stats => AppController.getStats
Inside the folder controllers, create a file AppController.js that contains the definition of the 2 endpoints:

GET /status should return if Redis is alive and if the DB is alive too by using the 2 utils created previously: { "redis": true, "db": true } with a status code 200
GET /stats should return the number of users and files in DB: { "users": 12, "files": 1231 } with a status code 200
users collection must be used for counting all users
files collection must be used for counting all files

3. Create a new user

Now that we have a simple API, it???s time to add users to our database.

In the file routes/index.js, add a new endpoint:

POST /users => UsersController.postNew
Inside controllers, add a file UsersController.js that contains the new endpoint:

POST /users should create a new user in DB:

To create a user, you must specify an email and a password
If the email is missing, return an error Missing email with a status code 400
If the password is missing, return an error Missing password with a status code 400
If the email already exists in DB, return an error Already exist with a status code 400
The password must be stored after being hashed in SHA1
The endpoint is returning the new user with only the email and the id (auto generated by MongoDB) with a status code 201
The new user must be saved in the collection users:
email: same as the value received
password: SHA1 value of the value received

4. Authenticate a user

In the file routes/index.js, add 3 new endpoints:

GET /connect => AuthController.getConnect
GET /disconnect => AuthController.getDisconnect
GET /users/me => UserController.getMe
Inside controllers, add a file AuthController.js that contains new endpoints:

GET /connect should sign-in the user by generating a new authentication token:

By using the header Authorization and the technique of the Basic auth (Base64 of the <email>:<password>), find the user associate to this email and with this password (reminder: we are storing the SHA1 of the password)
If no user has been found, return an error Unauthorized with a status code 401
Otherwise:
Generate a random string (using uuidv4) as token
Create a key: auth_<token>

5. First file

In the file routes/index.js, add a new endpoint

6. Get and list file

In the file routes/index.js, add 2 new endpoints:

7. File publish/unpublish

In the file routes/index.js, add 2 new endpoints:

8. File data

In the file routes/index.js, add one new endpoint:

9. Image Thumbnails

Update the endpoint POST /files endpoint to start a background processing for generating thumbnails for a file of type image:

10. Tests!

Of course, a strong and stable project can not be good without tests.

11. New user - welcome email

Update the endpoint POST /users endpoint to start a background processing for sending a ???Welcome email??? to the user:

