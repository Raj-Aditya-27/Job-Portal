Here’s a comprehensive documentation for the JavaScript files you shared, including routes, controllers, and middlewares:

---

# **Project Documentation**

## **1. `user.controller.js`**

This file handles the logic for user-related actions such as registration, login, logout, and profile update.

### **Functions:**

- **register:**  
  - Registers a new user with required fields such as `fullname`, `email`, `phoneNumber`, `password`, and `role`.
  - Validates input, checks if a user with the same email exists, hashes the password, and stores it in the database.
  - **Route:** `POST /register`
  - **Middleware:** Uses `singleUpload` for profile picture upload but currently commented out.
  
- **login:**  
  - Logs in a user by validating email, password, and role. On success, returns a JWT token stored in cookies.
  - **Route:** `POST /login`

- **logout:**  
  - Logs out a user by clearing the JWT token from cookies.
  - **Route:** `GET /logout`

- **updateProfile:**  
  - Allows an authenticated user to update their profile details like `fullname`, `email`, `phoneNumber`, `bio`, and `skills`.
  - Uses `singleUpload` middleware to upload profile photos or resumes (cloudinary integration is commented out).
  - **Route:** `POST /profile/update`
  - **Middleware:** `isAuthenticated` to verify user authentication.

---

## **2. `job.controller.js`**

This file handles job-related operations such as posting a job, retrieving jobs, and admin-specific operations.

### **Functions:**

- **postJob:**  
  - Allows an authenticated admin to create a new job with fields such as `title`, `description`, `requirements`, `salary`, `location`, etc.
  - **Route:** `POST /post`
  - **Middleware:** `isAuthenticated`

- **getAllJobs:**  
  - Retrieves all jobs with a keyword search feature. Jobs are populated with company details.
  - **Route:** `GET /get`
  - **Middleware:** `isAuthenticated`

- **getJobById:**  
  - Retrieves details of a specific job by its ID. It also populates applications related to the job.
  - **Route:** `GET /get/:id`
  - **Middleware:** `isAuthenticated`

- **getAdminJobs:**  
  - Retrieves all jobs posted by the authenticated admin.
  - **Route:** `GET /getadminjobs`
  - **Middleware:** `isAuthenticated`

---

## **3. `application.controller.js`**

This file manages actions related to job applications, such as applying for jobs, viewing applicants, and updating application status.

### **Functions:**

- **applyJob:**  
  - Allows authenticated users to apply for a job by job ID.
  - **Route:** `GET /apply/:id`
  - **Middleware:** `isAuthenticated`

- **getAppliedJobs:**  
  - Retrieves all jobs a user has applied to.
  - **Route:** `GET /get`
  - **Middleware:** `isAuthenticated`

- **getApplicants:**  
  - Allows admins to view all applicants for a specific job by job ID.
  - **Route:** `GET /:id/applicants`
  - **Middleware:** `isAuthenticated`

- **updateStatus:**  
  - Allows admins to update the application status for a specific applicant.
  - **Route:** `POST /status/:id/update`
  - **Middleware:** `isAuthenticated`

---

## **4. `company.controller.js`**

This file handles company-related functionalities such as registration, company retrieval, and updates.

### **Functions:**

- **registerCompany:**  
  - Registers a new company. The admin user must be authenticated to register a company.
  - **Route:** `POST /register`
  - **Middleware:** `isAuthenticated`

- **getCompany:**  
  - Retrieves all companies.
  - **Route:** `GET /get`
  - **Middleware:** `isAuthenticated`

- **getCompanyById:**  
  - Retrieves company details by company ID.
  - **Route:** `GET /get/:id`
  - **Middleware:** `isAuthenticated`

- **updateCompany:**  
  - Allows an authenticated user to update company details and upload company-related media using `singleUpload`.
  - **Route:** `PUT /update/:id`
  - **Middleware:** `isAuthenticated`, `singleUpload`

---

## **5. `application.router.js`**

This file defines the routes related to job applications. It connects the application controller functions to specific HTTP routes.

### **Routes:**

- `GET /apply/:id`: Calls `applyJob` to apply for a specific job by ID. Requires authentication.
- `GET /get`: Calls `getAppliedJobs` to retrieve all applied jobs for the current user. Requires authentication.
- `GET /:id/applicants`: Calls `getApplicants` to get all applicants for a specific job. Requires authentication.
- `POST /status/:id/update`: Calls `updateStatus` to update the application status of a candidate. Requires authentication.

---

## **6. `company.router.js`**

This file defines routes for handling company-related operations.

### **Routes:**

- `POST /register`: Calls `registerCompany` to register a new company. Requires authentication.
- `GET /get`: Calls `getCompany` to retrieve all companies. Requires authentication.
- `GET /get/:id`: Calls `getCompanyById` to retrieve company details by ID. Requires authentication.
- `PUT /update/:id`: Calls `updateCompany` to update company details. Requires authentication and uses `singleUpload`.

---

## **7. `job.route.js`**

This file defines routes for job-related functionalities.

### **Routes:**

- `POST /post`: Calls `postJob` to create a new job. Requires authentication.
- `GET /get`: Calls `getAllJobs` to retrieve all jobs, with a keyword search option. Requires authentication.
- `GET /getadminjobs`: Calls `getAdminJobs` to get jobs posted by the current admin. Requires authentication.
- `GET /get/:id`: Calls `getJobById` to retrieve job details by ID. Requires authentication.

---

## **8. `user.route.js`**

This file defines routes for user-related operations such as registration, login, logout, and profile updates.

### **Routes:**

- `POST /register`: Calls `register` to register a new user. Requires `singleUpload` for uploading profile pictures.
- `POST /login`: Calls `login` to log in a user.
- `GET /logout`: Calls `logout` to log out the current user.
- `POST /profile/update`: Calls `updateProfile` to update the profile of an authenticated user. Requires authentication and uses `singleUpload` for profile updates.

---

## **Middlewares:**

- **isAuthenticated.js:**  
  Ensures that a user is authenticated before accessing certain routes.
  
- **multer.js:**  
  A middleware for handling file uploads via `singleUpload`, used for profile pictures and other media uploads.

---

## **Technologies Used:**

- **Express.js:** A framework used for building the backend of the application.
- **Mongoose:** For MongoDB object data modeling (ODM).
- **bcryptjs:** For password hashing.
- **jsonwebtoken (JWT):** For user authentication.
- **Multer:** For handling file uploads.
- **Cloudinary:** For cloud storage of media files (profile pictures, resumes).
  
---

## **Error Handling:**

Each function uses `try-catch` blocks to handle errors. If an error occurs, it returns an appropriate status code (400, 404, or 500) and an error message in the response.

## **Security:**

- Passwords are hashed before storage using `bcryptjs`.
- User sessions are managed via JWT tokens stored in cookies. Routes that require user authentication use the `isAuthenticated` middleware.

---

### **Contribution Guidelines:**

- Ensure that required fields are validated before performing any operations.
- For database operations, use Mongoose queries such as `find`, `findById`, and `populate` for relationships.
- Handle file uploads carefully and ensure cloud storage setup (e.g., Cloudinary) is properly integrated.
- Use JWT tokens securely, particularly with proper cookie configurations like `httpOnly` and `sameSite`.

---

This documentation provides an overview of the major components, routes, and controllers in the project. Let me know if you need any more information!