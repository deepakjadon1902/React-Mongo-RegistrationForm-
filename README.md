React:(App.tsx):-
import "./App.css";
import { BrowserRouter, Route, Routes } from "react-router-dom";
import LoginForm from "./login";
import Table from "./table";
import HomePage  from "./home";
import Counter from "./counter";
import Register from "./register";

const App = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/home" element={<HomePage />} />
        <Route path="/login" element={<LoginForm />} />
        <Route path="/counter" element={<Counter />} />
        <Route path="/table" element={<Table />} />
        <Route path="/register" element={<Register />} />
      </Routes>
    </BrowserRouter>
  );
};
export default App;


(Regitration.tsx):-
import { useState } from "react";
import './axios'
import api from "./axios";
const Register = (props) => {
  const [message, setMessage] = useState(false);
  const [data, setData] = useState({ 
    username: "", 
    email: "", 
    number: "", 
    password: "", 
    confirmPassword: "" 
  });
  const [error, setError] = useState(null);

  const updateData = (type, event) => {
    setData({ ...data, [type]: event.target.value });
  };

  const handleRegister = async (e) => {
    e.preventDefault();
    const response = await api.post("/register",data )
    console.log(data);
    const result = await response.data;
    console.log('result: ', result)
    setMessage("Registration successful!");
    console.log(result);
  };

  return (
    <div style={{ padding: "20px", textAlign: "center" }}>
      <h1>Register Form</h1>
      <div>
        <label>
          Username:
          <input
            type="text"
            value={data.username}
            onChange={(e) => updateData("username", e)}
          />
        </label>
      </div>
      <br />
      <div>
        <label>
          Email:
          <input
            type="email"
            value={data.email}
            onChange={(e) => updateData("email", e)}
          />
        </label>
      </div>
      <br />
      <div>
        <label>
          Number:
          <input
            type="number"
            value={data.number}
            onChange={(e) => updateData("number", e)}
          />
        </label>
      </div>
      <br />
      <div>
        <label>
          Password:
          <input
            type="password"
            value={data.password}
            onChange={(e) => updateData("password", e)}
          />
        </label>
      </div>
      <br />
      {error && <p style={{ color: "red" }}>{error}</p>}
      {message && <p style={{ color: "green" }}>{message}</p>}
      <button onClick={handleRegister}>Register</button>
    </div>
  );
};
export default Register;


(axios.tsx):-
import axios from 'axios';
const api = axios.create({
    baseURL:"http://127.0.0.1:5000",
    headers:{
        "content-type":"application/json"
    },
});
export default api;

(App.css):-
#root {
    max-width: 1280px;
    margin: 0 auto;
    padding: 2rem;
    text-align: center;
  } 
  body {
    font-family: sans-serif;
    background-color: #1234f7;
  }
  
  .container {
    width: 500px;
    margin: 0 auto;
    padding: 20px;
    background-color: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
    text-align: center;
    margin-bottom: 20px;
  }
  
  label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
  }
  
  input[type="text"],
  input[type="password"] {
    width: 100%;
    padding: 10px;
    margin-bottom: 10px;
    border: 1px solid #ddd;
    border-radius: 3px;
  }
  
  button {
    background-color: #4CAF50;
    color: white;
    padding: 10px 20px;
    border: none;
    border-radius: 3px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: #45a049;
  }



  MongoDb(hello.py):-
  



from flask import Flask,request
from pymongo import MongoClient


from flask_cors import CORS   #pip install flask_cors

app = Flask(__name__)

cors=CORS(app,resources={r"/*":{"origins":"*"}})

client=MongoClient('mongodb://localhost:27017/')

mdb=client['pro456']


@app.route("/register", methods=['POST'])
def register():
    input_data = request.get_json()
    name = input_data.get('username')
    email = input_data.get('email')
    password = input_data.get('password')
    user = mdb.users.find_one({'email':email})
    print(user)
    if user and '_id' in user:
        return {
            'status': 0,
            'msg':"user exit",
            'class':"success"
        }
    count = mdb.users.count_documents({})
    users={
        "_id":count+1,
        "name":name,
        "email":email,
        "password":password,
    }
    mdb.users.insert_one(users)
    return "inserted successfully"








  
