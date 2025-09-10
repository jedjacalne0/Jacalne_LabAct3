# Laboratory Activity 3: API Authentication using JWT
This project focuses on building a standalone authentication API using Express.js and MySQL. It provides essential functionality for secure user management, including signup, login, logout with token revocation, and access to a protected /profile endpoint through JWT authentication. To ensure security, user passwords are never stored in plain text but are instead hashed using bcrypt. The result is a robust and scalable foundation for handling authentication in modern web applications.

# Step-by-Step Procedure

  # Files Initialization
  - Initialize the database through the use of XAMPP Control Panel by starting Apache and MySQL
  - Open PHP Admin and create a database named **lab_auth** with collation **utf8mb4_general_ci**
  
        CREATE TABLE IF NOT EXISTS users (
          id INT AUTO_INCREMENT PRIMARY KEY,
          email VARCHAR(100) NOT NULL UNIQUE,
          password_hash VARCHAR(255) NOT NULL,
          full_name VARCHAR(120),
          role VARCHAR(30) DEFAULT 'student',
          created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;
        
        CREATE TABLE IF NOT EXISTS revoked_tokens (
          id INT AUTO_INCREMENT PRIMARY KEY,
          jti VARCHAR(64) NOT NULL UNIQUE,
          expires_at DATETIME NOT NULL,
          revoked_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
        ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

  - Tables users and revoked_tokens should exist
    
  - Initialize the files:
  
        mkdir lab-auth-api
        cd lab-auth-api
        npm init -y
        npm install express mysql2 dotenv cors bcryptjs jsonwebtoken uuid
        npm install -D nodemon

  # Postman Testing
  - After initializing the files and fully functioning, proceed to Postman for testing different authentication processes   (Signup, Login, Protected, Logout), including errors that may come through the API.
    
  - Create a new environment named **dev**
  - Variable = **baseUrl**
  - Value = **http://localhost:3000/api**
    
  - Now test for Signup
    
        POST {{baseUrl}}/auth/signup
    
        Content-Type: application/json
        
        {
          "email": "user1@example.com",
          "password": "Pass@1234",
          "full_name": "User One",
          "role": "student"
        }
    
  - Now test for Login

        POST {{baseUrl}}/auth/login
    
        Content-Type: application/json
        
        {
          "email": "user1@example.com",
          "password": "Pass@1234"
        }

  - Now test for Login with Scripts

        if (pm.response.code === 200) {
          const data = pm.response.json();
          pm.environment.set("token", data.token);
        }

  - Now test for Profile (Protected)
      - Select Authorization
      - In Auth Type, select Bearer Token
      - In toke add {{token}}

            GET {{baseUrl}}/profile
            Authorization: Bearer {{token}}

  - Now test for Logout (Revocation)
    - Follow the same configuration of the authorization header.

          POST {{baseUrl}}/auth/logout
          Authorization: Bearer {{token}}

  - Now do Negative Tests
    - Duplicate signup → 409
      - Enter the same email since it has a unique constraint 

              POST {{baseUrl}}/auth/signup
        
    - Wrong password → 401
      - Enter a wrong password from the previous signup
  
             POST {{baseUrl}}/auth/login

    - No token on /profile → 401
      - In toke, don't put anything    
   
            GET {{baseUrl}}/profile
            Authorization: Bearer (Empty)

    - Tampered/expired token → 401
      - No Auth type and take

            GET {{baseUrl}}/profile
            Authorization: Empty (Empty)



    
          
