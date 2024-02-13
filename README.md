# Local Keycloak   

## Start Keycloak services locally

Docker service required to launch Keycloak and required database.
There are Keycloak and database already predefined in `compose.yaml` file.

To start service you should run:
1. Build docker containers (only first time)
```
docker compose build
```

2. Start up and run all containers
```
docker compose up
```

Now Keycloak ready for managing. 
Got to administration console
https://localhost:8443/admin/master/console/

User predefined admin credentials:
- username: `admin`
- password: `change_me`

## Keycloak configuration  

At startup, Keycloak automatically imports the prepared DNA realm from here `./realms`

There are already 2 clients pre-installed in DNA realm, for the frontend and for the backend
https://localhost:8443/admin/master/console/#/dna/clients
- `local-module-fronted` Used for to authenticate user account vua username and password
- `local-module-service` User for authenticate backend service via client secret `Lov2Koc3X17wHx6L1c16OuLY5EVsjnqY`

### Add user account
To create first user account in predefined `DNA` realm you should:
1. go to admin in user section  
https://localhost:8443/admin/master/console/#/dna/users
2. Press `Add user` button to create new user
3. Open user account details and got to `Credentials` section to set password
4. Press `Set passsword` button and set user password. Do not forget switch off `Temporary` toggle to make password permanent.

Now you ready to get 

## Get access tokens 

You can use Postman collection file `postman_collection.json` with predefined requests for corresponding clients.   

Or use CURL request (replace <USERNAME> and <USER_PASSWORD> for tour user account credentials):
- `local-module-fronted`
```
curl --location 'https://localhost:8443/realms/dna/protocol/openid-connect/token' \
  --header 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'client_id=local-module-frontend' \
  --data-urlencode 'grant_type=password' \
  --data-urlencode 'username=<USERNAME>' \
  --data-urlencode 'password=<USER_PASSWORD>'
```
- `local-module-service`
```
curl --location 'https://localhost:8443/realms/dna/protocol/openid-connect/token' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--data-urlencode 'client_id=local-module-service' \
--data-urlencode 'client_secret=Lov2Koc3X17wHx6L1c16OuLY5EVsjnqY' \
--data-urlencode 'grant_type=client_credentials'
```