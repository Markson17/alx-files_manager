# Files Manager - Back-End

This project is a summary of the back-end trimester, focusing on authentication, NodeJS, MongoDB, Redis, pagination, and background processing. The objective is to build a simple platform to upload and view files with the following features:

1. User authentication via a token.
2. Listing all files.
3. Uploading a new file.
4. Changing permission of a file.
5. Viewing a file.
6. Generating thumbnails for images.

You will be guided step by step for building it, but you have some freedoms of implementation, such as splitting code into more files.

## Prerequisites

Before you start, make sure you have the following set up:

- Node.js installed on your system.
- MongoDB and Redis databases set up.
- Clone the GitHub repository

## Installation

1. Clone the repository to your local machine.

   ```bash
   git clone https://github.com/Markson17/alx-files_manager.git
   ```

2. Navigate to the project directory.

   ```bash
   cd alx-files_manager
   ```

3. Install the required dependencies.

   ```bash
   npm install
   ```

## Configuration

### Redis Configuration

Inside the `utils` folder, create a file `redis.js` that contains the `RedisClient` class. Configure the connection to your Redis instance using environment variables (`DB_HOST`, `DB_PORT`, and `DB_DATABASE`).

### MongoDB Configuration

Inside the `utils` folder, create a file `db.js` that contains the `DBClient` class. Configure the connection to your MongoDB instance using environment variables (`DB_HOST`, `DB_PORT`, and `DB_DATABASE`).

### Server Configuration

In `server.js`, set the desired port for your Express server using the `PORT` environment variable.

## Usage

### Start the Express Server

```bash
npm start
```

### Start the Worker for Thumbnails Generation (in a separate terminal)

```bash
npm run start-worker
```

### API Endpoints

#### 1. Get Status

- **Endpoint**: `GET /status`
- **Description**: Check if Redis and MongoDB are alive.
- **Response**: `{ "redis": true, "db": true }`

#### 2. Get Stats

- **Endpoint**: `GET /stats`
- **Description**: Get the number of users and files in the database.
- **Response**: `{ "users": 12, "files": 1231 }`

#### 3. Create a New User

- **Endpoint**: `POST /users`
- **Description**: Create a new user in the database.
- **Request Body**:

  ```json
  {
    "email": "user@example.com",
    "password": "password123"
  }
  ```

- **Response**: `{ "id": "user_id", "email": "user@example.com" }`

#### 4. Authenticate a User

- **Endpoint**: `GET /connect`
- **Description**: Sign in a user by generating an authentication token.
- **Request Header**: `Authorization: Basic base64(email:password)`
- **Response**: `{ "token": "authentication_token" }`

#### 5. Sign Out a User

- **Endpoint**: `GET /disconnect`
- **Description**: Sign out a user based on the authentication token.
- **Request Header**: `X-Token: authentication_token`
- **Response**: No content (204)

#### 6. Get User Profile

- **Endpoint**: `GET /users/me`
- **Description**: Retrieve the user's profile based on the authentication token.
- **Request Header**: `X-Token: authentication_token`
- **Response**: `{ "id": "user_id", "email": "user@example.com" }`

#### 7. Upload a File

- **Endpoint**: `POST /files`
- **Description**: Upload a new file to the platform.
- **Request Header**: `X-Token: authentication_token`
- **Request Body**:

  ```json
  {
    "name": "myFile.txt",
    "type": "file",
    "data": "Base64-encoded-data"
  }
  ```

- **Response**: `{ "id": "file_id", "userId": "user_id", "name": "myFile.txt", "type": "file", "isPublic": false, "parentId": 0 }`

#### 8. List Files

- **Endpoint**: `GET /files`
- **Description**: List all user files with pagination.
- **Request Header**: `X-Token: authentication_token`
- **Query Parameters**:
  - `page` (default: 0) - Page number (starts at 0).
- **Response**: Array of file objects.

#### 9. Get File Details

- **Endpoint**: `GET /files/:id`
- **Description**: Get details of a specific file.
- **Request Header**: `X-Token: authentication_token`
- **Response**: File details object.

#### 10. Publish a File

- **Endpoint**: `PUT /files/:id/publish`
- **Description**: Set

 a file's permissions to public.
- **Request Header**: `X-Token: authentication_token`
- **Response**: Updated file details object.

#### 11. Unpublish a File

- **Endpoint**: `PUT /files/:id/unpublish`
- **Description**: Set a file's permissions to private.
- **Request Header**: `X-Token: authentication_token`
- **Response**: Updated file details object.

#### 12. Delete a File

- **Endpoint**: `DELETE /files/:id`
- **Description**: Delete a file.
- **Request Header**: `X-Token: authentication_token`
- **Response**: No content (204)

## Testing

To run the tests, use the following command:

```bash
npm test
```
