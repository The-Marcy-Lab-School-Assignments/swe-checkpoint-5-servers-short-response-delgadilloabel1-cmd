# Short Response Questions

Answer each of these questions completely but concisely. Use the proper technical terminology. You may refer to the [Marcy Lab School Docs](https://marcylabschool.gitbook.io/marcy-lab-school-docs) or Google but do NOT copy and paste definitions or explanations verbatim.

You can earn up to 6 points for each response (3 points for writing quality, 3 points for technical content).

Before submitting your responses, use a spell checker / AI to ensure that you have no grammar or spelling mistakes.

## Question 1: Servers and HTTP

What is a server? Describe the HTTP request-response cycle including the key components of a request (method, endpoint, headers, body) and a response (status code, headers, body). Use an analogy to support your explanation.

**Your Answer:**

A **server** is a computer or system that provides services, data, or resources to other computers, called clients, over a network. Servers are designed to handle requests continuously and are commonly used for hosting websites, storing data, and running applications.

The **HTTP request-response cycle** describes how a client communicates with a server:

1. **Request (Client to Server):**  
    The client sends an HTTP request to the server. This request includes:
   - **Method:** The action being requested (GET to retrieve data, POST to send data)
   - **Endpoint:** The specific URL or path being accessed (`/users`, `/login`)
   - **Headers:** Additional information about the request (content type, authorization)
   - **Body:** Optional data sent with the request (POST or PUT)
2. **Response (Server to Client):**  
    The server processes the request and sends back a response, which includes:
   - **Status Code:** Indicates the result ( 200 for success, 404 for not found, 500 for server error)
   - **Headers:** Metadata about the response ( content type, caching rules)
   - **Body:** The actual data returned ( HTML, JSON)

Think of this like ordering food at a restaurant:

- You look at the menu and place an order with a waiter

- Your **order** is the request:
  - Method \= what you want to do (order food \=\> POST, ask for menu \=\> GET)

  - Endpoint \= the specific item (burger, pizza)

  - Headers \= preferences (no onions, extra cheese)

  - Body \= details of your order

- The kitchen prepares your food and sends it back through the waiter:
  - Status Code \= whether your order was successful (200 \= served, 404 \= item unavailable)

  - Headers \= extra info (plate is hot, allergies)

  - Body \= the actual food

## Question 2: Middleware

What is middleware in Express? How does it differ from a regular controller? Explain the role of `next()` and provide an example of when middleware is useful.

**Your Answer:**

In **Express**, **middleware** is a function that runs during the request-response cycle and has access to the request (`req`), response (`res`), and the `next` function. Middleware is used to process requests before they reach the final route handler, such as logging, authentication, or parsing data. The difference between **middleware** and a **controller** is that middleware sits _in the middle_ of the request flow and can modify the request or stop it early, while a controller is typically the final function that handles the request and sends back a response. The `next()` function is used to pass control to the next middleware or route handler in the stack. If `next()` is not called, the request will hang unless a response is sent. An example of useful middleware is authentication. For instance, a middleware function can check if a user is logged in before allowing access to a protected route. If the user is not authenticated, it can return an error response instead of calling `next()`, preventing the controller from running.

## Question 3: API Key Security

Why is it dangerous to use API keys in client-side (frontend) code? Explain how a backend server solves this problem (the "proxy" pattern). Include what role environment variables (`.env`) play in this approach.

**Your Answer:**

It is dangerous to use API keys in client-side code because anyone can inspect the browser using developer tools and easily access those keys. A user could then steal the key, make unauthorized requests or exhaust your API quota. A backend server solves this problem using the **proxy pattern**. Instead of the frontend calling the external API directly, it sends a request to your backend. The backend securely stores the API key and makes the request to the external API on behalf of the client, then returns the data. This way, the API key is never exposed to the public. Environment variables, like those stored in a `.env` file, are used on the backend to keep sensitive information such as API keys secure. They allow you to store secrets outside of your source code, so they aren’t exposed.

## Question 4: Debugging a Server

A fellow student is building an Express server. They send a `PATCH` request to `/api/bookmarks/1` using Postman, but they receive a `404` status code. List at least three things you would check to debug this issue and explain why each one could be the cause of the problem.

**Your Answer:**

If a `PATCH` request to `/api/bookmarks/1` returns a `404`, I would check:

- First, verify that the **route is defined correctly** in Express (`app.patch('/api/bookmarks/:id', ...)`). If the route path or method doesn’t exactly match the request, Express won’t find it and will return a 404\.
- Second, check that the **server is running on the correct port** and that Postman is sending the request to the right URL (`http://localhost:8080`). A mismatch in port can cause the request to hit a non-existent route.
- Third, confirm that the **router is properly mounted** (`app.use('/api/bookmarks', bookmarksRouter)`). If the router isn’t connected to the main app the endpoint won’t be reachable.

Finally, check if there is any **middleware returning a 404 early**, such as a “not found” handler or a condition that exits before reaching the controller.
