# Complete Frontend Development Guide - Prescripto Doctor Booking System

A comprehensive step-by-step guide to build the Prescripto frontend from scratch using React.js, Vite, and Tailwind CSS.

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Prerequisites](#prerequisites)
3. [Initial Setup](#initial-setup)
4. [Project Structure](#project-structure)
5. [Core Configuration](#core-configuration)
6. [Building Components](#building-components)
7. [State Management](#state-management)
8. [Routing Setup](#routing-setup)
9. [API Integration](#api-integration)
10. [Authentication Implementation](#authentication-implementation)
11. [Payment Integration](#payment-integration)
12. [Testing](#testing)
13. [Deployment](#deployment)

---

## 🎯 Project Overview

We'll build a modern, responsive doctor booking frontend with:
- **React.js 18** with functional components and hooks
- **Vite** for fast development and building
- **Tailwind CSS** for styling
- **React Router** for navigation
- **Axios** for API calls
- **Context API** for state management
- **React Toastify** for notifications

### Features to Implement:
- User authentication (login/register)
- Doctor listing and filtering
- Appointment booking system
- User profile management
- Responsive design
- Payment integration

---

## 🔧 Prerequisites

Before starting, ensure you have:

```bash
# Check Node.js version (v14 or higher required)
node --version

# Check npm version
npm --version

# Install Git if not already installed
git --version
```

### Required Knowledge:
- Basic JavaScript ES6+ syntax
- React.js fundamentals
- CSS/HTML basics
- API integration concepts

---

## 🚀 Initial Setup

### Step 1: Create Vite React Project

```bash
# Create new Vite project
npm create vite@latest prescripto-frontend -- --template react

# Navigate to project directory
cd prescripto-frontend

# Install dependencies
npm install
```

### Step 2: Install Required Dependencies

```bash
# Core dependencies
npm install react-router-dom axios react-toastify

# Tailwind CSS setup
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

### Step 3: Configure Tailwind CSS

**tailwind.config.js**
```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      gridTemplateColumns: {
        'auto': 'repeat(auto-fill, minmax(200px, 1fr))'
      },
      colors: {
        'primary': '#5F6FFF'
      }
    },
  },
  plugins: [],
}
```

**src/index.css**
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Custom styles */
body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', 'Oxygen',
    'Ubuntu', 'Cantarell', 'Fira Sans', 'Droid Sans', 'Helvetica Neue',
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

* {
  box-sizing: border-box;
}
```

### Step 4: Environment Setup

**Create .env file in root directory:**
```env
VITE_BACKEND_URL=http://localhost:4000
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
```

---

## 📁 Project Structure

Create the following folder structure:

```
src/
├── components/          # Reusable components
│   ├── Navbar.jsx
│   ├── Footer.jsx
│   ├── Header.jsx
│   ├── Banner.jsx
│   └── RelatedDoctors.jsx
├── pages/              # Page components
│   ├── Home.jsx
│   ├── Doctors.jsx
│   ├── Login.jsx
│   ├── About.jsx
│   ├── Contact.jsx
│   ├── Appointment.jsx
│   ├── MyAppointments.jsx
│   ├── MyProfile.jsx
│   └── Verify.jsx
├── context/            # Context providers
│   └── AppContext.jsx
├── assets/             # Static assets
│   ├── assets.js       # Asset imports
│   └── images/         # Image files
├── utils/              # Utility functions
│   └── helpers.js
├── hooks/              # Custom hooks
│   └── useAuth.js
├── App.jsx             # Main app component
└── main.jsx           # Entry point
```

---

## ⚙️ Core Configuration

### Step 1: Update main.jsx

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import { BrowserRouter } from 'react-router-dom'
import App from './App.jsx'
import './index.css'
import AppContextProvider from './context/AppContext.jsx'

ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <AppContextProvider>
      <App />
    </AppContextProvider>
  </BrowserRouter>
)
```

### Step 2: Create Assets Management

**src/assets/assets.js**
```javascript
// Import all assets
import logo from './logo.svg'
import dropdown_icon from './dropdown_icon.svg'
import menu_icon from './menu_icon.svg'
import cross_icon from './cross_icon.png'
import header_img from './header_img.png'
import appointment_img from './appointment_img.png'
import about_image from './about_image.png'
import contact_image from './contact_image.png'
import profile_pic from './profile_pic.png'
import arrow_icon from './arrow_icon.svg'
import info_icon from './info_icon.svg'
import upload_icon from './upload_icon.png'
import stripe_logo from './stripe_logo.png'
import razorpay_logo from './razorpay_logo.png'
import verified_icon from './verified_icon.svg'

// Doctor images
import doc1 from './doc1.png'
import doc2 from './doc2.png'
import doc3 from './doc3.png'
// ... add more doctors as needed

// Specialty icons
import General_physician from './General_physician.svg'
import Gynecologist from './Gynecologist.svg'
import Dermatologist from './Dermatologist.svg'
import Pediatricians from './Pediatricians.svg'
import Neurologist from './Neurologist.svg'
import Gastroenterologist from './Gastroenterologist.svg'

export const assets = {
  logo,
  dropdown_icon,
  menu_icon,
  cross_icon,
  header_img,
  appointment_img,
  about_image,
  contact_image,
  profile_pic,
  arrow_icon,
  info_icon,
  upload_icon,
  stripe_logo,
  razorpay_logo,
  verified_icon
}

export const doctors = [
  {
    _id: 'doc1',
    name: 'Dr. Richard James',
    image: doc1,
    speciality: 'General physician',
    degree: 'MBBS',
    experience: '4 Years',
    about: 'Dr. Davis has a strong commitment to delivering comprehensive medical care...',
    fees: 50,
    address: {
      line1: '17th Cross, Richmond',
      line2: 'Circle, Ring Road, London'
    }
  },
  // Add more doctors...
]

export const specialityData = [
  {
    speciality: 'General physician',
    image: General_physician
  },
  {
    speciality: 'Gynecologist',
    image: Gynecologist
  },
  {
    speciality: 'Dermatologist',
    image: Dermatologist
  },
  {
    speciality: 'Pediatricians',
    image: Pediatricians
  },
  {
    speciality: 'Neurologist',
    image: Neurologist
  },
  {
    speciality: 'Gastroenterologist',
    image: Gastroenterologist
  }
]
```

---

## 🔄 State Management

### Step 1: Create AppContext

**src/context/AppContext.jsx**
```jsx
import { createContext, useEffect, useState } from "react"
import { toast } from "react-toastify"
import axios from 'axios'

export const AppContext = createContext()

const AppContextProvider = (props) => {
  const currencySymbol = '₹'
  const backendUrl = import.meta.env.VITE_BACKEND_URL

  const [doctors, setDoctors] = useState([])
  const [token, setToken] = useState(localStorage.getItem('token') || '')
  const [userData, setUserData] = useState(false)

  // Get doctors data from API
  const getDoctorsData = async () => {
    try {
      const { data } = await axios.get(`${backendUrl}/api/doctor/list`)
      if (data.success) {
        setDoctors(data.doctors)
      } else {
        toast.error(data.message)
      }
    } catch (error) {
      console.error('Error fetching doctors:', error)
      toast.error('Failed to load doctors')
    }
  }

  // Load user profile data
  const loadUserProfileData = async () => {
    try {
      const { data } = await axios.get(`${backendUrl}/api/user/get-profile`, {
        headers: { token }
      })
      if (data.success) {
        setUserData(data.userData)
      } else {
        toast.error(data.message)
      }
    } catch (error) {
      console.error('Error loading profile:', error)
      toast.error('Failed to load profile')
    }
  }

  // Context value
  const value = {
    doctors,
    getDoctorsData,
    currencySymbol,
    backendUrl,
    token,
    setToken,
    userData,
    setUserData,
    loadUserProfileData
  }

  useEffect(() => {
    getDoctorsData()
  }, [])

  useEffect(() => {
    if (token) {
      loadUserProfileData()
    } else {
      setUserData(false)
    }
  }, [token])

  return (
    <AppContext.Provider value={value}>
      {props.children}
    </AppContext.Provider>
  )
}

export default AppContextProvider
```

---

## 🧩 Building Components

### Step 1: Navbar Component

**src/components/Navbar.jsx**
```jsx
import React, { useContext, useState } from 'react'
import { assets } from '../assets/assets'
import { NavLink, useNavigate } from 'react-router-dom'
import { AppContext } from '../context/AppContext'

const Navbar = () => {
  const navigate = useNavigate()
  const [showMenu, setShowMenu] = useState(false)
  const { token, setToken, userData } = useContext(AppContext)

  const logout = () => {
    localStorage.removeItem('token')
    setToken('')
    navigate('/login')
  }

  return (
    <div className='flex items-center justify-between text-sm py-4 mb-5 border-b border-b-[#ADADAD]'>
      {/* Logo */}
      <img 
        onClick={() => navigate('/')} 
        className='w-44 cursor-pointer' 
        src={assets.logo} 
        alt="Prescripto Logo" 
      />
      
      {/* Desktop Navigation */}
      <ul className='md:flex items-start gap-5 font-medium hidden'>
        <NavLink to='/' className={({ isActive }) => 
          `py-1 ${isActive ? 'text-primary' : 'text-gray-700'}`
        }>
          <li className='py-1'>HOME</li>
          <hr className='border-none outline-none h-0.5 bg-primary w-3/5 m-auto hidden' />
        </NavLink>
        <NavLink to='/doctors' className={({ isActive }) => 
          `py-1 ${isActive ? 'text-primary' : 'text-gray-700'}`
        }>
          <li className='py-1'>ALL DOCTORS</li>
          <hr className='border-none outline-none h-0.5 bg-primary w-3/5 m-auto hidden' />
        </NavLink>
        <NavLink to='/about' className={({ isActive }) => 
          `py-1 ${isActive ? 'text-primary' : 'text-gray-700'}`
        }>
          <li className='py-1'>ABOUT</li>
          <hr className='border-none outline-none h-0.5 bg-primary w-3/5 m-auto hidden' />
        </NavLink>
        <NavLink to='/contact' className={({ isActive }) => 
          `py-1 ${isActive ? 'text-primary' : 'text-gray-700'}`
        }>
          <li className='py-1'>CONTACT</li>
          <hr className='border-none outline-none h-0.5 bg-primary w-3/5 m-auto hidden' />
        </NavLink>
      </ul>

      {/* User Profile / Login Button */}
      <div className='flex items-center gap-4'>
        {token && userData ? (
          <div className='flex items-center gap-2 cursor-pointer group relative'>
            <img 
              className='w-8 rounded-full' 
              src={userData.image || assets.profile_pic} 
              alt="Profile" 
            />
            <img 
              className='w-2.5' 
              src={assets.dropdown_icon} 
              alt="Dropdown" 
            />
            <div className='absolute top-0 right-0 pt-14 text-base font-medium text-gray-600 z-20 hidden group-hover:block'>
              <div className='min-w-48 bg-gray-50 rounded flex flex-col gap-4 p-4'>
                <p 
                  onClick={() => navigate('/my-profile')} 
                  className='hover:text-black cursor-pointer'
                >
                  My Profile
                </p>
                <p 
                  onClick={() => navigate('/my-appointments')} 
                  className='hover:text-black cursor-pointer'
                >
                  My Appointments
                </p>
                <p 
                  onClick={logout} 
                  className='hover:text-black cursor-pointer'
                >
                  Logout
                </p>
              </div>
            </div>
          </div>
        ) : (
          <button 
            onClick={() => navigate('/login')} 
            className='bg-primary text-white px-8 py-3 rounded-full font-light hidden md:block'
          >
            Create account
          </button>
        )}

        {/* Mobile Menu Icon */}
        <img 
          onClick={() => setShowMenu(true)} 
          className='w-6 md:hidden' 
          src={assets.menu_icon} 
          alt="Menu" 
        />

        {/* Mobile Menu */}
        <div className={`md:hidden ${showMenu ? 'fixed w-full' : 'h-0 w-0'} right-0 top-0 bottom-0 z-20 overflow-hidden bg-white transition-all`}>
          <div className='flex items-center justify-between px-5 py-6'>
            <img src={assets.logo} className='w-36' alt="Logo" />
            <img 
              onClick={() => setShowMenu(false)} 
              src={assets.cross_icon} 
              className='w-7 cursor-pointer' 
              alt="Close" 
            />
          </div>
          <ul className='flex flex-col items-center gap-2 mt-5 px-5 text-lg font-medium'>
            <NavLink onClick={() => setShowMenu(false)} to='/'>
              <p className='px-4 py-2 rounded inline-block'>HOME</p>
            </NavLink>
            <NavLink onClick={() => setShowMenu(false)} to='/doctors'>
              <p className='px-4 py-2 rounded inline-block'>ALL DOCTORS</p>
            </NavLink>
            <NavLink onClick={() => setShowMenu(false)} to='/about'>
              <p className='px-4 py-2 rounded inline-block'>ABOUT</p>
            </NavLink>
            <NavLink onClick={() => setShowMenu(false)} to='/contact'>
              <p className='px-4 py-2 rounded inline-block'>CONTACT</p>
            </NavLink>
          </ul>
        </div>
      </div>
    </div>
  )
}

export default Navbar
```

### Step 2: Header Component

**src/components/Header.jsx**
```jsx
import React from 'react'
import { assets } from '../assets/assets'

const Header = () => {
  return (
    <div className='flex flex-col md:flex-row flex-wrap bg-primary rounded-lg px-6 md:px-10 lg:px-20'>
      {/* Left Side */}
      <div className='md:w-1/2 flex flex-col items-start justify-center gap-4 py-10 m-auto md:py-[10vw] md:mb-[-30px]'>
        <p className='text-3xl md:text-4xl lg:text-5xl text-white font-semibold leading-tight md:leading-tight lg:leading-tight'>
          Book Appointment <br />
          With Trusted Doctors
        </p>
        <div className='flex flex-col md:flex-row items-center gap-3 text-white text-sm font-light'>
          <img className='w-28' src={assets.group_profiles} alt="Group Profiles" />
          <p>
            Simply browse through our extensive list of trusted doctors, <br className='hidden sm:block' />
            schedule your appointment hassle-free.
          </p>
        </div>
        <a 
          href='#speciality' 
          className='flex items-center gap-2 bg-white px-8 py-3 rounded-full text-gray-600 text-sm m-auto md:m-0 hover:scale-105 transition-all duration-300'
        >
          Book appointment 
          <img className='w-3' src={assets.arrow_icon} alt="Arrow" />
        </a>
      </div>

      {/* Right Side */}
      <div className='md:w-1/2 relative'>
        <img 
          className='w-full md:absolute bottom-0 h-auto rounded-lg' 
          src={assets.header_img} 
          alt="Doctor Header" 
        />
      </div>
    </div>
  )
}

export default Header
```

### Step 3: Banner Component

**src/components/Banner.jsx**
```jsx
import React, { useContext } from 'react'
import { assets } from '../assets/assets'
import { useNavigate } from 'react-router-dom'
import { AppContext } from '../context/AppContext'

const Banner = () => {
  const navigate = useNavigate()
  const { token } = useContext(AppContext)

  return (
    <div className='flex bg-primary rounded-lg px-6 sm:px-10 md:px-14 lg:px-12 my-20 md:mx-10'>
      {/* Left Side */}
      <div className='flex-1 py-8 sm:py-10 md:py-16 lg:py-24 lg:pl-5'>
        <div className='text-xl sm:text-2xl md:text-3xl lg:text-5xl font-semibold text-white'>
          <p>Book Appointment</p>
          <p className='mt-4'>With 100+ Trusted Doctors</p>
        </div>
        <button 
          onClick={() => {
            if (token) {
              navigate('/doctors')
            } else {
              navigate('/login')
            }
            scrollTo(0, 0)
          }} 
          className='bg-white text-sm sm:text-base text-[#595959] px-8 py-3 rounded-full mt-6 hover:scale-105 transition-all duration-300'
        >
          {token ? 'Book Appointment' : 'Create account'}
        </button>
      </div>

      {/* Right Side */}
      <div className='hidden md:block md:w-1/2 lg:w-[370px] relative'>
        <img 
          className='w-full absolute bottom-0 right-0 max-w-md' 
          src={assets.appointment_img} 
          alt="Appointment" 
        />
      </div>
    </div>
  )
}

export default Banner
```

### Step 4: Footer Component

**src/components/Footer.jsx**
```jsx
import React from 'react'
import { assets } from '../assets/assets'

const Footer = () => {
  return (
    <div className='md:mx-10'>
      <div className='flex flex-col sm:grid grid-cols-[3fr_1fr_1fr] gap-14 my-10 mt-40 text-sm'>
        {/* Left Section */}
        <div>
          <img className='mb-5 w-40' src={assets.logo} alt="Prescripto Logo" />
          <p className='w-full md:w-2/3 text-gray-600 leading-6'>
            Lorem ipsum dolor sit amet consectetur adipisicing elit. Consectetur, 
            explicabo iste. Aliquid, quae voluptatibus omnis nam provident nobis 
            laboriosam facilis ullam dolor, eos soluta esse inventore temporibus 
            architecto eligendi consectetur!
          </p>
        </div>

        {/* Center Section */}
        <div>
          <p className='text-xl font-medium mb-5'>COMPANY</p>
          <ul className='flex flex-col gap-2 text-gray-600'>
            <li className='cursor-pointer hover:text-primary'>Home</li>
            <li className='cursor-pointer hover:text-primary'>About us</li>
            <li className='cursor-pointer hover:text-primary'>Contact us</li>
            <li className='cursor-pointer hover:text-primary'>Privacy policy</li>
          </ul>
        </div>

        {/* Right Section */}
        <div>
          <p className='text-xl font-medium mb-5'>GET IN TOUCH</p>
          <ul className='flex flex-col gap-2 text-gray-600'>
            <li>+1-212-456-7890</li>
            <li>contact@prescripto.com</li>
          </ul>
        </div>
      </div>

      {/* Copyright */}
      <div>
        <hr />
        <p className='py-5 text-sm text-center'>
          Copyright 2024 @ Prescripto - All Right Reserved.
        </p>
      </div>
    </div>
  )
}

export default Footer
```

---

## 🛣️ Routing Setup

### Step 1: Create Main App Component

**src/App.jsx**
```jsx
import React from 'react'
import { Routes, Route } from 'react-router-dom'
import { ToastContainer } from 'react-toastify'
import 'react-toastify/dist/ReactToastify.css'

// Import components
import Navbar from './components/Navbar'
import Footer from './components/Footer'

// Import pages
import Home from './pages/Home'
import Doctors from './pages/Doctors'
import Login from './pages/Login'
import About from './pages/About'
import Contact from './pages/Contact'
import MyProfile from './pages/MyProfile'
import MyAppointments from './pages/MyAppointments'
import Appointment from './pages/Appointment'
import Verify from './pages/Verify'

const App = () => {
  return (
    <div className='mx-4 sm:mx-[10%]'>
      <ToastContainer 
        position="top-right"
        autoClose={3000}
        hideProgressBar={false}
        newestOnTop={false}
        closeOnClick
        rtl={false}
        pauseOnFocusLoss
        draggable
        pauseOnHover
      />
      
      <Navbar />
      
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='/doctors' element={<Doctors />} />
        <Route path='/doctors/:speciality' element={<Doctors />} />
        <Route path='/login' element={<Login />} />
        <Route path='/about' element={<About />} />
        <Route path='/contact' element={<Contact />} />
        <Route path='/my-profile' element={<MyProfile />} />
        <Route path='/my-appointments' element={<MyAppointments />} />
        <Route path='/appointment/:docId' element={<Appointment />} />
        <Route path='/verify' element={<Verify />} />
      </Routes>
      
      <Footer />
    </div>
  )
}

export default App
```

---

## 📄 Building Pages

### Step 1: Home Page

**src/pages/Home.jsx**
```jsx
import React from 'react'
import Header from '../components/Header'
import SpecialityMenu from '../components/SpecialityMenu'
import TopDoctors from '../components/TopDoctors'
import Banner from '../components/Banner'

const Home = () => {
  return (
    <div>
      <Header />
      <SpecialityMenu />
      <TopDoctors />
      <Banner />
    </div>
  )
}

export default Home
```

### Step 2: Login Page

**src/pages/Login.jsx**
```jsx
import React, { useContext, useEffect, useState } from 'react'
import { AppContext } from '../context/AppContext'
import axios from 'axios'
import { toast } from 'react-toastify'
import { useNavigate } from 'react-router-dom'

const Login = () => {
  const [state, setState] = useState('Sign Up')
  const [name, setName] = useState('')
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  const navigate = useNavigate()
  const { backendUrl, token, setToken } = useContext(AppContext)

  const onSubmitHandler = async (event) => {
    event.preventDefault()

    try {
      if (state === 'Sign Up') {
        const { data } = await axios.post(`${backendUrl}/api/user/register`, {
          name,
          email,
          password
        })

        if (data.success) {
          localStorage.setItem('token', data.token)
          setToken(data.token)
          toast.success('Account created successfully!')
        } else {
          toast.error(data.message)
        }
      } else {
        const { data } = await axios.post(`${backendUrl}/api/user/login`, {
          email,
          password
        })

        if (data.success) {
          localStorage.setItem('token', data.token)
          setToken(data.token)
          toast.success('Logged in successfully!')
        } else {
          toast.error(data.message)
        }
      }
    } catch (error) {
      console.error('Authentication error:', error)
      toast.error('Something went wrong. Please try again.')
    }
  }

  useEffect(() => {
    if (token) {
      navigate('/')
    }
  }, [token, navigate])

  return (
    <form onSubmit={onSubmitHandler} className='min-h-[80vh] flex items-center'>
      <div className='flex flex-col gap-3 m-auto items-start p-8 min-w-[340px] sm:min-w-96 border rounded-xl text-[#5E5E5E] text-sm shadow-lg'>
        <p className='text-2xl font-semibold'>
          {state === 'Sign Up' ? 'Create Account' : 'Login'}
        </p>
        <p>
          Please {state === 'Sign Up' ? 'sign up' : 'log in'} to book appointment
        </p>

        {state === 'Sign Up' && (
          <div className='w-full'>
            <p>Full Name</p>
            <input
              onChange={(e) => setName(e.target.value)}
              value={name}
              className='border border-[#DADADA] rounded w-full p-2 mt-1'
              type="text"
              required
            />
          </div>
        )}

        <div className='w-full'>
          <p>Email</p>
          <input
            onChange={(e) => setEmail(e.target.value)}
            value={email}
            className='border border-[#DADADA] rounded w-full p-2 mt-1'
            type="email"
            required
          />
        </div>

        <div className='w-full'>
          <p>Password</p>
          <input
            onChange={(e) => setPassword(e.target.value)}
            value={password}
            className='border border-[#DADADA] rounded w-full p-2 mt-1'
            type="password"
            required
          />
        </div>

        <button className='bg-primary text-white w-full py-2 my-2 rounded-md text-base hover:bg-blue-600 transition-colors'>
          {state === 'Sign Up' ? 'Create account' : 'Login'}
        </button>

        {state === 'Sign Up' ? (
          <p>
            Already have an account?{' '}
            <span
              onClick={() => setState('Login')}
              className='text-primary underline cursor-pointer'
            >
              Login here
            </span>
          </p>
        ) : (
          <p>
            Create a new account?{' '}
            <span
              onClick={() => setState('Sign Up')}
              className='text-primary underline cursor-pointer'
            >
              Click here
            </span>
          </p>
        )}
      </div>
    </form>
  )
}

export default Login
```

### Step 3: Doctors Page

**src/pages/Doctors.jsx**
```jsx
import React, { useContext, useEffect, useState } from 'react'
import { useNavigate, useParams } from 'react-router-dom'
import { AppContext } from '../context/AppContext'

const Doctors = () => {
  const { speciality } = useParams()
  const { doctors } = useContext(AppContext)
  const navigate = useNavigate()
  const [filterDoc, setFilterDoc] = useState([])
  const [showFilter, setShowFilter] = useState(false)

  const applyFilter = () => {
    if (speciality) {
      setFilterDoc(doctors.filter(doc => doc.speciality === speciality))
    } else {
      setFilterDoc(doctors)
    }
  }

  useEffect(() => {
    applyFilter()
  }, [doctors, speciality])

  return (
    <div>
      <p className='text-gray-600'>Browse through the doctors specialist.</p>
      <div className='flex flex-col sm:flex-row items-start gap-5 mt-5'>
        {/* Filter Button - Mobile */}
        <button
          onClick={() => setShowFilter(!showFilter)}
          className={`py-1 px-3 border rounded text-sm transition-all sm:hidden ${showFilter ? 'bg-primary text-white' : ''}`}
        >
          Filters
        </button>

        {/* Filter Sidebar */}
        <div className={`flex-col gap-4 text-sm text-gray-600 ${showFilter ? 'flex' : 'hidden sm:flex'}`}>
          <p
            onClick={() => speciality === 'General physician' ? navigate('/doctors') : navigate('/doctors/General physician')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'General physician' ? 'bg-indigo-50 text-black' : ''}`}
          >
            General physician
          </p>
          <p
            onClick={() => speciality === 'Gynecologist' ? navigate('/doctors') : navigate('/doctors/Gynecologist')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'Gynecologist' ? 'bg-indigo-50 text-black' : ''}`}
          >
            Gynecologist
          </p>
          <p
            onClick={() => speciality === 'Dermatologist' ? navigate('/doctors') : navigate('/doctors/Dermatologist')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'Dermatologist' ? 'bg-indigo-50 text-black' : ''}`}
          >
            Dermatologist
          </p>
          <p
            onClick={() => speciality === 'Pediatricians' ? navigate('/doctors') : navigate('/doctors/Pediatricians')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'Pediatricians' ? 'bg-indigo-50 text-black' : ''}`}
          >
            Pediatricians
          </p>
          <p
            onClick={() => speciality === 'Neurologist' ? navigate('/doctors') : navigate('/doctors/Neurologist')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'Neurologist' ? 'bg-indigo-50 text-black' : ''}`}
          >
            Neurologist
          </p>
          <p
            onClick={() => speciality === 'Gastroenterologist' ? navigate('/doctors') : navigate('/doctors/Gastroenterologist')}
            className={`w-[94vw] sm:w-auto pl-3 py-1.5 pr-16 border border-gray-300 rounded transition-all cursor-pointer ${speciality === 'Gastroenterologist' ? 'bg-indigo-50 text-black' : ''}`}
          >
            Gastroenterologist
          </p>
        </div>

        {/* Doctors Grid */}
        <div className='w-full grid grid-cols-auto gap-4 gap-y-6'>
          {filterDoc.map((item, index) => (
            <div
              onClick={() => {
                navigate(`/appointment/${item._id}`)
                scrollTo(0, 0)
              }}
              className='border border-blue-200 rounded-xl overflow-hidden cursor-pointer hover:translate-y-[-10px] transition-all duration-500'
              key={index}
            >
              <img className='bg-blue-50' src={item.image} alt={item.name} />
              <div className='p-4'>
                <div className={`flex items-center gap-2 text-sm text-center ${item.available ? 'text-green-500' : 'text-gray-500'}`}>
                  <p className={`w-2 h-2 rounded-full ${item.available ? 'bg-green-500' : 'bg-gray-500'}`}></p>
                  <p>{item.available ? 'Available' : 'Not Available'}</p>
                </div>
                <p className='text-gray-900 text-lg font-medium'>{item.name}</p>
                <p className='text-gray-600 text-sm'>{item.speciality}</p>
              </div>
            </div>
          ))}
        </div>
      </div>
    </div>
  )
}

export default Doctors
```

---

## 🔐 Authentication Implementation

### Step 1: Create Authentication Hook

**src/hooks/useAuth.js**
```javascript
import { useContext } from 'react'
import { AppContext } from '../context/AppContext'

export const useAuth = () => {
  const { token, userData, setToken, setUserData } = useContext(AppContext)

  const login = (token) => {
    localStorage.setItem('token', token)
    setToken(token)
  }

  const logout = () => {
    localStorage.removeItem('token')
    setToken('')
    setUserData(false)
  }

  const isAuthenticated = () => {
    return !!token
  }

  return {
    token,
    userData,
    login,
    logout,
    isAuthenticated
  }
}
```

### Step 2: Protected Route Component

**src/components/ProtectedRoute.jsx**
```jsx
import React from 'react'
import { Navigate } from 'react-router-dom'
import { useAuth } from '../hooks/useAuth'

const ProtectedRoute = ({ children }) => {
  const { isAuthenticated } = useAuth()

  if (!isAuthenticated()) {
    return <Navigate to="/login" replace />
  }

  return children
}

export default ProtectedRoute
```

---

## 💳 Payment Integration

### Step 1: Razorpay Integration

**src/utils/payment.js**
```javascript
// Load Razorpay script
export const loadRazorpay = () => {
  return new Promise((resolve) => {
    const script = document.createElement('script')
    script.src = 'https://checkout.razorpay.com/v1/checkout.js'
    script.onload = () => {
      resolve(true)
    }
    script.onerror = () => {
      resolve(false)
    }
    document.body.appendChild(script)
  })
}

// Handle Razorpay payment
export const handleRazorpayPayment = async (amount, currency, userData, onSuccess, onError) => {
  const res = await loadRazorpay()

  if (!res) {
    onError('Razorpay SDK failed to load')
    return
  }

  const options = {
    key: import.meta.env.VITE_RAZORPAY_KEY_ID,
    amount: amount * 100, // Amount in paise
    currency: currency,
    name: 'Prescripto',
    description: 'Appointment Booking',
    handler: function (response) {
      onSuccess(response)
    },
    prefill: {
      name: userData.name,
      email: userData.email,
      contact: userData.phone || ''
    },
    theme: {
      color: '#5F6FFF'
    }
  }

  const paymentObject = new window.Razorpay(options)
  paymentObject.open()
}
```

---

## 🧪 Testing

### Step 1: Create Test Files

**src/components/__tests__/Navbar.test.jsx**
```jsx
import { render, screen } from '@testing-library/react'
import { BrowserRouter } from 'react-router-dom'
import { AppContext } from '../../context/AppContext'
import Navbar from '../Navbar'

const renderWithContext = (contextValue) => {
  return render(
    <BrowserRouter>
      <AppContext.Provider value={contextValue}>
        <Navbar />
      </AppContext.Provider>
    </BrowserRouter>
  )
}

describe('Navbar Component', () => {
  const mockContextValue = {
    token: null,
    userData: null,
    setToken: jest.fn()
  }

  test('renders logo and navigation links', () => {
    renderWithContext(mockContextValue)
    
    expect(screen.getByAltText('Prescripto Logo')).toBeInTheDocument()
    expect(screen.getByText('HOME')).toBeInTheDocument()
    expect(screen.getByText('ALL DOCTORS')).toBeInTheDocument()
    expect(screen.getByText('ABOUT')).toBeInTheDocument()
    expect(screen.getByText('CONTACT')).toBeInTheDocument()
  })

  test('shows create account button when not logged in', () => {
    renderWithContext(mockContextValue)
    
    expect(screen.getByText('Create account')).toBeInTheDocument()
  })

  test('shows user profile when logged in', () => {
    const loggedInContext = {
      ...mockContextValue,
      token: 'mock-token',
      userData: { name: 'John Doe', image: 'profile.jpg' }
    }

    renderWithContext(loggedInContext)
    
    expect(screen.getByAltText('Profile')).toBeInTheDocument()
  })
})
```

### Step 2: Setup Testing Environment

**package.json** (add to scripts section):
```json
{
  "scripts": {
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage"
  }
}
```

Install testing dependencies:
```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom
```

---

## 🚀 Deployment

### Step 1: Build for Production

```bash
# Create production build
npm run build

# Preview production build locally
npm run preview
```

### Step 2: Environment Variables for Production

Create production environment file:
```env
VITE_BACKEND_URL=https://your-api-domain.com
VITE_RAZORPAY_KEY_ID=your_production_razorpay_key
```

### Step 3: Deploy to Vercel

**vercel.json**
```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

Deploy commands:
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

### Step 4: Deploy to Netlify

**_redirects** file in public folder:
```
/*    /index.html   200
```

Deploy via Netlify CLI:
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Build and deploy
npm run build
netlify deploy --prod --dir=dist
```

---

## 📋 Final Checklist

### Before Going Live:

- [ ] **Environment Variables**: Set up production environment variables
- [ ] **API Endpoints**: Ensure all API calls use production URLs
- [ ] **Error Handling**: Implement comprehensive error handling
- [ ] **Loading States**: Add loading indicators for better UX
- [ ] **Responsive Design**: Test on all device sizes
- [ ] **Performance**: Optimize images and code splitting
- [ ] **SEO**: Add meta tags and OpenGraph data
- [ ] **Analytics**: Integrate Google Analytics or similar
- [ ] **Security**: Sanitize user inputs and implement CSP
- [ ] **Testing**: Run all tests and manual testing
- [ ] **Accessibility**: Ensure WCAG compliance
- [ ] **Browser Testing**: Test on different browsers

### Performance Optimizations:

1. **Code Splitting**: Implement React.lazy for route-based code splitting
2. **Image Optimization**: Use WebP format and lazy loading
3. **Bundle Analysis**: Use tools like webpack-bundle-analyzer
4. **Caching**: Implement proper caching strategies
5. **CDN**: Use CDN for static assets

---

## 🎯 Next Steps

### Advanced Features to Add:

1. **PWA Support**: Make the app installable
2. **Offline Support**: Implement service workers
3. **Push Notifications**: Add real-time notifications
4. **Dark Mode**: Implement theme switching
5. **Internationalization**: Add multi-language support
6. **Advanced Search**: Implement filters and search
7. **Chat Feature**: Add doctor-patient messaging
8. **Video Calls**: Integrate video consultation
9. **Calendar Integration**: Sync with external calendars
10. **Medical Records**: Add patient history management

This comprehensive guide provides everything needed to build the Prescripto frontend from scratch. Each section builds upon the previous one, creating a fully functional, modern web application.

Remember to test thoroughly, follow best practices, and iterate based on user feedback!
