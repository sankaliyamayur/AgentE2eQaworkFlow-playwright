# User Story: Admin Login

## Story Title
As an Admin User, I want to securely log in to the Admin Panel so that I can access platform modules and perform actions based on my assigned role and permissions.

## Story Description
Implement a secure admin authentication flow that allows admin users to log in using valid credentials, verify their assigned roles and permissions, complete Two-Factor Authentication (2FA), and access the appropriate dashboard. The system must validate account status, enforce role-based access, and maintain audit logs for all login activities.

## Application URL
https://xuprocoin-admin.appworkdemo.com/admin/login

## Test Credentials

### Super Admin
- Email: `admin@xuprocoin.com`
- Password: `Xupro_Admin@2026Kz`

### Finance Admin User
- Email: `mayur97@yopmail.com`
- Password: `Admin@123`

### Static OTP
- OTP: `4444`

---

## Acceptance Criteria

### AC1: Valid Login
- GIVEN the admin account is active
- WHEN the admin enters valid credentials
- THEN the system should authenticate successfully
- AND redirect to the assigned dashboard

### AC2: Account Status Validation
- GIVEN the admin account exists
- WHEN the account status is inactive
- THEN the system should deny access
- AND display an appropriate error message

### AC3: Role Validation
- GIVEN the admin credentials are valid
- WHEN authentication is successful
- THEN the system should validate assigned roles and permissions
- AND load the corresponding dashboard

### AC4: Two-Factor Authentication
- GIVEN 2FA is enabled
- WHEN valid OTP is entered
- THEN the system should allow dashboard access

### AC5: Audit Logging
- GIVEN the login is successful
- WHEN session creation completes
- THEN the system should store audit logs with admin activity

---

## Preconditions
- Admin account exists in the system
- Admin account status is Active
- Admin role is assigned
- Admin has valid login credentials
- Admin panel is accessible

---

## Postconditions
- Admin session is created successfully
- Admin dashboard is loaded based on assigned role
- Login activity is stored in audit logs
- Last login timestamp is updated
- Security monitoring records the login event

---

## Navigation Flow

### Main Flow
Admin Login Page → Enter Credentials → Authentication → Role Validation → OTP Verification → Dashboard

### Alternative Flow
Admin Login Page → Forgot Password → Reset Password → Login

---

## Functional Flow (Core Logic)

### Step 1: Open Admin Login Page
- Admin accesses the Admin Portal URL
- System displays login screen

### Step 2: Enter Credentials
Admin enters:
- Username / Email
- Password

System validates:
- Mandatory fields
- Account existence

### Step 3: Authenticate User
System verifies:
- Username/Email
- Password
- Account status

#### Case A: Success
Proceed to Role Validation

#### Case B: Failure
- Display error message
- Increment failed login counter

### Step 4: Role Validation
System verifies:
- Assigned role
- Role-specific permissions

### Step 5: Two-Factor Authentication (2FA)
If enabled:
- Generate OTP
- Send OTP
- Verify OTP

#### Successful OTP
Proceed to dashboard

#### Failed OTP
Display validation error

### Step 6: Session Creation
System creates secure admin session and stores:
- Session ID
- Login Timestamp
- Device Information
- IP Address

### Step 7: Dashboard Redirection

Based on role:

- Super Admin → Full Platform Dashboard
- Finance Admin User → Finance Dashboard
- Investment Admin User → Investment Dashboard
- Support Admin User → Support Dashboard
- Risk Admin User→ Monitoring Dashboard

### Step 8: Audit Logging
System stores:
- Admin ID
- Login Timestamp
- IP Address
- Device Information
- Login Status

---

## Success Flow

| Step | Actor | Action |
|---|---|---|
| 1 | Admin | Opens login page |
| 2 | Admin | Enters credentials |
| 3 | System | Validates credentials |
| 4 | System | Validates role |
| 5 | System | Creates session |
| 6 | System | Redirects dashboard |
| 7 | System | Creates audit log |

---

## Error Flow

| Scenario | Trigger | System Response |
|---|---|---|
| Invalid Username | Username not found | Show validation message |
| Invalid Password | Wrong password | Show validation message |
| Inactive Account | Account disabled | Access denied |
| Invalid OTP | Wrong OTP | OTP validation failed |
| OTP Expired | OTP timeout | Request new OTP |
| Session Failure | Server issue | Login failed |
| Permission Error | No role assigned | Access denied |

---

## Business Rules
- Only active admin users can log in
- RBAC must be enforced
- Every login attempt must be logged
- Session timeout must be configurable
- Passwords must be encrypted

---

## Validations

| Item | Rule | Error |
|---|---|---|
| Username/Email | Mandatory | Username/Email is required |
| Username/Email | Must exist | Account not found |
| Password | Mandatory | Password is required |
| Password | Must match | Invalid password |
| Account Status | Must be Active | Account inactive |
| Role Assignment | Must exist | Role not assigned |
| OTP | Must be valid | Invalid OTP |
| OTP | Must not expire | OTP expired |

---

## Notifications

| Event | Recipient | Channel |
|---|---|---|
| Successful Login | Admin | In-App |
| New Device Login | Admin | Email |
| New Device Login | Admin | In-App |

---

## Audit Logs
System should record:
- Admin ID
- Login Timestamp
- IP Address
- Device Information
- Login Status
- Role Assigned

---

## Security Notes
- All authentication APIs must be secured
- Passwords must never be stored in plain text
- Future SSO integration should be supported
- Security logs must be retained for audit
- Login activity should be visible in Admin Audit Reports