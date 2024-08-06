ASSIGNMENT-B    Backend

MongoDB Schema Document
1. User Schema
Schema:

{
  "_id": "ObjectId",
  "username": "string",
  "email": "string",
  "passwordHash": "string",
  "fullName": "string",
  "profilePicture": "string",
  "friends": [
    {
      "userId": "ObjectId",
      "status": "string" // 'accepted', 'pending', 'rejected'
    }
  ],
  "friendRequests": [
    {
      "fromUserId": "ObjectId",
      "status": "string" // 'pending', 'accepted', 'rejected'
    }
  ]
}

Example Document:
{
  "_id": "64b15c7e8d2e4f0d90e0f1b2",
  "username": "john_doe",
  "email": "john.doe@example.com",
  "passwordHash": "$2a$12$KIX1npOlX4YOuZqE5O2y6.GgF2Q/J3q5oc/pXitKmdJcOT0MwX3Dq",
  "fullName": "John Doe",
  "profilePicture": "https://example.com/profile/john_doe.jpg",
  "friends": [
    {
      "userId": "64b15c7e8d2e4f0d90e0f1b3",
      "status": "accepted"
    }
  ],
  "friendRequests": [
    {
      "fromUserId": "64b15c7e8d2e4f0d90e0f1b4",
      "status": "pending"
    }
  ]
}
2. Post Schema
Schema:

{
  "_id": "ObjectId",
  "userId": "ObjectId", // ID of the user who created the post
  "content": "string", // Text content of the post
  "timestamp": "date",
  "comments": [
  {
     "userId": "ObjectId",
      "content": "string",
      "timestamp": "date"
    }
  ]
}
Example Document:
{
  "_id": "64b15c7e8d2e4f0d90e0f1b5",
  "userId": "64b15c7e8d2e4f0d90e0f1b2",
  "content": "Enjoying a sunny day at the park!",
  "timestamp": "2024-08-06T14:23:45Z",
  "comments": [
    {
      "userId": "64b15c7e8d2e4f0d90e0f1b6",
      "content": "Looks amazing!",
      "timestamp": "2024-08-06T15:00:00Z"
    }
  ]
}

 *  API Design Document
1. Fetch Feed
Endpoint: GET /api/feed
Description: Fetches posts for the logged-in user.
Parameters:
userId (query parameter)

Example Request:
GET /api/feed?userId=64b15c7e8d2e4f0d90e0f1b2
Example Response:
[
  {
    "_id": "64b15c7e8d2e4f0d90e0f1b5",
    "userId": "64b15c7e8d2e4f0d90e0f1b2",
    "content": "Enjoying a sunny day at the park!",
    "timestamp": "2024-08-06T14:23:45Z",
    "comments": [
      {
        "userId": "64b15c7e8d2e4f0d90e0f1b6",
        "content": "Looks amazing!",
        "timestamp": "2024-08-06T15:00:00Z"
      }
    ]
  }
]

2. Create Post
Endpoint: POST /api/posts
Description: Create a new text-only post.
Body:

{
  "userId": "64b15c7e8d2e4f0d90e0f1b2",
  "content": "Just had a great workout!",
  "timestamp": "2024-08-06T16:00:00Z"
}

Example Response:
{
  "_id": "64b15c7e8d2e4f0d90e0f1b7",
  "userId": "64b15c7e8d2e4f0d90e0f1b2",
  "content": "Just had a great workout!",
  "timestamp": "2024-08-06T16:00:00Z",
  "comments": []
}

3. Comment on Post
Endpoint: POST /api/posts/:postId/comments
Description: Add a comment to a post.
Parameters:
postId (path parameter)
Body:
  {
  "userId": "64b15c7e8d2e4f0d90e0f1b6",
  "content": "Great job!",
  "timestamp": "2024-08-06T16:30:00Z"
}

Example Response:
{
  "_id": "64b15c7e8d2e4f0d90e0f1b7",
  "userId": "64b15c7e8d2e4f0d90e0f1b2",
  "content": "Just had a great workout!",
  "timestamp": "2024-08-06T16:00:00Z",
  "comments": [
    {
      "userId": "64b15c7e8d2e4f0d90e0f1b6",
      "content": "Great job!",
      "timestamp": "2024-08-06T16:30:00Z"
    }
  ]
}

4. Send Friend Request
Endpoint: POST /api/friend-requests
Description: Send a friend request to another user.

Example Request:
POST /api/friend-requests
{
  "fromUserId": "64b15c7e8d2e4f0d90e0f1b2",
  "toUserId": "64b15c7e8d2e4f0d90e0f1b8"
}

Example Response:
{
  "message": "Friend request sent."
}



5. Accept/Reject Friend Request
Endpoint: PUT /api/friend-requests/:requestId
Description: Accept or reject a friend request.
Parameters:
requestId (path parameter)

Example Request:
PUT /api/friend-requests/64b15c7e8d2e4f0d90e0f1b9
{
  "status": "accepted"
}
Example Response:
{
  "message": "Friend request accepted."
}


6. View Friend Requests
Endpoint: GET /api/friend-requests
Description: Get all friend requests for the logged-in user.
Parameters:
userId (query parameter)

Example Request:
GET /api/friend-requests?userId=64b15c7e8d2e4f0d90e0f1b2
Example Response:
[
  {
    "fromUserId": "64b15c7e8d2e4f0d90e0f1b8",
    "status": "pending"
  }
]









