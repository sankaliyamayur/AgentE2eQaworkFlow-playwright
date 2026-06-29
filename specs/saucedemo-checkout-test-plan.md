# Test Plan: Admin Login Authentication Flow

This document details the test scenarios, test cases, and validation strategies for the Admin Login and Two-Factor Authentication (2FA) flows on the Admin Panel.

## Target Application Details
* **Application URL**: `https://xuprocoin-admin.appworkdemo.com/admin/login`
* **Target Environment**: Staging / Demo Admin Panel
* **Bypass OTP**: `4444` (Static OTP)

## Test Environment & Configurations
* **Supported Browsers**: Chromium, Firefox, WebKit (Safari)

---

## Test Scenarios & Detailed Test Cases

### Scenario 1: Happy Path - Super Admin Authentication & Redirection (AC1, AC3, AC4)
* **Test Case ID**: TC-LOGIN-001
* **Title**: Successfully log in as Super Admin and verify redirection to the full platform dashboard.
* **Preconditions**:
  * Super Admin account `admin@xuprocoin.com` is active and exists.
  * 2FA is enabled.
* **Step-by-Step Instructions**:
  1. Navigate to the login page `https://xuprocoin-admin.appworkdemo.com/admin/login`.
  2. Enter email `admin@xuprocoin.com` in the email input field.
  3. Enter password `Xupro_Admin@2026Kz` in the password input field.
  4. Click the "Sign In" button (or press Enter).
  5. Verify redirection to the Two-Factor Authentication (2FA) verification view.
  6. Enter OTP `4444` in the verification code input.
  7. Click "Verify & Continue" (or press Enter).
* **Expected Results**:
  * After Step 4, the URL remains `/admin/login` but the view state displays "TWO-FACTOR AUTH".
  * After Step 7, the URL changes to `/admin/dashboard`.
  * The full platform dashboard elements load successfully, showing the sidebar options (Wallets, KYC, Settings, Users, Admin Users, Roles, Deposits, Withdrawals).
* **Test Data**:
  * Email: `admin@xuprocoin.com`
  * Password: `Xupro_Admin@2026Kz`
  * OTP: `4444`

---

### Scenario 2: Happy Path - Finance Admin Authentication & Redirection (AC1, AC3, AC4)
* **Test Case ID**: TC-LOGIN-002
* **Title**: Successfully log in as Finance Admin User and verify redirection.
* **Preconditions**:
  * Finance Admin account `mayur97@yopmail.com` is active and exists.
  * 2FA is enabled.
* **Step-by-Step Instructions**:
  1. Navigate to the login page.
  2. Enter email `mayur97@yopmail.com` in the email input field.
  3. Enter password `Admin@123` in the password input field.
  4. Click the "Sign In" button.
  5. Verify redirection to the Two-Factor Authentication (2FA) page.
  6. Enter OTP `4444` in the verification code input.
  7. Click "Verify & Continue".
* **Expected Results**:
  * Redirection is successful. The user is logged in and redirected to their respective dashboard view.
* **Test Data**:
  * Email: `mayur97@yopmail.com`
  * Password: `Admin@123`
  * OTP: `4444`

---

### Scenario 3: Negative Scenario - Client-side Empty Fields Validation (AC1, AC2)
* **Test Case ID**: TC-LOGIN-003
* **Title**: Validate form behavior when submitting empty credentials.
* **Preconditions**: None.
* **Step-by-Step Instructions**:
  1. Navigate to the login page.
  2. Leave the email and password fields blank.
  3. Click the "Sign In" button.
* **Expected Results**:
  * The form is not submitted.
  * Inline validation error messages are displayed:
    * Under Email input: `"Email is required"`
    * Under Password input: `"Password is required"`
  * Input border colors change to pink/red (e.g., classes updated with `!border-[#ff5c8a]`).

---

### Scenario 4: Negative Scenario - Invalid Credentials Toast Validation (AC1)
* **Test Case ID**: TC-LOGIN-004
* **Title**: Validate system response when entering incorrect email or password.
* **Preconditions**: None.
* **Step-by-Step Instructions**:
  1. Navigate to the login page.
  2. Enter email `admin@xuprocoin.com`.
  3. Enter password `WrongPassword123`.
  4. Click the "Sign In" button.
* **Expected Results**:
  * The form submits.
  * The page does not redirect to the 2FA screen.
  * A toast notification containing the error message `"Invalid credentials"` is displayed.

---

### Scenario 5: Negative Scenario - Invalid Two-Factor OTP Validation (AC4)
* **Test Case ID**: TC-LOGIN-005
* **Title**: Validate error handling on the 2FA page when entering an incorrect OTP.
* **Preconditions**:
  * Successfully inputted valid email/password in the login view.
* **Step-by-Step Instructions**:
  1. From the 2FA page, enter an incorrect OTP (e.g., `1234` or `0000`).
  2. Click "Verify & Continue".
* **Expected Results**:
  * Redirection is blocked.
  * An error toast/notification is shown, or the field validation triggers.
  * OTP field can be cleared for re-entry.

---

### Scenario 6: UI Element & Accessibility Validation
* **Test Case ID**: TC-LOGIN-006
* **Title**: Verify visual design, layout structure, placeholders, and basic elements of the Login View.
* **Preconditions**: None.
* **Step-by-Step Instructions**:
  1. Navigate to the login page.
  2. Check email field placeholder.
  3. Check password field placeholder.
  4. Verify the visibility of the "Sign In" button.
* **Expected Results**:
  * Email field has placeholder `admin@xuprocoin.com`.
  * Password field has placeholder `••••••••`.
  * Sign In button has text `Sign In` and is fully visible.

---

### Scenario 7: Navigation & Session Lifecycle - Logout Flow
* **Test Case ID**: TC-LOGIN-007
* **Title**: Verify that a logged-in user can log out and their session is destroyed.
* **Preconditions**:
  * User is successfully logged in and redirected to the dashboard.
* **Step-by-Step Instructions**:
  1. Navigate to the profile/logout section in the sidebar/header.
  2. Click the "Logout" button.
  3. Attempt to navigate back to the dashboard URL (`/admin/dashboard`) manually.
* **Expected Results**:
  * The user is immediately redirected to the login page `/admin/login`.
  * Navigating back to `/admin/dashboard` redirects the user back to `/admin/login` (unauthenticated redirection).
