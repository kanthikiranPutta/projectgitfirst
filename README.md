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

export default function Dashboard({ onLogout }) {
  return (
    <div className="container">
      <header className="app-header">
        <h1>Dashboard</h1>
        <div>
          <button className="btn-logout" onClick={onLogout}>Logout</button>
        </div>
      </header>

      <div className="tiles">
        <Link to="/contact" className="tile">
          <div className="tile-icon">✍️</div>
          <div className="tile-title">Contact Form</div>
          <div className="tile-desc">Create name, address and phone entries</div>
        </Link>

        <Link to="/entries" className="tile">
          <div className="tile-icon">📋</div>
          <div className="tile-title">Entries</div>
          <div className="tile-desc">View submitted entries</div>
        </Link>

        <Link to="/upload" className="tile">
          <div className="tile-icon">🔒</div>
          <div className="tile-title">Upload CSV</div>
          <div className="tile-desc">Profile Upload</div>
        </Link>

        <Link to="/reports" className="tile">
          <div className="tile-icon">📊</div>
          <div className="tile-title">Reports</div>
          <div className="tile-desc">Export & analytics</div>
        </Link>
      </div>
    </div>
  );
}


import React from 'react';
import { Link } from 'react-router-dom';

export default function EntriesList({ entries, onEdit, onDelete }) {
  return (
    <div className="container">
      <header className="app-header">
        <h1>Entries</h1>
        <Link to="/" className="btn-back">Back</Link>
      </header>

      <div className="list-card" style={{ backgroundColor: '#E6E6FA' }}>
        {entries.length === 0 ? (
          <p>No entries yet.</p>
        ) : (
          <table className="entries-table">
            <thead><tr><th>Name</th><th>Address</th><th>Phone</th><th>Designation</th></tr></thead>
            <tbody>
              {entries.map((e, idx) => (
                <tr key={idx}>
                  <td>{e.name}</td>
                  <td>{e.address}</td>
                  <td>{e.phone}</td>
                  <td>{e.designation}</td>
                  <td>
                  <button onClick={() => onEdit(idx)}>Edit</button>
                  <button
                    onClick={() => onDelete(idx)}
                    style={{ marginLeft: "10px", color: "white", backgroundColor: "red" }}
                  >
                    Delete
                  </button>
                </td>
                </tr>
              ))}
            </tbody>
          </table>
        )}
      </div>
    </div>
  );
}



import React, { Component } from 'react';
import { useNavigate } from 'react-router-dom';

class LoginPageInner extends Component {
  constructor(props) {
    super(props);
    this.state = { username: '', password: '', error: '' };
  }

  handleChange = (e) => {
    this.setState({ [e.target.name]: e.target.value, error: '' });
  }

  handleSubmit = (e) => {
    e.preventDefault();
    const { username, password } = this.state;
    const ok = this.props.onLogin(username, password);
    if (ok) {
      this.props.navigate('/');
    } else {
      this.setState({ error: 'Invalid credentials' });
    }
  }

  render() {
    return (
      <div className="center-container">
        <div className="card login-card">
          <h2>Sign In</h2>
          <form onSubmit={this.handleSubmit}>
            <label>Username</label>
            <input name="username" value={this.state.username} onChange={this.handleChange} />
            <label>Password</label>
            <input name="password" type="password" value={this.state.password} onChange={this.handleChange} />
            <button type="submit">Login</button>
            {this.state.error && <div className="error">{this.state.error}</div>}
          </form>
        </div>
      </div>
    );
  }
}

// wrapper to use navigate hook
export default function LoginPage(props) {
  const navigate = useNavigate();
  return <LoginPageInner {...props} navigate={navigate} />;
}



