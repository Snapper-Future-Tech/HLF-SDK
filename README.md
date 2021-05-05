# HLF_SDK_lab
sdk for accessing blockchain network on server

# Edit the /etc/hosts file on your local machine with the Server IP address and the Containers running on it.
# For Example

20.198.1.28     ca.org1.example.com
20.198.1.28     ca.org2.example.com
20.198.1.28     peer0.org1.example.com
20.198.1.28     peer1.org1.example.com
20.198.1.28     peer0.org2.example.com
20.198.1.28     peer1.org2.example.com
20.198.1.28     orderer.example.com
20.198.1.28     orderer2.example.com
20.198.1.28     orderer3.example.com


### Replace the IP address in Constant.json

navigate to HLFSDK/api2.0 folder

cd config

sudo nano constant.json

replace the ip address with ip address provided in IBI LAB email.

Change the Port number as per provided in the same email

Save the changes.

cd ..

### Modify Invoke.js and Query.js

navigate to app folder

replace your smart contracts function names in invoke.js and query.js.

save the changes.

cd ..

### start the Node Application

node app.js



## Sample REST APIs Requests

### Login Request

* Register and enroll new users in Organization - **Org1**:

`curl -s -X POST http://20.193.230.28:{External Port}/users -H "content-type: application/x-www-form-urlencoded" -d 'username=Jim&orgName=Org1'`

**OUTPUT:**

```
{
  "success": true,
  "secret": "RaxhMgevgJcm",
  "message": "Jim enrolled Successfully",
  "token": "<put JSON Web Token here>"
}
```

The response contains the success/failure status, an **enrollment Secret** and a **JSON Web Token (JWT)** that is a required string in the Request Headers for subsequent requests.

### Invoke request

This invoke request is signed by peers from both orgs, *org1* & *org2*.
```
curl -s -X POST \
  http://20.193.230.28:{External Port}/channels/mychannel/chaincodes/{Chaincode Name} \
  -H "authorization: Bearer <put JSON Web Token here>" \
  -H "content-type: application/json" \
  -d '{
	"peers": ["peer0.org1.example.com","peer0.org2.example.com"],
	"fcn":"{Function Name}",
	"args":["a","b","10"]
}'
```
**NOTE:** Ensure that you save the Transaction ID from the response in order to pass this string in the subsequent query transactions.




### Chaincode Query

```
curl -s -X GET \
http://20.193.230.28:{External Port}/channels/mychannel/chaincodes/{Chaincode Name}?args=["{Key}"]&peer=peer0.org1.example.com&fcn={Chaincode query function}



