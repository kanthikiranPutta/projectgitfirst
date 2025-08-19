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

import React from "react";
import { PieChart, Pie, Cell, Tooltip, Legend } from "recharts";

export default function AddressPieChart({ entries }) {
  // Count entries by address
  const addressCounts = entries.reduce((acc, entry) => {
    acc[entry.address] = (acc[entry.address] || 0) + 1;
    return acc;
  }, {});

  const data = Object.entries(addressCounts).map(([address, count]) => ({
    name: address,
    value: count
  }));

  const COLORS = ["#8884d8", "#82ca9d", "#ffc658", "#ff7f50", "#d0ed57"];

  return (
    <PieChart width={400} height={300}>
      <Pie
        data={data}
        cx="50%"
        cy="50%"
        labelLine={false}
        outerRadius={100}
        fill="#8884d8"
        dataKey="value"
        label={({ name, value }) => `${name} (${value})`}
      >
        {data.map((entry, index) => (
          <Cell key={index} fill={COLORS[index % COLORS.length]} />
        ))}
      </Pie>
      <Tooltip />
      <Legend />
    </PieChart>
  );
}

import React, { Component } from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import LoginPage from './LoginPage';
import Dashboard from './Dashboard';
import ContactForm from './ContactForm';
import EntriesList from './EntriesList';
import Reports from './Reports';
import Upload from './Upload';

class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      isLoggedIn: false,
      currentUser: null,
      entries: []
    };
  }

  handleLogin = (username, password) => {
    // simple check - in real apps use API
    if ((username === 'admin' && password === 'admin123') || (username === 'user' && password === 'user123')) {
      this.setState({ isLoggedIn: true, currentUser: { username } });
      return true;
    }
    return false;
  }

  handleLogout = () => {
    this.setState({ isLoggedIn: false, currentUser: null });
  }

  addEntry = (entry) => {
    this.setState(prev => ({ entries: [...prev.entries, entry] }));
  }

  handleEdit = (idx) => {
  const name = prompt("Enter new name", this.state.entries[idx].name);
  const address = prompt("Enter new address", this.state.entries[idx].address);
  const phone = prompt("Enter new phone", this.state.entries[idx].phone);
  const designation = prompt("Enter new designation", this.state.entries[idx].designation);

  if (name && address && phone) {
    const updatedEntries = [...this.state.entries];
    updatedEntries[idx] = { name, address, phone, designation };
    this.setState({ entries: updatedEntries });
  }
};

handleDelete = (idx) => {
  if (window.confirm("Are you sure you want to delete this entry?")) {
    const updatedEntries = this.state.entries.filter((_, i) => i !== idx);
    this.setState({ entries: updatedEntries });
  }
};

  render() {
    const { isLoggedIn } = this.state;
    return (
      <Router>
        <Routes>
          <Route path="/login" element={<LoginPage onLogin={this.handleLogin} />} />
          <Route path="/" element={isLoggedIn ? <Dashboard onLogout={this.handleLogout} /> : <Navigate to="/login" replace />} />
          <Route path="/contact" element={isLoggedIn ? <ContactForm addEntry={this.addEntry} /> : <Navigate to="/login" replace />} />
          <Route path="/entries" element={isLoggedIn ? <EntriesList entries={this.state.entries} onEdit={this.handleEdit} onDelete={this.handleDelete}/> : <Navigate to="/login" replace />} />
          <Route path="/reports" element={isLoggedIn ? <Reports entries={this.state.entries} /> : <Navigate to="/login" replace />} />
          <Route path="/upload" element={isLoggedIn ? <Upload entries={this.state.entries} setEntries={(fn) => this.setState({ entries: fn(this.state.entries) })} /> : <Navigate to="/login" replace />} />
        </Routes>
      </Router>
    );
  }
}

export default App;



import React, { Component } from 'react';
import { useNavigate } from 'react-router-dom';
import { Link } from 'react-router-dom';
class ContactFormInner extends Component {
  constructor(props) {
    super(props);
    this.state = { name: '', address: '', phone: '',designation:'' };
  }

  handleChange = (e) => this.setState({ [e.target.name]: e.target.value });

  handleSubmit = (e) => {
    e.preventDefault();
    const { name, address, phone, designation } = this.state;
    if (name && address && phone && designation) {
      this.props.addEntry({ name, address, phone, designation });
      this.props.navigate('/entries');
    }
  }

  render() {
    return (     

      <div className="center-container">        
        <div className="card" style={{ backgroundColor: '#E6E6FA' }}>
          <h2>Contact Form</h2>
          <form onSubmit={this.handleSubmit}>
            <label>Name</label>
            <input name="name" value={this.state.name} onChange={this.handleChange} />
            <label>Address</label>
            <input name="address" value={this.state.address} onChange={this.handleChange} />
            <label>Phone</label>
            <input name="phone" value={this.state.phone} onChange={this.handleChange} />
            <label>Designation</label>
            <input name="designation" value={this.state.designation} onChange={this.handleChange} />
            <button type="submit">Submit</button>
          </form>
          <div>
             <footer className="app-footer">
                <h1><br></br></h1>
                <Link to="/" className="btn-back" style={{ color: 'red' }}>Back</Link>
              </footer>
              </div>
          </div>
        </div>
           
    );
  }
}

export default function ContactForm(props) {
  const navigate = useNavigate();
  return <ContactFormInner {...props} navigate={navigate} />;
}




