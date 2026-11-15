# Question 2: Login System Test Case Design

## Test setup and expected behaviour

These are designed test cases, not executed test results; no login application was supplied.

Use these fictional test accounts: `user1` / `pass123` (7 characters) and `user6` / `abc123` (exactly 6 characters). `unknown_user` does not exist. Start each independent case with unlocked accounts and zero failed attempts unless the case states otherwise. Each attempt means entering the stated values and clicking **Login**. Sign out between successful logins when a case contains multiple attempts.

The requirements do not specify exact message wording or how validation failures affect the counter. For this design, use these explicit assumptions:

- Empty means a zero-length string (`""`). An empty username displays **"Username is required."**
- An empty or short password displays **"Password must be at least 6 characters long."** If both fields fail validation, display both messages.
- Input-validation failures do not count as failed authentication attempts. A submitted incorrect password for an existing account increments that account's counter.
- Incorrect credentials display **"Invalid username or password."** Successful login displays **"Login successful."**, grants authenticated access, and resets the consecutive-failure counter to zero.
- The third consecutive incorrect-password attempt locks the account immediately and displays **"Account locked after 3 consecutive failed login attempts."** Subsequent attempts, including correct credentials, display **"Account is locked."** and grant no access. Unlock duration and recovery are outside the supplied requirements.

These assumptions define a precise test oracle and should be confirmed against an actual application's specification before execution.

| Test Case ID | Test Scenario | Input Data | Expected Output |
|---|---|---|---|
| TC_01 | Valid login with a password longer than 6 characters | Username = `user1`; Password = `pass123` | Display "Login successful."; grant authenticated access; failed-attempt counter = 0. |
| TC_02 | Empty username | Username = `""`; Password = `pass123` | Display "Username is required."; do not authenticate or change any account's failed-attempt counter. |
| TC_03 | Empty password | Username = `user1`; Password = `""` | Display "Password must be at least 6 characters long."; deny access; counter remains 0. |
| TC_04 | Both fields empty | Username = `""`; Password = `""` | Display both "Username is required." and "Password must be at least 6 characters long."; deny access; no failed authentication is counted. |
| TC_05 | Password length just below the minimum (5) | Username = `user1`; Password = `abc12` | Display "Password must be at least 6 characters long."; deny access; counter remains 0. |
| TC_06 | Valid password length exactly at the minimum (6) | Username = `user6`; Password = `abc123` | Display "Login successful."; grant authenticated access; account remains unlocked with counter = 0. |
| TC_07 | Six-character password passes length validation but is incorrect | Username = `user1`; Password = `abc123` | Display "Invalid username or password."; deny access; counter = 1; account remains unlocked. |
| TC_08 | Nonexistent username with a sufficiently long password | Username = `unknown_user`; Password = `pass123` | Display "Invalid username or password."; deny access; no existing account is locked or has its counter changed. |
| TC_09 | First incorrect-password attempt | Username = `user1`; Password = `wrong123` | Display "Invalid username or password."; deny access; counter = 1; account remains unlocked. |
| TC_10 | Two consecutive incorrect-password attempts do not yet lock the account | Submit Username = `user1`, Password = `wrong123` twice consecutively | Each attempt displays "Invalid username or password." and denies access; counter progresses 1, 2; account remains unlocked after attempt 2. |
| TC_11 | Third consecutive failure locks the account | Submit Username = `user1`, Password = `wrong123` three times consecutively | Attempts 1 and 2 display "Invalid username or password."; attempt 3 displays "Account locked after 3 consecutive failed login attempts."; counter = 3; account is locked; all attempts deny access. |
| TC_12 | Correct credentials cannot bypass an existing lock | Precondition: `user1` is locked after 3 consecutive failures. Submit Username = `user1`; Password = `pass123` | Display "Account is locked."; deny authenticated access; account stays locked. |
| TC_13 | Successful login resets consecutive failures | For `user1`, submit passwords in order: `wrong123`, `wrong123`, `pass123`, sign out, then submit `wrong123` | First two attempts fail with "Invalid username or password." and counters 1, 2. Third attempt displays "Login successful.", grants access, and resets counter to 0. Final attempt displays "Invalid username or password."; counter = 1; account remains unlocked. |
| TC_14 | Failure counters belong to individual accounts | Submit `user1` / `wrong123` twice; then submit `user6` / `wrong123` once | Every attempt displays "Invalid username or password." and denies access. `user1` counter = 2; `user6` counter = 1; neither account is locked. |

## Coverage mapping

| Requirement / edge case | Test cases |
|---|---|
| Username must not be empty | TC_02, TC_04 |
| Password must contain at least 6 characters | TC_03–TC_07 |
| Valid credentials allow successful login | TC_01, TC_06, TC_13 |
| Invalid credentials display a precise error | TC_07–TC_11, TC_13, TC_14 |
| Lock after exactly 3 consecutive failures | TC_09–TC_13 |
| Empty password | TC_03, TC_04 |
| Length boundaries: 5, 6, and 7 characters | TC_05, TC_06, TC_01 |
