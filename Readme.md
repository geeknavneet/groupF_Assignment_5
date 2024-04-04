# User Management System with Invitation Code

## Overview

This Go package offers functionalities for managing users, covering user registration, login, and admin management. It features HTTP handlers to handle user registration, login, admin registration, and invitation code generation.

## Features

- **User Registration:** Allows users to register with a unique username and password.
- **User Login:** Enables users to authenticate themselves with their registered credentials.
- **Admin Registration:** Provides functionality for registering admin users with special privileges.
- **Invitation Code Generation:** Generates unique invitation codes for user registration.

### Backend Server:

- Developed in Go (Golang).
- Handles HTTP requests and interacts with the PostgreSQL database.
- Provides endpoints for user registration, login, admin registration, and invitation code generation.
- Utilizes bcrypt for password hashing and JSON Web Tokens (JWT) for authentication.

### Frontend Interface:

- Developed using HTML, CSS, and JavaScript.
- Provides a user-friendly interface for user registration and login.
- Includes a separate page for generating invitation codes.

## Installation

To utilize this package, ensure you have Go installed. You can install the package via the following command:

```bash
git clone https://github.com/vinaychhabra/goUserManagement.git
```

After cloning, run the setup script:

```bash
./setup_postgres.sh
```

Then execute the Go code:

```bash
go run main.go
```

## Usage

Once installed, import the package in your Go code and utilize its functionalities. Below is an example of setting up a user management server:

```go
package main

import (
	"database/sql"
	"fmt"
	"log"
	"net/http"
	"path/filepath"

	_ "github.com/lib/pq"
)

func main() {
	// Set up database connection
	db := SetupDatabase()
	defer db.Close()

	// Set up HTTP handlers
	http.HandleFunc("/register", RegisterHandler(db))
	http.HandleFunc("/login", LoginHandler(db))
	http.HandleFunc("/generate-invitation", GenerateInvitationHandler(db))
	http.HandleFunc("/register-admin", RegisterAdminHandler(db))
	http.HandleFunc("/invite", invitePageHandler)
	http.HandleFunc("/", StaticFileHandler)

	// Start server
	log.Println("Server started on :8081")
	log.Fatal(http.ListenAndServe(":8081", nil))
}
```

Ensure to replace `SetupDatabase`, `RegisterHandler`, `LoginHandler`, `GenerateInvitationHandler`, `RegisterAdminHandler`, `invitePageHandler`, and `StaticFileHandler` with your actual implementations.

## Endpoints

- `/register`: Endpoint for user registration. Expects a POST request with JSON containing username, password, and invitation_code fields.
- `/login`: Endpoint for user login. Expects a POST request with JSON containing username and password fields.
- `/generate-invitation`: Endpoint for generating an invitation code. Expects a POST request with JSON containing admin credentials (username and password fields).
- `/register-admin`: Endpoint for registering an admin user. Expects a POST request with JSON containing username and password fields.
- `/invite`: Endpoint for serving the invite page.
- `/`: Endpoint for serving static files (frontend). This serves the main index.html file.

## Folder Structure

- `frontend/`: Contains the HTML, CSS, and JavaScript files for the frontend interface.
  - `index.html`: User registration and login interface.
  - `invite/`: Folder with index.html for invitation code generation.
- `db_script/`: Contains the Bash script for setting up the PostgreSQL database container.
  - Creates necessary tables for users, invitations, and admins.
- `main.go`: Go source code for the backend server.
  - Includes main server logic, database interaction, and HTTP request handlers.
- `go.mod`: Go module file for managing dependencies.
- `README.md`: Markdown file containing project documentation and setup instructions.