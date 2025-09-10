# Laboratory Activity 3: API Authentication using JWT
This project focuses on building a standalone authentication API using Express.js and MySQL. It provides essential functionality for secure user management, including signup, login, logout with token revocation, and access to a protected /profile endpoint through JWT authentication. To ensure security, user passwords are never stored in plain text but are instead hashed using bcrypt. The result is a robust and scalable foundation for handling authentication in modern web applications.

# Step-by-Step Procedure
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

- 
