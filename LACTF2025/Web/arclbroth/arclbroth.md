# Arclbroth Writeup

**Team: M4j4nDyn4sty**

![Challenge Screenshot](./challenge.png)
## Overview

Arclbroth is a web-game-based challenge where your objective is to brew a flag using "arcs" as your currency. Regular users start with just 10 arcs which is too few to meet the 50-arc threshold required to brew the flag while the admin account is automatically granted 100 arcs. Normally, the username "admin" is reserved and protected against duplicate registrations. However, by exploiting a null byte injection vulnerability during registration, I managed to bypass this restriction and register as the admin then voila I got the flag :)


## Vulnerability

**Null Byte Injection in Registration:** By injecting a null byte into the username, I was able to bypass the duplicate check.


## Exploitation

1. **Intercepting the Registration Request:**

   I used Burp Suite to intercept the registration request. I attempted to register with the following credentials:

   - **Username:** `admin\u0000hax`
   - **Password:** `password`

   I noticed that the website automatically added an extra escape character, changing the username to `admin\\u0000hax`

   ![Intercepting the Registration Request Screenshot](./exploit1.png)

2. **Correcting the Payload:**

   Before forwarding the request, I edited the payload back to:
   
   ```json
   {"username": "admin\u0000hax", "password": "password"}
   ```
   ![Correcting the Payload Screenshot](./requestModification.png)
3. **Logging in as Admin:**

   After registering with the credentials above, the server processed the username as admin (since SQLite stops at the null byte), then I was logged in as admin and granted 100 arcs.
   <br><br>
   ![Logging in as Admin Screenshot](./100_arcs.png)
4. **Brewing the flag:**
  
   Finally, I clicked the "brew" button. With 100 arcs available, the system deducted 50 arcs and returned the flag in the response:
    `lactf{bulri3v3_it_0r_n0t_s3cur3_sqlit3_w4s_n0t_s3cur3}`
    <br><br>
   ![Brewing the Flag Screenshot](./flagPic.png)

<br><br>
<div align="center">
  <strong>Sincerely yours (Aura 1000+),</strong><br>
  <strong>Ahmed Al-Naamani (C00k1eSn4tch3r)</strong>
</div>



