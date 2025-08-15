# Frontend-Backend Connection Architecture - Prescripto Full-Stack

A comprehensive guide detailing how the frontend and backend are connected in the Prescripto Doctor Booking System, including data flow, folder structure, and API integration patterns.

## 📋 Table of Contents

1. [Project Structure Overview](#project-structure-overview)
2. [Connection Architecture](#connection-architecture)
3. [Frontend Context System](#frontend-context-system)
4. [Backend API Structure](#backend-api-structure)
5. [Data Flow Patterns](#data-flow-patterns)
6. [Authentication & Authorization Flow](#authentication--authorization-flow)
7. [API Integration Examples](#api-integration-examples)
8. [File Upload Integration](#file-upload-integration)
9. [Error Handling & Response Patterns](#error-handling--response-patterns)
10. [Environment Configuration](#environment-configuration)

---

## 🏗️ Project Structure Overview

### Root Directory Structure
```
prescripto-full-stack/
├── backend/                 # Node.js/Express API Server
│   ├── config/             # Database & service configurations
│   ├── controllers/        # Business logic & API handlers
│   ├── middleware/         # Authentication & validation
│   ├── models/            # MongoDB/Mongoose schemas
│   ├── routes/            # API route definitions
│   └── server.js          # Main server entry point
├── frontend/              # React.js Patient Portal
│   ├── src/
│   │   ├── components/    # Reusable UI components
│   │   ├── context/       # State management (React Context)
│   │   ├── pages/         # Route-based page components
│   │   └── assets/        # Static images & resources
│   └── main.jsx           # React application entry point
└── admin/                 # React.js Admin/Doctor Panel
    ├── src/
    │   ├── components/    # Admin-specific UI components
    │   ├── context/       # Admin & Doctor state management
    │   ├── pages/         # Admin & Doctor pages
    │   └── assets/        # Admin-specific assets
    └── main.jsx           # Admin application entry point
```

---

## 🔗 Connection Architecture

### Communication Flow Diagram
```
┌─────────────────┐    HTTP/HTTPS     ┌─────────────────┐
│   Frontend      │◄─────────────────►│    Backend      │
│   (Port 5173)   │   Axios Requests  │   (Port 4000)   │
└─────────────────┘                   └─────────────────┘
                                             │
┌─────────────────┐    HTTP/HTTPS           │
│   Admin Panel   │◄─────────────────────────┘
│   (Port 5174)   │   Axios Requests
└─────────────────┘
                                             
                                      ┌─────────────────┐
                                      │    MongoDB      │
                                      │   Database      │
                                      └─────────────────┘
```

### Connection Points

#### 1. **Frontend ↔ Backend Connection**
- **Protocol**: HTTP/HTTPS
- **Library**: Axios for HTTP requests
- **Base URL**: `http://localhost:4000` (development)
- **Authentication**: JWT tokens in request headers

#### 2. **Admin Panel ↔ Backend Connection**
- **Protocol**: HTTP/HTTPS
- **Library**: Axios for HTTP requests
- **Base URL**: `http://localhost:4000` (development)
- **Authentication**: Separate JWT tokens for admin and doctor roles

---

## 🎯 Frontend Context System

### Context Architecture in Frontend

#### **Main AppContext** (`frontend/src/context/AppContext.jsx`)
```jsx
import { createContext, useEffect, useState } from "react";
import axios from 'axios'

export const AppContext = createContext()

const AppContextProvider = (props) => {
    // Configuration from environment
    const currencySymbol = '₹'
    const backendUrl = import.meta.env.VITE_BACKEND_URL

    // Global state
    const [doctors, setDoctors] = useState([])
    const [token, setToken] = useState(localStorage.getItem('token') || '')
    const [userData, setUserData] = useState(false)

    // API Methods
    const getDoctosData = async () => {
        try {
            const { data } = await axios.get(backendUrl + '/api/doctor/list')
            if (data.success) {
                setDoctors(data.doctors)
            }
        } catch (error) {
            console.log(error)
        }
    }

    const loadUserProfileData = async () => {
        try {
            const { data } = await axios.get(
                backendUrl + '/api/user/get-profile', 
                { headers: { token } }
            )
            if (data.success) {
                setUserData(data.userData)
            }
        } catch (error) {
            console.log(error)
        }
    }

    // Auto-load data on context initialization
    useEffect(() => {
        getDoctosData()
    }, [])

    useEffect(() => {
        if (token) {
            loadUserProfileData()
        }
    }, [token])

    const value = {
        doctors, getDoctosData,
        currencySymbol, backendUrl,
        token, setToken,
        userData, setUserData, loadUserProfileData
    }

    return (
        <AppContext.Provider value={value}>
            {props.children}
        </AppContext.Provider>
    )
}
```

#### **Context Usage in Components**
```jsx
// In any frontend component
import { useContext } from 'react'
import { AppContext } from '../context/AppContext'

const SomeComponent = () => {
    const { doctors, backendUrl, token, userData } = useContext(AppContext)
    
    // Component can now access all global state and methods
    return (
        <div>
            {doctors.map(doctor => (
                <div key={doctor._id}>{doctor.name}</div>
            ))}
        </div>
    )
}
```

### Context Architecture in Admin Panel

#### **AdminContext** (`admin/src/context/AdminContext.jsx`)
```jsx
import axios from "axios";
import { createContext, useState } from "react";

export const AdminContext = createContext()

const AdminContextProvider = (props) => {
    const backendUrl = import.meta.env.VITE_BACKEND_URLk
    
    // Admin-specific state
    const [aToken, setAToken] = useState(localStorage.getItem('aToken') || '')
    const [appointments, setAppointments] = useState([])
    const [doctors, setDoctors] = useState([])
    const [dashData, setDashData] = useState(false)

    // Admin-specific API methods
    const getAllDoctors = async () => {
        try {
            const { data } = await axios.get(
                backendUrl + '/api/admin/all-doctors', 
                { headers: { aToken } }
            )
            if (data.success) {
                setDoctors(data.doctors)
            }
        } catch (error) {
            console.log(error)
        }
    }

    const getAllAppointments = async () => {
        try {
            const { data } = await axios.get(
                backendUrl + '/api/admin/appointments', 
                { headers: { aToken } }
            )
            if (data.success) {
                setAppointments(data.appointments.reverse())
            }
        } catch (error) {
            console.log(error)
        }
    }

    const value = {
        aToken, setAToken,
        doctors, getAllDoctors,
        appointments, getAllAppointments,
        dashData
    }

    return (
        <AdminContext.Provider value={value}>
            {props.children}
        </AdminContext.Provider>
    )
}
```

#### **DoctorContext** (`admin/src/context/DoctorContext.jsx`)
```jsx
import { createContext, useState } from "react";
import axios from 'axios'

export const DoctorContext = createContext()

const DoctorContextProvider = (props) => {
    const backendUrl = import.meta.env.VITE_BACKEND_URL
    
    // Doctor-specific state
    const [dToken, setDToken] = useState(localStorage.getItem('dToken') || '')
    const [appointments, setAppointments] = useState([])
    const [dashData, setDashData] = useState(false)
    const [profileData, setProfileData] = useState(false)

    // Doctor-specific API methods
    const getAppointments = async () => {
        try {
            const { data } = await axios.get(
                backendUrl + '/api/doctor/appointments', 
                { headers: { dToken } }
            )
            if (data.success) {
                setAppointments(data.appointments.reverse())
            }
        } catch (error) {
            console.log(error)
        }
    }

    const value = {
        dToken, setDToken,
        appointments, getAppointments,
        dashData, profileData
    }

    return (
        <DoctorContext.Provider value={value}>
            {props.children}
        </DoctorContext.Provider>
    )
}
```

---

## ⚙️ Backend API Structure

### Server Configuration (`backend/server.js`)
```javascript
import express from "express"
import cors from 'cors'
import 'dotenv/config'
import connectDB from "./config/mongodb.js"
import connectCloudinary from "./config/cloudinary.js"
import userRouter from "./routes/userRoute.js"
import doctorRouter from "./routes/doctorRoute.js"
import adminRouter from "./routes/adminRoute.js"

const app = express()
const port = process.env.PORT || 4000

// Initialize services
connectDB()
connectCloudinary()

// Middleware
app.use(express.json())
app.use(cors())

// API Routes
app.use("/api/user", userRouter)      // Patient operations
app.use("/api/admin", adminRouter)    // Admin operations
app.use("/api/doctor", doctorRouter)  // Doctor operations

app.listen(port, () => console.log(`Server started on PORT:${port}`))
```

### Route Structure

#### **User Routes** (`backend/routes/userRoute.js`)
```javascript
import express from 'express';
import { 
    loginUser, registerUser, getProfile, updateProfile, 
    bookAppointment, listAppointment, cancelAppointment 
} from '../controllers/userController.js';
import authUser from '../middleware/authUser.js';

const userRouter = express.Router();

// Public routes
userRouter.post("/register", registerUser)
userRouter.post("/login", loginUser)

// Protected routes (require authentication)
userRouter.get("/get-profile", authUser, getProfile)
userRouter.post("/update-profile", authUser, updateProfile)
userRouter.post("/book-appointment", authUser, bookAppointment)
userRouter.get("/appointments", authUser, listAppointment)
userRouter.post("/cancel-appointment", authUser, cancelAppointment)

export default userRouter;
```

#### **Admin Routes** (`backend/routes/adminRoute.js`)
```javascript
import express from 'express';
import { 
    loginAdmin, appointmentsAdmin, appointmentCancel, 
    addDoctor, allDoctors, adminDashboard 
} from '../controllers/adminController.js';
import authAdmin from '../middleware/authAdmin.js';

const adminRouter = express.Router();

// Public routes
adminRouter.post("/login", loginAdmin)

// Protected routes (require admin authentication)
adminRouter.post("/add-doctor", authAdmin, addDoctor)
adminRouter.get("/appointments", authAdmin, appointmentsAdmin)
adminRouter.post("/cancel-appointment", authAdmin, appointmentCancel)
adminRouter.get("/all-doctors", authAdmin, allDoctors)
adminRouter.get("/dashboard", authAdmin, adminDashboard)

export default adminRouter;
```

#### **Doctor Routes** (`backend/routes/doctorRoute.js`)
```javascript
import express from 'express';
import { 
    loginDoctor, appointmentsDoctor, appointmentCancel, 
    doctorList, changeAvailablity, appointmentComplete 
} from '../controllers/doctorController.js';
import authDoctor from '../middleware/authDoctor.js';

const doctorRouter = express.Router();

// Public routes
doctorRouter.post("/login", loginDoctor)
doctorRouter.get("/list", doctorList)  // Used by frontend to get all doctors

// Protected routes (require doctor authentication)
doctorRouter.get("/appointments", authDoctor, appointmentsDoctor)
doctorRouter.post("/cancel-appointment", authDoctor, appointmentCancel)
doctorRouter.post("/change-availability", authDoctor, changeAvailablity)
doctorRouter.post("/complete-appointment", authDoctor, appointmentComplete)

export default doctorRouter;
```

---

## 🔄 Data Flow Patterns

### 1. **Initial Data Loading Flow**

#### Frontend Application Startup
```
1. main.jsx renders App component wrapped with AppContextProvider
2. AppContext initializes with useEffect hooks
3. getDoctosData() automatically called
4. Axios GET request to /api/doctor/list
5. Backend returns doctors array
6. Frontend updates doctors state
7. All components using AppContext get updated doctors data
```

**Code Flow:**
```jsx
// 1. main.jsx
ReactDOM.createRoot(document.getElementById('root')).render(
  <BrowserRouter>
    <AppContextProvider>  {/* Context wrapper */}
      <App />
    </AppContextProvider>
  </BrowserRouter>
)

// 2. AppContext.jsx
useEffect(() => {
    getDoctosData()  // Auto-call on mount
}, [])

// 3. API call
const getDoctosData = async () => {
    const { data } = await axios.get(backendUrl + '/api/doctor/list')
    if (data.success) {
        setDoctors(data.doctors)  // Update global state
    }
}

// 4. Backend controller (doctorController.js)
const doctorList = async (req, res) => {
    const doctors = await doctorModel.find({}).select(['-password', '-email'])
    res.json({ success: true, doctors })
}
```

### 2. **User Authentication Flow**

#### Login Process
```
1. User enters credentials in Login component
2. Form submission triggers onSubmitHandler
3. Axios POST to /api/user/login with email/password
4. Backend validates credentials and generates JWT token
5. Frontend stores token in localStorage and context
6. Token triggers useEffect to load user profile data
7. User redirected to home page
```

**Code Flow:**
```jsx
// 1. Login.jsx - Form submission
const onSubmitHandler = async (event) => {
    event.preventDefault();
    
    const { data } = await axios.post(
        backendUrl + '/api/user/login', 
        { email, password }
    )
    
    if (data.success) {
        localStorage.setItem('token', data.token)
        setToken(data.token)  // Updates context
    }
}

// 2. AppContext.jsx - Token change effect
useEffect(() => {
    if (token) {
        loadUserProfileData()  // Auto-load profile when token available
    }
}, [token])

// 3. Backend userController.js - Login validation
const loginUser = async (req, res) => {
    const { email, password } = req.body
    const user = await userModel.findOne({ email })
    
    const isMatch = await bcrypt.compare(password, user.password)
    
    if (isMatch) {
        const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET)
        res.json({ success: true, token })
    }
}
```

### 3. **Appointment Booking Flow**

#### Complete Booking Process
```
1. User selects doctor from Doctors page
2. Navigates to /appointment/:docId
3. Appointment component loads doctor details from context
4. User selects date and time slot
5. Clicks "Book Appointment" button
6. Axios POST to /api/user/book-appointment with appointment data
7. Backend validates and creates appointment record
8. Backend updates doctor's slot availability
9. Frontend shows success message and redirects
10. My Appointments page auto-refreshes to show new appointment
```

**Code Flow:**
```jsx
// 1. Appointment.jsx - Booking function
const bookAppointment = async () => {
    const slotDate = day + "_" + month + "_" + year
    
    try {
        const { data } = await axios.post(
            backendUrl + '/api/user/book-appointment', 
            { docId, slotDate, slotTime }, 
            { headers: { token } }
        )
        
        if (data.success) {
            toast.success(data.message)
            getDoctosData()  // Refresh doctors data
            navigate('/my-appointments')
        }
    } catch (error) {
        toast.error(error.message)
    }
}

// 2. Backend userController.js - Appointment creation
const bookAppointment = async (req, res) => {
    const { userId, docId, slotDate, slotTime } = req.body
    
    // Get doctor and user data
    const docData = await doctorModel.findById(docId).select("-password")
    const userData = await userModel.findById(userId).select("-password")
    
    // Create appointment
    const appointmentData = {
        userId, docId, userData, docData, 
        amount: docData.fees, slotTime, slotDate, 
        date: Date.now()
    }
    
    const newAppointment = new appointmentModel(appointmentData)
    await newAppointment.save()
    
    // Update doctor's slots
    await doctorModel.findByIdAndUpdate(docId, {
        $push: { [`slots_booked.${slotDate}`]: slotTime }
    })
    
    res.json({ success: true, message: 'Appointment Booked' })
}
```

### 4. **Admin Dashboard Data Flow**

#### Admin Data Loading
```
1. Admin logs in through admin panel
2. AdminContext gets admin token (aToken)
3. Dashboard component calls getDashData()
4. Multiple API calls fetch dashboard statistics
5. Data displayed in admin dashboard cards and charts
```

**Code Flow:**
```jsx
// 1. Admin Dashboard.jsx
const Dashboard = () => {
    const { aToken, getDashData, dashData } = useContext(AdminContext)
    
    useEffect(() => {
        if (aToken) {
            getDashData()  // Load dashboard data when admin authenticated
        }
    }, [aToken])
    
    return dashData && (
        <div>
            <div>Doctors: {dashData.doctors}</div>
            <div>Appointments: {dashData.appointments}</div>
            <div>Patients: {dashData.patients}</div>
        </div>
    )
}

// 2. AdminContext.jsx
const getDashData = async () => {
    try {
        const { data } = await axios.get(
            backendUrl + '/api/admin/dashboard', 
            { headers: { aToken } }
        )
        
        if (data.success) {
            setDashData(data.dashData)
        }
    } catch (error) {
        console.log(error)
    }
}

// 3. Backend adminController.js
const adminDashboard = async (req, res) => {
    try {
        const doctors = await doctorModel.find({})
        const users = await userModel.find({})
        const appointments = await appointmentModel.find({})
        
        const dashData = {
            doctors: doctors.length,
            appointments: appointments.length,
            patients: users.length,
            latestAppointments: appointments.reverse()
        }
        
        res.json({ success: true, dashData })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

---

## 🔐 Authentication & Authorization Flow

### Token-Based Authentication System

#### **Frontend Token Storage**
```jsx
// Token storage in localStorage and context
const [token, setToken] = useState(localStorage.getItem('token') || '')

// Automatic token persistence
useEffect(() => {
    if (token) {
        localStorage.setItem('token', token)
        loadUserProfileData()
    }
}, [token])
```

#### **Backend Authentication Middleware**

**User Authentication** (`backend/middleware/authUser.js`)
```javascript
import jwt from "jsonwebtoken"

const authUser = async (req, res, next) => {
    try {
        const { token } = req.headers
        
        if (!token) {
            return res.json({ success: false, message: 'Not Authorized' })
        }
        
        const decoded = jwt.verify(token, process.env.JWT_SECRET)
        req.body.userId = decoded.id  // Add userId to request
        next()
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

**Admin Authentication** (`backend/middleware/authAdmin.js`)
```javascript
const authAdmin = async (req, res, next) => {
    try {
        const { atoken } = req.headers
        
        if (!atoken) {
            return res.json({ success: false, message: 'Not Authorized' })
        }
        
        const token_decode = jwt.verify(atoken, process.env.JWT_SECRET)
        
        if (token_decode !== process.env.ADMIN_EMAIL + process.env.ADMIN_PASSWORD) {
            return res.json({ success: false, message: 'Not Authorized' })
        }
        
        next()
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

### Authentication Headers in API Calls

#### **Frontend API Pattern**
```jsx
// User authenticated requests
const { data } = await axios.get(
    backendUrl + '/api/user/get-profile',
    { headers: { token } }  // User token
)

// Admin authenticated requests
const { data } = await axios.get(
    backendUrl + '/api/admin/all-doctors',
    { headers: { aToken } }  // Admin token
)

// Doctor authenticated requests
const { data } = await axios.get(
    backendUrl + '/api/doctor/appointments',
    { headers: { dToken } }  // Doctor token
)
```

---

## 📡 API Integration Examples

### 1. **Doctor Listing** (Public API)

#### Frontend Implementation
```jsx
// components/TopDoctors.jsx or pages/Doctors.jsx
import { useContext } from 'react'
import { AppContext } from '../context/AppContext'

const DoctorsComponent = () => {
    const { doctors } = useContext(AppContext)  // Get from global context
    
    return (
        <div>
            {doctors.map(doctor => (
                <div key={doctor._id} className="doctor-card">
                    <img src={doctor.image} alt={doctor.name} />
                    <h3>{doctor.name}</h3>
                    <p>{doctor.speciality}</p>
                    <p>₹{doctor.fees}</p>
                </div>
            ))}
        </div>
    )
}
```

#### Backend Implementation
```javascript
// controllers/doctorController.js
const doctorList = async (req, res) => {
    try {
        // Select all doctors except sensitive fields
        const doctors = await doctorModel
            .find({})
            .select(['-password', '-email'])
        
        res.json({ success: true, doctors })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

### 2. **User Profile Management** (Authenticated API)

#### Frontend Implementation
```jsx
// pages/MyProfile.jsx
const MyProfile = () => {
    const { token, backendUrl, userData, setUserData } = useContext(AppContext)
    const [isEdit, setIsEdit] = useState(false)
    const [image, setImage] = useState(false)
    
    const updateUserProfileData = async () => {
        try {
            const formData = new FormData()
            formData.append('name', userData.name)
            formData.append('phone', userData.phone)
            formData.append('address', JSON.stringify(userData.address))
            formData.append('gender', userData.gender)
            formData.append('dob', userData.dob)
            
            if (image) {
                formData.append('image', image)
            }
            
            const { data } = await axios.post(
                backendUrl + '/api/user/update-profile', 
                formData, 
                { headers: { token } }
            )
            
            if (data.success) {
                toast.success(data.message)
                await loadUserProfileData()  // Refresh user data
                setIsEdit(false)
            }
        } catch (error) {
            toast.error(error.message)
        }
    }
}
```

#### Backend Implementation
```javascript
// controllers/userController.js
const updateProfile = async (req, res) => {
    try {
        const { userId, name, phone, address, dob, gender } = req.body
        const imageFile = req.file
        
        if (!name || !phone || !dob || !gender) {
            return res.json({ success: false, message: "Data Missing" })
        }
        
        let imageUrl = null
        if (imageFile) {
            // Upload to Cloudinary
            const imageUpload = await cloudinary.uploader.upload(imageFile.path, {
                resource_type: "image"
            })
            imageUrl = imageUpload.secure_url
        }
        
        await userModel.findByIdAndUpdate(userId, {
            name, phone, address: JSON.parse(address), dob, gender,
            ...(imageUrl && { image: imageUrl })
        })
        
        res.json({ success: true, message: "Profile Updated" })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

### 3. **Appointment Management** (Multi-role API)

#### Frontend - Patient View
```jsx
// pages/MyAppointments.jsx
const MyAppointments = () => {
    const { backendUrl, token } = useContext(AppContext)
    const [appointments, setAppointments] = useState([])
    
    const getUserAppointments = async () => {
        try {
            const { data } = await axios.get(
                backendUrl + '/api/user/appointments', 
                { headers: { token } }
            )
            setAppointments(data.appointments.reverse())
        } catch (error) {
            toast.error(error.message)
        }
    }
    
    const cancelAppointment = async (appointmentId) => {
        try {
            const { data } = await axios.post(
                backendUrl + '/api/user/cancel-appointment',
                { appointmentId },
                { headers: { token } }
            )
            
            if (data.success) {
                toast.success(data.message)
                getUserAppointments()  // Refresh list
            }
        } catch (error) {
            toast.error(error.message)
        }
    }
}
```

#### Admin Panel - Admin View
```jsx
// pages/Admin/AllAppointments.jsx
const AllAppointments = () => {
    const { aToken, appointments, getAllAppointments, cancelAppointment } = useContext(AdminContext)
    
    useEffect(() => {
        if (aToken) {
            getAllAppointments()
        }
    }, [aToken])
    
    return (
        <div>
            {appointments.map(appointment => (
                <div key={appointment._id}>
                    <p>Patient: {appointment.userData.name}</p>
                    <p>Doctor: {appointment.docData.name}</p>
                    <button onClick={() => cancelAppointment(appointment._id)}>
                        Cancel
                    </button>
                </div>
            ))}
        </div>
    )
}
```

#### Backend - Unified Controller
```javascript
// controllers/userController.js - Patient appointments
const listAppointment = async (req, res) => {
    try {
        const { userId } = req.body  // From auth middleware
        const appointments = await appointmentModel.find({ userId })
        res.json({ success: true, appointments })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}

// controllers/adminController.js - All appointments
const appointmentsAdmin = async (req, res) => {
    try {
        const appointments = await appointmentModel.find({})
        res.json({ success: true, appointments })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

---

## 📁 File Upload Integration

### Frontend File Upload Flow
```jsx
// File selection and form data preparation
const [image, setImage] = useState(false)

const handleSubmit = async () => {
    const formData = new FormData()
    formData.append('name', userData.name)
    formData.append('image', image)  // File object
    
    const { data } = await axios.post(
        backendUrl + '/api/user/update-profile',
        formData,  // FormData for file upload
        { 
            headers: { 
                token,
                'Content-Type': 'multipart/form-data'  // Important for file uploads
            } 
        }
    )
}

// File input handling
<input 
    type="file" 
    onChange={(e) => setImage(e.target.files[0])}
    accept="image/*"
/>
```

### Backend File Processing
```javascript
// middleware/multer.js - File upload middleware
import multer from 'multer'

const storage = multer.diskStorage({
    filename: (req, file, callback) => {
        callback(null, file.originalname)
    }
})

const upload = multer({ storage })
export default upload

// Route with file upload
adminRouter.post("/add-doctor", authAdmin, upload.single('image'), addDoctor)

// Controller processing
const addDoctor = async (req, res) => {
    try {
        const imageFile = req.file  // File from multer
        
        // Upload to Cloudinary
        const imageUpload = await cloudinary.uploader.upload(imageFile.path, {
            resource_type: "image"
        })
        
        const imageUrl = imageUpload.secure_url
        
        // Save to database
        const docData = {
            name, email, password: hashedPassword,
            image: imageUrl,  // Cloudinary URL
            // ... other fields
        }
        
        const newDoctor = new doctorModel(docData)
        await newDoctor.save()
        
        res.json({ success: true, message: 'Doctor Added' })
    } catch (error) {
        res.json({ success: false, message: error.message })
    }
}
```

---

## ⚠️ Error Handling & Response Patterns

### Consistent API Response Format
```javascript
// Success response
{
    success: true,
    message: "Operation successful",
    data: { /* response data */ }
}

// Error response
{
    success: false,
    message: "Error description",
    error?: "Detailed error information"
}
```

### Frontend Error Handling Pattern
```jsx
const apiCall = async () => {
    try {
        const { data } = await axios.post(url, payload, config)
        
        if (data.success) {
            toast.success(data.message)
            // Handle success
        } else {
            toast.error(data.message)
            // Handle API error
        }
    } catch (error) {
        // Handle network/server errors
        console.error('Network error:', error)
        toast.error('Network error. Please try again.')
    }
}
```

### Backend Error Handling
```javascript
const controllerFunction = async (req, res) => {
    try {
        // Business logic
        const result = await someOperation()
        
        res.json({ 
            success: true, 
            message: 'Operation successful',
            data: result 
        })
    } catch (error) {
        console.log(error)
        res.json({ 
            success: false, 
            message: error.message 
        })
    }
}
```

---

## 🔧 Environment Configuration

### Frontend Environment Setup
```bash
# frontend/.env
VITE_BACKEND_URL=http://localhost:4000
VITE_RAZORPAY_KEY_ID=your_razorpay_key
VITE_STRIPE_PUBLISHABLE_KEY=your_stripe_key
```

### Admin Panel Environment Setup
```bash
# admin/.env
VITE_BACKEND_URL=http://localhost:4000
```

### Backend Environment Setup
```bash
# backend/.env
PORT=4000
JWT_SECRET=your_jwt_secret
MONGODB_URI=mongodb://localhost:27017/prescripto
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_SECRET_KEY=your_secret_key
ADMIN_EMAIL=admin@example.com
ADMIN_PASSWORD=admin_password
```

### Environment Variable Usage

#### Frontend
```jsx
// Accessing environment variables in React
const backendUrl = import.meta.env.VITE_BACKEND_URL
const razorpayKey = import.meta.env.VITE_RAZORPAY_KEY_ID
```

#### Backend
```javascript
// Accessing environment variables in Node.js
const port = process.env.PORT || 4000
const jwtSecret = process.env.JWT_SECRET
const mongoUri = process.env.MONGODB_URI
```

---

## 🎯 Summary

The Prescripto frontend-backend connection is built on:

1. **Context-based State Management**: React Context provides global state across components
2. **RESTful API Design**: Clean, resource-based API endpoints
3. **Token-based Authentication**: JWT tokens for secure user identification
4. **Axios HTTP Client**: Consistent API communication patterns
5. **Environment Configuration**: Flexible configuration through environment variables
6. **Error Handling**: Comprehensive error handling at all levels
7. **File Upload Integration**: Seamless image upload through Cloudinary
8. **Multi-role Architecture**: Separate contexts and APIs for different user types

This architecture ensures scalable, maintainable, and secure communication between all components of the Prescripto system.
