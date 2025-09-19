Perfect 👌 You’ll get to see the **full cycle**: build a small frontend → dockerize it → run the container → open in browser. Let’s do a **minimal React app** example (since you’re already learning React).

---

## 🖥️ Step 1: Create a simple frontend app

Directory structure:

```
frontend-app/
  ├── public/
  │   └── index.html
  ├── src/
  │   └── App.js
  │   └── index.js
  ├── package.json
```

**package.json**

```json
{
  "name": "frontend-app",
  "version": "1.0.0",
  "private": true,
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-scripts": "5.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build"
  }
}
```

**src/App.js**

```jsx
function App() {
  return (
    <div style={{ textAlign: "center", marginTop: "50px" }}>
      <h1>Hello from Dockerized React App 🚀</h1>
    </div>
  );
}
export default App;
```

**src/index.js**

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(<App />);
```

**public/index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Frontend App</title>
  </head>
  <body>
    <div id="root"></div>
  </body>
</html>
```

---

## 🐳 Step 2: Create Dockerfile

Put this `Dockerfile` in the root of `frontend-app/`.

```dockerfile
# Step 1: Build React app
FROM node:18 AS build
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build

# Step 2: Serve using nginx
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

---

## 🛠️ Step 3: Build Docker image

Inside `frontend-app/` directory:

```bash
docker build -t my-frontend-app .
```

---

## 🚀 Step 4: Run container

```bash
docker run -d -p 8080:80 my-frontend-app
```

Here:

* `-d` → run in background
* `-p 8080:80` → map host port 8080 → container port 80

---

## 🌍 Step 5: Open in browser

Go to:

```
http://localhost:8080
```

You should see:

```
Hello from Dockerized React App 🚀
```

---

👉 This is the **production build approach** (React bundled into static files served by NGINX).
If you want a **development container** (with live reload via `npm start`), that would use a different Dockerfile.

Do you want me to also show you the **dev-mode Dockerfile** where you can code and see instant changes without rebuilding the image?
