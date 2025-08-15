# Prescripto Full-Stack Integration Guide

A comprehensive guide to integrate the frontend, admin panel, and backend components of the Prescripto Doctor Booking System.

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Architecture Components](#architecture-components)
3. [Environment Setup](#environment-setup)
4. [Backend Configuration](#backend-configuration)
5. [Frontend Integration](#frontend-integration)
6. [Admin Panel Integration](#admin-panel-integration)
7. [API Integration Details](#api-integration-details)
8. [Database Integration](#database-integration)
9. [Authentication Flow](#authentication-flow)
10. [Payment Gateway Integration](#payment-gateway-integration)
11. [File Upload Integration](#file-upload-integration)
12. [Development Workflow](#development-workflow)
13. [Production Deployment](#production-deployment)
14. [Testing Integration](#testing-integration)
15. [Troubleshooting](#troubleshooting)

---

## 🎯 Project Overview

The Prescripto system consists of three main components that work together:

- **Backend API** (Node.js/Express) - Port 4000
- **Frontend** (React/Vite) - Port 5173
- **Admin Panel** (React/Vite) - Port 5174

### Integration Flow
```
Frontend (5173) ←→ Backend API (4000) ←→ MongoDB
Admin Panel (5174) ←→ Backend API (4000) ←→ MongoDB
```

---

## 🏗️ Architecture Components

### Backend (Node.js/Express)
- **Server**: Express.js application on port 4000
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: JWT tokens with bcrypt password hashing
- **File Storage**: Cloudinary integration
- **Payment**: Razorpay and Stripe integration
- **API Routes**: RESTful endpoints for users, doctors, and admin

### Frontend (React/Vite)
- **Framework**: React 18 with Vite build tool
- **Routing**: React Router DOM
- **HTTP Client**: Axios for API communication
- **UI**: Tailwind CSS for styling
- **State Management**: React Context API
- **Notifications**: React Toastify

### Admin Panel (React/Vite)
- **Framework**: React 18 with Vite build tool
- **Port**: 5174 (different from frontend)
- **Authentication**: Admin-specific JWT tokens
- **Features**: Doctor management, appointment oversight, analytics

---

## 🔧 Environment Setup

### 1. Prerequisites
Ensure you have the following installed:
```bash
Node.js (v18 or higher)
npm or yarn
MongoDB (local or cloud)
Git
```

### 2. Clone and Setup
```bash
# Clone the repository
git clone <repository-url>
cd prescripto-full-stack

# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install

# Install admin panel dependencies
cd ../admin
npm install
```

### 3. Environment Variables

#### Backend Environment (.env)
Create a `.env` file in the `backend` directory:
```env
# Server Configuration
PORT=4000
JWT_SECRET=your_jwt_secret_key

# Admin Panel Credentials
ADMIN_EMAIL=admin@prescripto.com
ADMIN_PASSWORD=your_admin_password

# MongoDB Configuration
MONGODB_URI=mongodb://localhost:27017/prescripto
# or for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/prescripto

# Cloudinary Configuration (required for file uploads)
CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_SECRET_KEY=your_cloudinary_secret_key

# Payment Gateway Configuration
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
STRIPE_SECRET_KEY=your_stripe_secret_key

# Currency Settings
CURRENCY=INR
```

#### Frontend Environment (.env)
Create a `.env` file in the `frontend` directory:
```env
# Backend API URL
VITE_BACKEND_URL=http://localhost:4000

# Payment Gateway Keys (Public Keys)
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
VITE_STRIPE_PUBLISHABLE_KEY=your_stripe_publishable_key
```

#### Admin Panel Environment (.env)
Create a `.env` file in the `admin` directory:
```env
# Backend API URL
VITE_BACKEND_URL=http://localhost:4000
```

---

## 🗄️ Backend Configuration

### 1. Database Models
The backend uses MongoDB with three main models:

#### User Model (`userModel.js`)
```javascript
{
  name: String,
  email: String (unique),
  password: String (hashed),
  image: String (Cloudinary URL),
  address: Object,
  gender: String,
  dob: String,
  phone: String
}
```

#### Doctor Model (`doctorModel.js`)
```javascript
{
  name: String,
  email: String (unique),
  password: String (hashed),
  image: String (Cloudinary URL),
  speciality: String,
  degree: String,
  experience: String,
  about: String,
  available: Boolean,
  fees: Number,
  address: Object,
  date: Number,
  slots_booked: Object
}
```

#### Appointment Model (`appointmentModel.js`)
```javascript
{
  userId: ObjectId,
  docId: ObjectId,
  slotDate: String,
  slotTime: String,
  userData: Object,
  docData: Object,
  amount: Number,
  date: Number,
  cancelled: Boolean,
  payment: Boolean,
  isCompleted: Boolean
}
```

### 2. API Endpoints Structure

#### User Routes (`/api/user`)
- `POST /register` - User registration
- `POST /login` - User login
- `GET /get-profile` - Get user profile (auth required)
- `POST /update-profile` - Update user profile (auth required)
- `POST /book-appointment` - Book appointment (auth required)
- `GET /appointments` - Get user appointments (auth required)
- `POST /cancel-appointment` - Cancel appointment (auth required)
- `POST /payment-razorpay` - Razorpay payment (auth required)
- `POST /verifyRazorpay` - Verify Razorpay payment (auth required)
- `POST /payment-stripe` - Stripe payment (auth required)
- `POST /verifyStripe` - Verify Stripe payment (auth required)

#### Doctor Routes (`/api/doctor`)
- `POST /login` - Doctor login
- `GET /appointments` - Get doctor appointments (auth required)
- `POST /cancel-appointment` - Cancel appointment (auth required)
- `POST /complete-appointment` - Mark appointment complete (auth required)
- `GET /list` - Get all doctors (public)
- `POST /change-availability` - Toggle availability (auth required)
- `GET /dashboard` - Get doctor dashboard data (auth required)
- `GET /profile` - Get doctor profile (auth required)
- `POST /update-profile` - Update doctor profile (auth required)

#### Admin Routes (`/api/admin`)
- `POST /login` - Admin login
- `POST /add-doctor` - Add new doctor (auth required)
- `GET /appointments` - Get all appointments (auth required)
- `POST /cancel-appointment` - Cancel appointment (auth required)
- `GET /all-doctors` - Get all doctors (auth required)
- `POST /change-availability` - Change doctor availability (auth required)
- `GET /dashboard` - Get admin dashboard data (auth required)

### 3. Authentication Middleware

#### User Authentication (`authUser.js`)
```javascript
// Validates user JWT token from headers
// Adds userId to req.body for authenticated routes
```

#### Doctor Authentication (`authDoctor.js`)
```javascript
// Validates doctor JWT token from headers
// Adds docId to req.body for authenticated routes
```

#### Admin Authentication (`authAdmin.js`)
```javascript
// Validates admin JWT token from headers
// Verifies admin credentials
```

---

## 🎨 Frontend Integration

### 1. Context Setup
The frontend uses React Context for state management:

#### AppContext (`AppContext.jsx`)
```javascript
const AppContext = createContext()

// Provides:
- doctors: Array of all doctors
- token: User authentication token
- userData: Current user data
- backendUrl: API base URL
- currencySymbol: Currency symbol
- getDoctosData(): Fetch doctors
- loadUserProfileData(): Load user profile
```

### 2. API Integration Pattern
```javascript
// Example API call pattern
const { backendUrl, token } = useContext(AppContext)

const apiCall = async () => {
  try {
    const { data } = await axios.get(
      `${backendUrl}/api/user/appointments`,
      { headers: { token } }
    )
    if (data.success) {
      // Handle success
    } else {
      toast.error(data.message)
    }
  } catch (error) {
    console.log(error)
    toast.error(error.message)
  }
}
```

### 3. Key Components Integration

#### Authentication Pages
- **Login/Register**: Handles user authentication
- **Token Management**: Stores JWT in localStorage
- **Auto-redirect**: Redirects authenticated users

#### Appointment System
- **Doctor Listing**: Fetches and displays doctors
- **Appointment Booking**: Integrates with backend booking API
- **Slot Management**: Real-time slot availability
- **Payment Integration**: Razorpay/Stripe integration

#### User Profile
- **Profile Management**: Update user information
- **Image Upload**: Cloudinary integration via backend
- **Appointment History**: Display user appointments

---

## 👨‍💼 Admin Panel Integration

### 1. Admin Context Setup
```javascript
const AdminContext = createContext()

// Provides:
- aToken: Admin authentication token
- doctors: All doctors data
- appointments: All appointments data
- dashData: Dashboard analytics
- getAllDoctors(): Fetch all doctors
- getAllAppointments(): Fetch all appointments
- changeAvailability(): Toggle doctor availability
- cancelAppointment(): Cancel appointments
```

### 2. Admin Authentication Flow
1. Admin enters credentials on login page
2. Backend validates against environment variables
3. JWT token generated and stored
4. All admin API calls include aToken header

### 3. Admin Features Integration

#### Doctor Management
- **Add Doctor**: Form with image upload
- **Doctor List**: Display all doctors with controls
- **Availability Toggle**: Real-time availability updates

#### Appointment Management
- **All Appointments**: System-wide appointment view
- **Cancel Appointments**: Admin override capability
- **Status Updates**: Real-time appointment status

#### Dashboard Analytics
- **Statistics**: Doctor count, appointment count, patient count
- **Recent Activity**: Latest appointments
- **Revenue Tracking**: Payment analytics

---

## 🔌 API Integration Details

### 1. Base URL Configuration
All API calls use the base URL from environment variables:
```javascript
// Frontend & Admin
const backendUrl = import.meta.env.VITE_BACKEND_URL
// Default: http://localhost:4000
```

### 2. Authentication Headers
Different authentication tokens for different user types:
```javascript
// User authentication
headers: { token: userToken }

// Doctor authentication  
headers: { dToken: doctorToken }

// Admin authentication
headers: { aToken: adminToken }
```

### 3. Response Format
All API responses follow a consistent format:
```javascript
{
  success: boolean,
  message: string,
  data?: any
}
```

### 4. Error Handling Pattern
```javascript
try {
  const { data } = await axios.post(url, payload, config)
  if (data.success) {
    // Handle success
    toast.success(data.message)
  } else {
    // Handle API error
    toast.error(data.message)
  }
} catch (error) {
  // Handle network/server error
  console.log(error)
  toast.error(error.message)
}
```

---

## 🗃️ Database Integration

### 1. MongoDB Connection
```javascript
// Backend: config/mongodb.js
const connectDB = async () => {
  mongoose.connection.on('connected', () => 
    console.log("Database Connected")
  )
  await mongoose.connect(`${process.env.MONGODB_URI}/prescripto`)
}
```

### 2. Data Relationships
- **Users ← Appointments → Doctors**: Many-to-many through appointments
- **Appointments**: Store both user and doctor data for efficiency
- **Slots Management**: Embedded in doctor model for real-time updates

### 3. Data Synchronization
- **Real-time Updates**: Immediate UI updates after API calls
- **Data Consistency**: Backend validates all relationships
- **Error Recovery**: Frontend refetches data on errors

---

## 🔐 Authentication Flow

### 1. User Authentication Flow
```
1. User submits login form
2. Frontend sends POST /api/user/login
3. Backend validates credentials
4. JWT token generated and returned
5. Frontend stores token in localStorage
6. Token included in all subsequent requests
7. Backend middleware validates token
```

### 2. Doctor Authentication Flow
```
1. Doctor login through admin panel
2. POST /api/doctor/login
3. Doctor-specific JWT token
4. Stored as dToken in localStorage
5. Used for doctor-specific operations
```

### 3. Admin Authentication Flow
```
1. Admin credentials hardcoded in environment
2. POST /api/admin/login validates against env vars
3. Admin JWT token generated
4. Stored as aToken in localStorage
5. Required for all admin operations
```

### 4. Token Refresh Strategy
- Tokens stored in localStorage persist across sessions
- No automatic refresh implemented
- Users must re-login when tokens expire
- Consider implementing refresh token for production

---

## 💳 Payment Gateway Integration

### 1. Razorpay Integration

#### Frontend Setup
```javascript
// Load Razorpay script
<script src="https://checkout.razorpay.com/v1/checkout.js"></script>

// Payment initialization
const options = {
  key: import.meta.env.VITE_RAZORPAY_KEY_ID,
  amount: order.amount,
  currency: order.currency,
  name: 'Prescripto',
  description: "Appointment Payment",
  order_id: order.id,
  handler: async (response) => {
    // Verify payment on backend
  }
}
const rzp = new window.Razorpay(options)
rzp.open()
```

#### Backend Integration
```javascript
// Create Razorpay order
const order = await razorpayInstance.orders.create({
  amount: amount * 100, // Convert to paisa
  currency: 'INR',
  receipt: appointmentId
})

// Verify payment
const isValidSignature = razorpay.validateWebhookSignature(
  body, signature, secret
)
```

### 2. Stripe Integration

#### Frontend Setup
```javascript
// Stripe checkout session
const { data } = await axios.post(
  backendUrl + '/api/user/payment-stripe',
  { appointmentId }
)
window.location.replace(data.session_url)
```

#### Backend Integration
```javascript
// Create Stripe session
const session = await stripeInstance.checkout.sessions.create({
  payment_method_types: ['card'],
  line_items: [{
    price_data: {
      currency: 'inr',
      product_data: { name: 'Appointment' },
      unit_amount: amount * 100
    },
    quantity: 1
  }],
  mode: 'payment',
  success_url: `${frontend_url}/verify?success=true`,
  cancel_url: `${frontend_url}/verify?success=false`
})
```

---

## 📁 File Upload Integration

### 1. Cloudinary Setup
```javascript
// Backend: config/cloudinary.js
import { v2 as cloudinary } from 'cloudinary'

const connectCloudinary = () => {
  cloudinary.config({
    cloud_name: process.env.CLOUDINARY_NAME,
    api_key: process.env.CLOUDINARY_API_KEY,
    api_secret: process.env.CLOUDINARY_SECRET_KEY
  })
}
```

### 2. Multer Configuration
```javascript
// Backend: middleware/multer.js
import multer from 'multer'

const storage = multer.diskStorage({
  filename: (req, file, callback) => {
    callback(null, file.originalname)
  }
})

const upload = multer({ storage })
```

### 3. Upload Process Flow
```
1. Frontend selects file
2. Form submission with multipart/form-data
3. Multer processes file on backend
4. Cloudinary uploads and returns URL
5. URL saved to database
6. Frontend displays uploaded image
```

---

## 🔄 Development Workflow

### 1. Starting the Development Environment

#### Option 1: Manual Start
```bash
# Terminal 1 - Backend
cd backend
npm run server

# Terminal 2 - Frontend  
cd frontend
npm run dev

# Terminal 3 - Admin Panel
cd admin
npm run dev
```

#### Option 2: Automated Start Script
Create a `start-dev.ps1` script in the root directory:
```powershell
# Start all services in parallel
Start-Process powershell -ArgumentList "-NoExit", "-Command", "cd backend; npm run server"
Start-Process powershell -ArgumentList "-NoExit", "-Command", "cd frontend; npm run dev"  
Start-Process powershell -ArgumentList "-NoExit", "-Command", "cd admin; npm run dev"
```

### 2. Development URLs
- **Backend API**: http://localhost:4000
- **Frontend**: http://localhost:5173
- **Admin Panel**: http://localhost:5174
- **API Documentation**: http://localhost:4000 (shows "API Working")

### 3. Hot Reload Configuration
- **Backend**: Uses nodemon for automatic restart on file changes
- **Frontend**: Vite provides instant hot module replacement
- **Admin**: Vite provides instant hot module replacement

### 4. Development Best Practices
```javascript
// Use environment variables for all configurations
const config = {
  apiUrl: import.meta.env.VITE_BACKEND_URL,
  razorpayKey: import.meta.env.VITE_RAZORPAY_KEY_ID
}

// Implement proper error boundaries
try {
  // API call
} catch (error) {
  console.error('API Error:', error)
  toast.error('Something went wrong')
}

// Use consistent API response handling
const handleApiResponse = (data) => {
  if (data.success) {
    toast.success(data.message)
    return data
  } else {
    toast.error(data.message)
    throw new Error(data.message)
  }
}
```

---

## 🚀 Production Deployment

### 1. Environment Preparation

#### Backend Production Environment
```env
# Production .env
NODE_ENV=production
PORT=4000
JWT_SECRET=your_secure_jwt_secret

# Production MongoDB (Atlas recommended)
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/prescripto

# Production Cloudinary
CLOUDINARY_NAME=prod_cloudinary_name
CLOUDINARY_API_KEY=prod_api_key
CLOUDINARY_SECRET_KEY=prod_secret_key

# Production Payment Keys
RAZORPAY_KEY_ID=prod_razorpay_key
RAZORPAY_KEY_SECRET=prod_razorpay_secret
STRIPE_SECRET_KEY=prod_stripe_secret
```

#### Frontend Production Environment
```env
# Production .env
VITE_BACKEND_URL=https://api.yourdomain.com
VITE_RAZORPAY_KEY_ID=prod_razorpay_key
VITE_STRIPE_PUBLISHABLE_KEY=prod_stripe_publishable_key
```

#### Admin Production Environment
```env
# Production .env
VITE_BACKEND_URL=https://api.yourdomain.com
```

### 2. Build Process

#### Frontend Build
```bash
cd frontend
npm run build
# Creates dist/ folder with optimized production files
```

#### Admin Panel Build
```bash
cd admin
npm run build
# Creates dist/ folder with optimized production files
```

#### Backend Preparation
```bash
cd backend
# No build step required for Node.js
# Ensure all dependencies are production-ready
npm ci --production
```

### 3. Deployment Options

#### Option 1: Separate Hosting
- **Backend**: Deploy to Heroku, Railway, or DigitalOcean
- **Frontend**: Deploy to Vercel, Netlify, or AWS S3
- **Admin**: Deploy to Vercel, Netlify (separate subdomain)

#### Option 2: Single Server Deployment
```bash
# Deploy all components to single server
# Use PM2 for process management
npm install -g pm2

# Start backend
cd backend
pm2 start server.js --name "prescripto-backend"

# Serve frontend and admin with nginx
# Frontend: yourdomain.com
# Admin: admin.yourdomain.com
# API: api.yourdomain.com
```

### 4. Domain Configuration
```
Main App: https://prescripto.com
Admin Panel: https://admin.prescripto.com  
API: https://api.prescripto.com
```

### 5. Security Considerations
```javascript
// CORS configuration for production
app.use(cors({
  origin: [
    'https://prescripto.com',
    'https://admin.prescripto.com'
  ],
  credentials: true
}))

// Environment variables validation
if (!process.env.JWT_SECRET) {
  throw new Error('JWT_SECRET is required')
}

// Rate limiting
import rateLimit from 'express-rate-limit'
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
})
app.use(limiter)
```

---

## 🧪 Testing Integration

### 1. API Testing
```javascript
// Test API endpoints with curl or Postman

// Test user registration
curl -X POST http://localhost:4000/api/user/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Test User","email":"test@test.com","password":"123456"}'

// Test doctor list (public)
curl http://localhost:4000/api/doctor/list

// Test authenticated endpoint
curl -X GET http://localhost:4000/api/user/get-profile \
  -H "token: your_jwt_token"
```

### 2. Frontend Testing
```javascript
// Test environment variables
console.log('Backend URL:', import.meta.env.VITE_BACKEND_URL)
console.log('Razorpay Key:', import.meta.env.VITE_RAZORPAY_KEY_ID)

// Test API connectivity
const testConnection = async () => {
  try {
    const response = await axios.get(`${backendUrl}/api/doctor/list`)
    console.log('API Connection Success:', response.data)
  } catch (error) {
    console.error('API Connection Failed:', error)
  }
}
```

### 3. Database Testing
```javascript
// Backend: Test database connection
mongoose.connection.on('connected', () => {
  console.log('✅ Database Connected Successfully')
})

mongoose.connection.on('error', (err) => {
  console.error('❌ Database Connection Error:', err)
})

// Test model operations
const testUser = new userModel({
  name: 'Test User',
  email: 'test@example.com',
  password: 'hashedpassword'
})

await testUser.save()
console.log('✅ User Model Test Passed')
```

### 4. Payment Testing
```javascript
// Use test payment credentials
// Razorpay Test Mode
RAZORPAY_KEY_ID=rzp_test_xxxxxxxxx
RAZORPAY_KEY_SECRET=test_secret_key

// Stripe Test Mode  
STRIPE_SECRET_KEY=sk_test_xxxxxxxxx
VITE_STRIPE_PUBLISHABLE_KEY=pk_test_xxxxxxxxx

// Test payment flow with test cards
// Razorpay: 4111 1111 1111 1111
// Stripe: 4242 4242 4242 4242
```

---

## 🔧 Troubleshooting

### 1. Common Integration Issues

#### CORS Errors
```javascript
// Error: Access to XMLHttpRequest blocked by CORS policy
// Solution: Configure CORS in backend
app.use(cors({
  origin: ['http://localhost:5173', 'http://localhost:5174'],
  credentials: true
}))
```

#### Environment Variable Issues
```javascript
// Error: Cannot read property of undefined
// Check environment variable names match exactly
console.log('All env vars:', process.env) // Backend
console.log('Vite env vars:', import.meta.env) // Frontend

// Restart development servers after .env changes
```

#### Database Connection Issues
```javascript
// Error: MongoNetworkError
// Check MongoDB URI format
// mongodb://localhost:27017/prescripto (local)
// mongodb+srv://user:pass@cluster.mongodb.net/prescripto (Atlas)

// Ensure MongoDB service is running (local)
// Check network access in MongoDB Atlas
```

#### JWT Token Issues
```javascript
// Error: JsonWebTokenError
// Check JWT_SECRET consistency across requests
// Verify token format in headers
// Check token expiration

// Debug token issues
import jwt from 'jsonwebtoken'
const decoded = jwt.verify(token, process.env.JWT_SECRET)
console.log('Token payload:', decoded)
```

### 2. Performance Issues

#### Slow API Responses
```javascript
// Add request logging middleware
app.use((req, res, next) => {
  const start = Date.now()
  res.on('finish', () => {
    const duration = Date.now() - start
    console.log(`${req.method} ${req.path} - ${duration}ms`)
  })
  next()
})

// Optimize database queries
// Use indexes on frequently queried fields
// Limit returned fields with .select()
```

#### Large Bundle Sizes
```javascript
// Analyze bundle size
npm run build -- --analyze

// Code splitting in React
const LazyComponent = React.lazy(() => import('./Component'))

// Optimize images through Cloudinary
const optimizedUrl = cloudinary.url('image_id', {
  width: 300,
  height: 200,
  crop: 'fill',
  quality: 'auto'
})
```

### 3. Security Issues

#### XSS Prevention
```javascript
// Sanitize user inputs
import validator from 'validator'

const sanitizedEmail = validator.escape(email)
const isValidEmail = validator.isEmail(email)
```

#### SQL Injection Prevention
```javascript
// Using Mongoose prevents SQL injection
// Always use parameterized queries
const user = await userModel.findOne({ email: userEmail })
// NOT: `SELECT * FROM users WHERE email = '${userEmail}'`
```

### 4. Development Debug Tools

#### Backend Debugging
```javascript
// Add debug middleware
app.use((req, res, next) => {
  console.log(`${new Date().toISOString()} - ${req.method} ${req.path}`)
  console.log('Headers:', req.headers)
  console.log('Body:', req.body)
  next()
})
```

#### Frontend Debugging
```javascript
// React Developer Tools
// Axios interceptors for request/response logging
axios.interceptors.request.use(config => {
  console.log('API Request:', config)
  return config
})

axios.interceptors.response.use(
  response => {
    console.log('API Response:', response)
    return response
  },
  error => {
    console.error('API Error:', error)
    return Promise.reject(error)
  }
)
```

---

## 📚 Additional Resources

### 1. Useful Commands
```bash
# Check if ports are in use
netstat -an | findstr :4000
netstat -an | findstr :5173
netstat -an | findstr :5174

# Kill processes on specific ports (Windows)
for /f "tokens=5" %a in ('netstat -aon ^| findstr :4000') do taskkill /f /pid %a

# Check environment variables
# Backend
node -e "console.log(process.env)"

# Frontend/Admin
npm run dev -- --debug
```

### 2. Monitoring and Logging
```javascript
// Production logging with winston
import winston from 'winston'

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
})

// Use in controllers
logger.info('User registered', { userId: user._id })
logger.error('Database error', { error: error.message })
```

### 3. Health Check Endpoints
```javascript
// Backend health check
app.get('/health', (req, res) => {
  res.json({
    status: 'OK',
    timestamp: new Date().toISOString(),
    uptime: process.uptime(),
    database: mongoose.connection.readyState === 1 ? 'connected' : 'disconnected'
  })
})
```

### 4. Backup and Recovery
```bash
# MongoDB backup
mongodump --uri="mongodb://localhost:27017/prescripto" --out=./backup

# MongoDB restore
mongorestore --uri="mongodb://localhost:27017/prescripto" ./backup/prescripto

# Environment backup
cp .env .env.backup
```

---

## ✅ Integration Checklist

### Pre-Integration Checklist
- [ ] Node.js and npm installed
- [ ] MongoDB running (local) or Atlas connection ready
- [ ] Cloudinary account setup
- [ ] Payment gateway accounts (Razorpay/Stripe)
- [ ] Git repository cloned

### Backend Setup Checklist
- [ ] Dependencies installed (`npm install`)
- [ ] Environment variables configured
- [ ] Database connection tested
- [ ] Cloudinary configuration verified
- [ ] Server starts without errors (`npm run server`)
- [ ] API endpoints accessible

### Frontend Setup Checklist
- [ ] Dependencies installed (`npm install`)
- [ ] Environment variables configured
- [ ] Backend URL accessible from frontend
- [ ] React app starts without errors (`npm run dev`)
- [ ] API calls working (check network tab)
- [ ] Authentication flow working

### Admin Panel Setup Checklist
- [ ] Dependencies installed (`npm install`)
- [ ] Environment variables configured
- [ ] Different port configured (5174)
- [ ] Admin app starts without errors (`npm run dev`)
- [ ] Admin authentication working
- [ ] Admin API calls working

### Integration Testing Checklist
- [ ] User registration and login working
- [ ] Doctor listing displayed
- [ ] Appointment booking functional
- [ ] Payment integration working
- [ ] File upload working
- [ ] Admin panel accessible
- [ ] Doctor management working
- [ ] Appointment management working

### Production Checklist
- [ ] Production environment variables set
- [ ] MongoDB Atlas connection configured
- [ ] Production payment keys configured
- [ ] Frontend and admin built successfully
- [ ] CORS configured for production domains
- [ ] SSL certificates installed
- [ ] Domain names configured
- [ ] Monitoring and logging setup

---

## 🎯 Conclusion

This integration guide provides a comprehensive roadmap for connecting the frontend, admin panel, and backend components of the Prescripto Doctor Booking System. The integration relies on:

1. **Environment Configuration**: Proper setup of environment variables across all components
2. **API Communication**: RESTful API design with consistent authentication
3. **State Management**: React Context for frontend state synchronization
4. **Database Integration**: MongoDB with Mongoose for data persistence
5. **External Services**: Cloudinary for file storage and payment gateways for transactions

By following this guide, you should be able to successfully integrate all components and have a fully functional doctor booking system running in both development and production environments.

For any issues not covered in this guide, check the troubleshooting section or refer to the individual component documentation.
