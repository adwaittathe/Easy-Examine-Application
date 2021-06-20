# Easy-Examine-Application

Imagine a case where students are expected to create a poster and present it during a poster session. During the poster session there are examiners that will be required to visit each poster and grade the poster based on a simple list of questions.\
API can be used to easily authenticate users for mobile applications.The server is hosted on AWS EC2.

[Click here](https://www.youtube.com/watch?v=lb_IaRQpGNo) to watch the demonstration of the application.

## Table of content
* [Get Started](#get-started)
    * [Login](#login)
    * [Get Judge Profile](#get-judge-profile)
    * [Get Team By Id](#update-user)
    * [Send Score For Team](#send-score-for-team)
    * [Get Team Scores](#get-team-scores)
* [Database Schema](#database-schema)
* [References](#references)

## Get Started
User-Authentication-API can be used to login, signup user and get user specific information.
> In the examples on this page, you would replace [TOKEN] with the token returned by this API after user SignUp/Login.
### Login 
> http://ec2-54-86-229-201.compute-1.amazonaws.com/api/user/login
* Method - Post 
* Request Payload - 
   ```
   {
     "email" : "chandler@gmail.com",
     "password" : "123456"
   }
   ```
* Response Payload- 
   ``` 
   {
       "status": 200,
       "id": "5d8b97164dfcab1a47b215ed",
       "token":"[TOKEN]",
       "name": "Chandler Bing",
       "email": "chandler@gmail.com"
   }
   ```

* Status codes -
   * 200 - success
   * 400 - Invalid email/password



### Get Judge Profile 
> http://ec2-54-86-229-201.compute-1.amazonaws.com/api/scoping/getJudgeProfile
* Method - POST
* Request Payload(Header) -
```
“token” :[TOKEN]
```
* Response Payload- 
```
{
    "status": 200,
    "userId": "5dcb237b66978d3752729a04",
    "firstName": "Chandler",
    "lastName": "Bing",
    "email": "chandler1@gmail.com",
    "gender": "Male",
    "contactNo": "7047059630",
    "age": "25",
    "createdAt": "2019-11-12T21:26:19.118Z"
}
```

* Status codes - 
   * 200 - success
   * 400 - Access denied. Token not provided
   * 401 - Invalid token


### Get Team By ID 
> http://ec2-54-86-229-201.compute-1.amazonaws.com/api/scoping/getTeambyId
* Method - POST
* Request Payload(Header) -
```
“token” :[TOKEN]
```
* Request Payload(Body) -
```
{
  "teamId" : "2"
}
```

* Response Payload- 
```
{
    "status": 200,
    "teamId": "5dcbb9b9d9bcca51e2c5add7",
    "name": "3idiots",
    "score": {
        "5dc7481ce1cf0d25bec3a0a0": "20",
        "5dcb8ee155378644c2be8ddf": "90"
    }
}
```

* Status codes - 
   * 200 - success
   * 400 - Access denied. Token not provided
   * 401 - Invalid token


### Send Score for Team 
> http://ec2-54-86-229-201.compute-1.amazonaws.com/api/scoping/sendScoreForTeam
* Method - POST
* Request Payload(Header) -
```
“token” :[TOKEN]
```
* Request Payload(Body) -
```
{
    "teamId":"4",
    "score": "200"
}
```

* Response Payload- 
```
{
    "status": 200,
    "message": "Team Score updated successfully"
}
```

* Status codes - 
   * 200 - success
   * 400 - Access denied. Token not provided
   * 401 - Invalid token


### Get Team Scores 
> http://ec2-54-86-229-201.compute-1.amazonaws.com/api/scoping/getTeamScore
* Method - GET
* Request Payload(Header) -
```
“token” :[TOKEN]
```

* Response Payload- 
```
{
    "status": 200,
    "team": [
        {
            "_id": "5dcbb9b9d9bcca51e2c5add9",
            "teamId": "4",
            "name": "Dragons",
            "finalScore": "119",
            "__v": 0,
            "score": {
                "5dc7481ce1cf0d25bec3a0a0": "38",
                "5dcb8ee155378644c2be8ddf": "200"
            }
        },
        {
            "_id": "5dcbb9b9d9bcca51e2c5add7",
            "teamId": "2",
            "name": "3idiots",
            "finalScore": "55",
            "__v": 0,
            "score": {
                "5dc7481ce1cf0d25bec3a0a0": "20",
                "5dcb8ee155378644c2be8ddf": "90"
            }
        },
        {
            "_id": "5dcbb9b9d9bcca51e2c5add8",
            "teamId": "3",
            "name": "JustDoIt",
            "finalScore": "50",
            "__v": 0,
            "score": {
                "5dc7481ce1cf0d25bec3a0a0": "50"
            }
        },
        {
            "_id": "5dcbb9b9d9bcca51e2c5add6",
            "teamId": "1",
            "name": "TechGeeks",
            "finalScore": "25",
            "__v": 0,
            "score": {
                "5dc7481ce1cf0d25bec3a0a0": "25"
            }
        }
    ]
}
```

* Status codes - 
   * 200 - success
   * 400 - Access denied. Token not provided
   * 401 - Invalid token


## Database Schema

MongoDB Database 

* User Schema
```
const userSchema = new mongoose.Schema({
    firstName : {
        type : String,
        required : true
    },
    lastName : {
        type : String,
        required : true
    },
    gender: {
        type :String,
        required : true
    },
    contactNo: {
        type:String,
        min : 10,
        max : 10,
        required: true
    },
    age : {
        type:String,
        min : 1,
        max : 3,
        required:true
    },
    email : {
        type : String,
        required : true,
        max : 255
    },
    customerId :{
        type : String
    },
    password : {
        type : String,
        min : 6,
        required : true
    },
    createdAt : {
        type : Date,
         default : Date.now
    }
});
```

* Team Schema
```
const teamSchema = new mongoose.Schema({
    teamId : {
        type : String
    },
    name : {
        type : String
    },
    score : {
        type : Object
    },
    finalScore : {
        type : String
    }
});
```

## References
- [JWT](https://jwt.io) - Decode, Verify, and generate JWT
- [Node.js](https://nodejs.org/en/) - JavaScript runtime
- [Express](https://expressjs.com) - Node.js web application framework
- [@hapi/joi](https://www.npmjs.com/package/@hapi/joi) - Data validation
