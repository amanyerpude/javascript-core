
---

> [!quote] Metadata  
> **Posted on:** February 6, 2023  
> **Author:** Prashant Yadav  
> **Posted in:** Interview, JavaScript | Tagged SystemDesign  
> **Tags:** #browser #window #postMessage #interview #systemdesign

---

# Share Data Between Two Browser Windows with JavaScript

There are many **use cases** where we want to share data between two different **tabs or windows** that are on the **same domain**.

---

## Real-World Example: SSO Login Flow

Imagine a **Single Sign-On (SSO) login button**:

1. User clicks on the login button → a **new tab** opens.
    
2. User performs the login in the new window.
    
3. After successful login → this login window **closes itself**.
    
4. The original window is **refreshed** once it **receives login data** from the new tab.
    

👉 This is possible only if the **new window shares data back** with the opener.

---

## Setup

We will create two HTML files (`home.html` and `login.html`) and one shared JS file (`index.js`).  
Both HTML files will import `index.js`.

---

## Step 1: Home Page

Create a button that opens the login page in a new tab and another button to send a message from Home → Login.

```html
<!-- home.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Home</title>
  </head>
  <body>
    <h1>Hello, this is the Home page.</h1>
    <button onclick="openLogin()">Open Login</button>
    <button onclick="sendMsgToLogin()">Send message to Login</button>
    <script src="./index.js"></script>
  </body>
</html>
```

---

## Step 2: Login Page

Create a button to **send a message back** to the home window and then close the login tab.

```html
<!-- login.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Login</title>
  </head>
  <body>
    <h1>Hello, this is the login page.</h1>
    <button onclick="sendMsgToHome()">Submit</button>
    <script src="./index.js"></script>
  </body>
</html>
```

---

## Step 3: Shared Logic (`index.js`)

```javascript
let loginWindow;

const openLogin = () => {
  loginWindow = window.open("login.html", "_blank");
};

const sendMsgToLogin = () => {
  loginWindow.postMessage({ login: "Hello from Home" }, "*");
};

const sendMsgToHome = () => {
  window.opener.postMessage({ home: "Hello from Login" }, "*");
  setTimeout(() => {
    window.close();
  }, 2000);
};

window.addEventListener("message", (event) => {
  if (event.data?.home) {
    console.log("Home page received a message", event.data?.home);
  }

  if (event.data?.login) {
    console.log("Login page received a message", event.data?.login);
  }
});
```

---

## Explanation of Functions

- **`openLogin()`**  
    Opens `login.html` in a new tab and stores its reference in `loginWindow`.
    
- **`sendMsgToLogin()`**  
    Sends a message to the login window using **`postMessage`**.
    
- **`sendMsgToHome()`**
    
    - Sends a message back to the **opener window** (`window.opener`).
        
    - Uses `postMessage` again to notify Home.
        
    - Closes the login window after a **2-second delay**.
        
- **`window.addEventListener("message")`**  
    Listens for any incoming messages in both windows.  
    Based on the content, logs whether it was received on Home or Login.
    

---

## Why `postMessage`?

- `postMessage` enables **cross-window communication**.
    
- Works for **same-origin tabs** and **controlled cross-origin with explicit target origin**.
    
- Essential for building **SSO, payment flows, or pop-up based OAuth**.
    

⚠️ **Security Note:**  
Avoid using `"*"` as the target origin in production. Always specify the exact origin (`"https://example.com"`) to prevent malicious scripts from hijacking messages.

---

## Real-World Relevance

This pattern is widely used in:

- **OAuth login flows** (Google, Facebook login).
    
- **SSO authentication**.
    
- **Payment gateway popups**.
    
- **Multi-tab communication** for collaborative apps.
    

---

> [!tip] Insights
> 
> - Use **`window.open`** and store references to new windows.
>     
> - Use **`window.opener`** for communication back to the opener.
>     
> - Always handle `postMessage` with a **specific target origin** in real systems.
>     
> - Don’t forget to **close popup windows** after the task is done.
>     

---

> [!summary] Takeaway
> 
> - Open a new tab with `window.open`.
>     
> - Share data via `postMessage`.
>     
> - Listen using the `"message"` event listener.
>     
> - Close the tab with `window.close()` after completing the flow.
>     
> - This is the foundation of **SSO and popup-based workflows**.
>     

---

### 📎 Reference

- [Original Post on LearnersBucket](https://learnersbucket.com/examples/interview/share-data-between-two-browser-window-with-javascript/)
    

---
