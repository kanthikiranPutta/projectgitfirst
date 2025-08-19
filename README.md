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




Step 1: Install Node.js

Download LTS version from 👉 https://nodejs.org.

Verify installation in VS Code Terminal:

node -v
npm -v


(If you see versions, Node.js + npm are installed.)

🔹 Step 2: Create React Project with Vite

Open VS Code.

Open the terminal in VS Code:
Ctrl + ~ (or Terminal → New Terminal).

Run:

npm create vite@latest my-app


It will ask:

Project name: my-app

Framework: React

Variant: JavaScript (or TypeScript if you prefer)

🔹 Step 3: Install Dependencies

Go inside your project and install:

cd my-app
npm install

🔹 Step 4: Run the App

Start the development server:

npm run dev


You’ll see something like:

  Local:   http://localhost:5173/


👉 Open that link in your browser, you’ll see your React app running 🚀.

🔹 Step 5: Open Project in VS Code

Inside VS Code → File → Open Folder → select my-app.

You’ll see folders:

src/ → Your React code

App.jsx → Main component

main.jsx → Entry point


import React from 'react';
import { Link } from 'react-router-dom';
import AddressPieChart from "./AddressPieChart";
export default function Reports({ entries }) {
  // Generate CSV from entries
  const exportToCSV = () => {
    if (entries.length === 0) return;

    const headers = ["Name", "Address", "Phone","Designation"];
    const csvRows = [
      headers.join(","), // header row
      ...entries.map(e => [e.name, e.address, e.phone,e.designation].join(","))
    ];

    const csvString = csvRows.join("\n");
    const blob = new Blob([csvString], { type: "text/csv" });
    const url = window.URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = "entries.csv";
    a.click();
    window.URL.revokeObjectURL(url);
  };

  return (
    <div className="container">
      <header className="app-header">
        <h1>Reports & Analytics</h1>
        <Link to="/" className="btn-back">Back</Link>
      </header>

      <div className="card">
        <h3>Entries Summary</h3>
        <p>Total entries: {entries.length}</p>
        {/* Example: unique names */}
        <p>Unique names: {new Set(entries.map(e => e.name)).size}</p>

        <button onClick={exportToCSV}>Export to CSV</button>
      </div>
      <h3>Address Distribution</h3>
      <AddressPieChart entries={entries} />
    </div>
  );
}



* { box-sizing: border-box; font-family: Arial, sans-serif; }
body { margin: 0; background: #f4f6f8; color: #222; }
.container { max-width: 900px; margin: 20px auto; padding: 10px; }
.app-header { display:flex; justify-content:space-between; align-items:center; padding:10px 0; }
.tiles { display:grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap:16px; margin-top:20px; }
.tile { display:flex; flex-direction:column; align-items:flex-start; padding:20px; background:#fff; border-radius:8px; text-decoration:none; color:inherit; box-shadow:0 2px 6px rgba(0,0,0,0.08); transition:transform .15s; }
.tile:hover { transform: translateY(-6px); }
.tile-icon { font-size:28px; margin-bottom:8px; }
.tile-title { font-weight:700; margin-bottom:6px; }
.tile-desc { color:#555; font-size:14px; }
.card { background:#fff; padding:20px; border-radius:8px; box-shadow:0 2px 6px rgba(0,0,0,0.06); width:100%; max-width:520px; }
.login-card { width:320px; }
.center-container { display:flex; justify-content:center; align-items:center; height:70vh; }
input { width:100%; padding:8px 10px; margin:6px 0 12px 0; border:1px solid #ddd; border-radius:4px; }
button { padding:10px 14px; background:#2b7cff; color:#fff; border:none; border-radius:4px; cursor:pointer; }
button:hover { opacity:0.95; }
.error { color:#b00020; margin-top:8px; }
.btn-logout { background:#ef4444; }
.btn-back { padding:8px 12px; background:#f3f4f6; border-radius:4px; text-decoration:none; color:#333; }
.list-card { margin-top:20px; background:#fff; padding:16px; border-radius:8px; box-shadow:0 2px 6px rgba(0,0,0,0.06); }
.entries-table { width:100%; border-collapse:collapse; }
.entries-table th, .entries-table td { border:1px solid #eee; padding:8px; text-align:left; }
.card-yellow {
  background-color: #E6E6FA;
}
.btn-back {
  color: red;
  text-decoration: none;
  font-weight: bold;

import React, { useState } from 'react';
import { Link } from 'react-router-dom';

export default function Reports({ entries, setEntries }) {
  const [uploadedCount, setUploadedCount] = useState(0);

  const exportToCSV = () => {
    if (entries.length === 0) return;

    const headers = ["Name", "Address", "Phone","Designation"];
    const csvRows = [
      headers.join(","),
      ...entries.map(e => [e.name, e.address, e.phone, e.designation].join(","))
    ];

    const blob = new Blob([csvRows.join("\n")], { type: "text/csv" });
    const url = window.URL.createObjectURL(blob);

    const a = document.createElement("a");
    a.href = url;
    a.download = "entries.csv";
    a.click();
    window.URL.revokeObjectURL(url);
  };

  const handleFileUpload = (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = function(event) {
      const text = event.target.result;
      const rows = text.split("\n").slice(1); // skip header row
      const newEntries = rows
        .map(row => row.split(","))
        .filter(cols => cols.length >= 3)
        .map(([name, address, phone, designation]) => ({
          name: name.trim(),
          address: address.trim(),
          phone: phone.trim(),
          designation: designation.trim()
        }));

      setEntries(prev => [...prev, ...newEntries]);
      setUploadedCount(newEntries.length);
    };
    reader.readAsText(file);
  };

  return (
    <div className="container">
      <header className="app-header">
        <h1>Reports & Analytics</h1>
        <Link to="/" className="btn-back">Back</Link>
      </header>

      <div className="card">
        <h3>Entries Summary</h3>
        <p>Total entries: {entries.length}</p>
        <p>Unique names: {new Set(entries.map(e => e.name)).size}</p>
        {uploadedCount > 0 && <p>Uploaded {uploadedCount} entries.</p>}

        <div style={{ marginTop: "10px" }}>
          <label>Upload CSV: </label>
          <input type="file" accept=".csv" onChange={handleFileUpload} />
        </div>

        <button style={{ marginTop: "10px" }} onClick={exportToCSV}>
          Export to CSV
        </button>
      </div>
    </div>
  );
}






