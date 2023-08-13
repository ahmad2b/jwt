# JWT
Here I have break JWT in simpler and more digestible sections for better understanding.

### What is JSON Web Token (JWT)?

JSON Web Token (JWT) is a compact, self-contained way of securely transmitting information between two parties. It's a string that can be verified and trusted since it's digitally signed.

1. **Signed Tokens**: Ensures the integrity of the information.
2. **Encrypted Tokens**: Provide secrecy between parties, hiding the information.

### When Should You Use JWT?

1. **Authorization**: After login, JWT allows the user to access specific routes and services. It's widely used in Single Sign-On (SSO) implementations.
2. **Information Exchange**: For securely transmitting information, ensuring the sender's identity and data integrity.

### Structure of JWT

JWT is made up of three parts:

1. **Header**: Contains the type of token and signing algorithm (e.g., HS256).
2. **Payload**: Contains claims, which are statements about a user or additional data. There are three types of claims:
   - **Registered**: Standard claims (e.g., issuer, expiration time).
   - **Public**: Defined by the user but should be collision resistant.
   - **Private**: Custom claims between parties.
3. **Signature**: Verifies that the message hasn't changed and the sender is authentic.

### How Do JWTs Work?

1. **User logs in**, receiving a JWT.
2. **User sends the JWT** in the Authorization header to access protected resources.
3. **Server checks the JWT**, allowing or denying access based on its validity.

**Note**: Care should be taken with token size and sensitive information in the payload, as it may be readable by anyone.

### Why Use JWT?

Compared to other tokens like Simple Web Tokens (SWT) and Security Assertion Markup Language Tokens (SAML), JWT has some advantages:

1. **Compact Size**: Less verbose than XML, so it's smaller and more efficient.
2. **Versatile Signing Options**: Can use public/private key pairs.
3. **Easier to Work With**: Direct mapping to objects in most programming languages.
4. **Widely Used**: Especially on mobile platforms, showing its ease of implementation.

### How to Play with JWT?

You can use tools like the jwt.io Debugger to decode, verify, and generate JWTs, to better understand how they function.

# JWT Implementation & its flow

Below there is a mnetal modal and user flow for implementing login, signup, and exchanging information.

### 1. Signup

During the signup process, you can create a JWT to confirm the user's email address or include other information.

#### Backend:

1. **Create User**: Store the user's information in the database.
2. **Create JWT**: Sign a JWT that includes the user's email or ID.
3. **Send Email**: Send an email to the user with a link containing the JWT.

#### Frontend:

1. **Collect User Information**: Use a form to gather the user's signup information.
2. **Send Signup Request**: Post the information to the backend.
3. **Handle Confirmation**: Direct the user to check their email for a confirmation link.

### 2. Login

#### Backend:

1. **Verify Credentials**: Check the user's credentials against the database.
2. **Create JWT**: If valid, create and sign a JWT that includes the user's ID or other identifying information.
3. **Send JWT**: Respond to the client with the JWT.

```javascript
import jwt from 'jsonwebtoken';

function login(req, res) {
  const { username, password } = req.body;

  // Verify credentials...
  const user = /* fetch user from database using username and password */;

  // Sign JWT
  const token = jwt.sign({ id: user.id }, 'your-secret-key', { expiresIn: '1d' });

  // Send JWT
  res.json({ token });
}
```

#### Frontend:

1. **Collect Credentials**: Use a form to gather the username and password.
2. **Send Login Request**: Post the credentials to the backend.
3. **Store JWT**: If the login is successful, store the JWT, perhaps in an HTTP cookie or local storage.
4. **Redirect**: Redirect the user to their dashboard or other authenticated page.

In Next.js, you can use the `useRouter` hook to programmatically navigate:

```javascript
import { useRouter } from 'next/router';

function LoginPage() {
  const router = useRouter();

  const handleLogin = async (username, password) => {
    // Send login request...
    const response = await fetch('/api/login', { /* ... */ });
    const data = await response.json();

    // Store token...
    localStorage.setItem('token', data.token);

    // Redirect
    router.push('/dashboard');
  };
}
```

### 3. Exchanging Information

With a JWT in place, you can include it in the headers of subsequent HTTP requests to authenticate those requests and access or modify protected resources.

#### Backend:

1. **Verify JWT**: For incoming requests, verify the JWT.
2. **Extract Information**: Use the information in the JWT to perform actions specific to the user.

#### Frontend:

1. **Include JWT in Requests**: When making requests to protected endpoints, include the JWT in the `Authorization` header.

```javascript
const token = localStorage.getItem('token');
const response = await fetch('/api/protected', {
  headers: {
    Authorization: `Bearer ${token}`
  }
});
```
