# projectgitfirst
sample project 
Option 1: Using Create React App (CRA) – Quick Setup

This is the easiest way for beginners. It sets up React with everything (Webpack, Babel, etc.).

Make sure Node.js is installed

Check:

node -v
npm -v


If not installed, download from 👉 https://nodejs.org

Open a terminal / command prompt and run:

npx create-react-app my-app


(Here my-app is your project folder name)

Move into the project:

cd my-app


Start the app:

npm start


Opens at http://localhost:3000 🎉

Option 2: Using Vite (Faster & Modern)

CRA is older and slower; many devs now prefer Vite.

In terminal:

npm create vite@latest my-app


Choose:

Framework: React

Variant: JavaScript or TypeScript

Move into the project:

cd my-app


Install dependencies:

npm install


Run:

npm run dev


App runs at http://localhost:5173
