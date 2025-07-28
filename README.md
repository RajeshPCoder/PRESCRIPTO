# Prescripto - Doctor Booking System

A comprehensive full-stack web application for booking medical appointments with trusted doctors. Built with React.js, Node.js, Express.js, and MongoDB.

## 🌟 Features

### Frontend (Patient Portal)
- **User Authentication**: Secure login and registration system
- **Doctor Discovery**: Browse doctors by speciality
- **Appointment Booking**: Easy-to-use booking system with time slots
- **User Profile**: Manage personal information and view appointment history
- **Responsive Design**: Mobile-friendly interface built with Tailwind CSS
- **Payment Integration**: Razorpay and Stripe payment gateways

### Admin Panel
- **Doctor Management**: Add, edit, and remove doctors
- **Appointment Management**: View and manage all appointments
- **Dashboard Analytics**: Overview of patients, doctors, and earnings
- **Secure Authentication**: Admin-only access with JWT tokens

### Backend API
- **RESTful API**: Clean and organized API endpoints
- **JWT Authentication**: Secure token-based authentication
- **File Upload**: Cloudinary integration for image uploads
- **Database**: MongoDB with Mongoose ODM
- **Payment Processing**: Integrated payment gateway support

## 🛠️ Tech Stack

### Frontend
- **React.js** - UI library
- **Vite** - Build tool and development server
- **Tailwind CSS** - Utility-first CSS framework
- **React Router** - Client-side routing
- **Axios** - HTTP client for API calls
- **React Toastify** - Toast notifications

### Backend
- **Node.js** - Runtime environment
- **Express.js** - Web framework
- **MongoDB** - NoSQL database
- **Mongoose** - MongoDB object modeling
- **JWT** - JSON Web Tokens for authentication
- **Bcrypt** - Password hashing
- **Multer** - File upload middleware
- **Cloudinary** - Cloud-based image management

### Payment Gateways
- **Razorpay** - Indian payment gateway
- **Stripe** - International payment processing

## 📁 Project Structure

```
prescripto-full-stack/
├── frontend/           # React.js patient portal
├── admin/             # React.js admin panel
├── backend/           # Node.js/Express.js API
├── README.md
├── .gitignore
└── How_To_Run_Project.pdf
```

## 🚀 Quick Start

### Prerequisites
- Node.js (v14 or higher)
- MongoDB Atlas account or local MongoDB installation
- Cloudinary account for image uploads
- Razorpay/Stripe accounts for payment processing

### Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd prescripto-full-stack
   ```

2. **Install Backend Dependencies**
   ```bash
   cd backend
   npm install
   ```

3. **Install Frontend Dependencies**
   ```bash
   cd ../frontend
   npm install
   ```

4. **Install Admin Panel Dependencies**
   ```bash
   cd ../admin
   npm install
   ```

### Environment Setup

1. **Backend Environment Variables**
   Create a `.env` file in the `backend` directory:
   ```env
   CURRENCY="INR"
   JWT_SECRET="your-jwt-secret"
   
   # Admin Panel Credentials
   ADMIN_EMAIL="admin@example.com"
   ADMIN_PASSWORD="your-admin-password"
   
   # MongoDB Setup
   MONGODB_URI="your-mongodb-connection-string"
   
   # Cloudinary Setup
   CLOUDINARY_NAME="your-cloudinary-name"
   CLOUDINARY_API_KEY="your-cloudinary-api-key"
   CLOUDINARY_SECRET_KEY="your-cloudinary-secret-key"
   
   # Payment Gateway Setup
   RAZORPAY_KEY_ID="your-razorpay-key-id"
   RAZORPAY_KEY_SECRET="your-razorpay-key-secret"
   STRIPE_SECRET_KEY="your-stripe-secret-key"
   ```

2. **Frontend Environment Variables**
   Create a `.env` file in the `frontend` directory:
   ```env
   VITE_BACKEND_URL="http://localhost:4000"
   VITE_RAZORPAY_KEY_ID="your-razorpay-key-id"
   ```

### Running the Application

1. **Start the Backend Server**
   ```bash
   cd backend
   npm run server
   # Server will run on http://localhost:4000
   ```

2. **Start the Frontend (Patient Portal)**
   ```bash
   cd frontend
   npm run dev
   # Frontend will run on http://localhost:5173
   ```

3. **Start the Admin Panel**
   ```bash
   cd admin
   npm run dev
   # Admin panel will run on http://localhost:5174
   ```

## 📊 API Endpoints

### User Routes
- `POST /api/user/register` - User registration
- `POST /api/user/login` - User login
- `GET /api/user/get-profile` - Get user profile
- `POST /api/user/update-profile` - Update user profile
- `POST /api/user/book-appointment` - Book appointment
- `GET /api/user/appointments` - Get user appointments

### Doctor Routes
- `GET /api/doctor/list` - Get all doctors
- `POST /api/doctor/login` - Doctor login
- `GET /api/doctor/appointments` - Get doctor appointments
- `POST /api/doctor/complete-appointment` - Mark appointment as completed

### Admin Routes
- `POST /api/admin/login` - Admin login
- `POST /api/admin/add-doctor` - Add new doctor
- `GET /api/admin/appointments` - Get all appointments
- `GET /api/admin/dashboard` - Get dashboard data

## 🔐 Authentication

The application uses JWT (JSON Web Tokens) for authentication:
- **Users**: Can register, login, and book appointments
- **Doctors**: Can login and manage their appointments
- **Admin**: Can manage doctors and view all appointments

## 💳 Payment Integration

The system supports multiple payment gateways:
- **Razorpay**: For Indian users (INR currency)
- **Stripe**: For international payments

## 🖼️ Image Upload

Profile pictures and doctor images are handled through Cloudinary for optimized storage and delivery.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 📞 Support

For support, email support@prescripto.com or create an issue in the repository.

## 🙏 Acknowledgments

- React.js team for the amazing UI library
- Express.js for the robust backend framework
- MongoDB for the flexible database solution
- Tailwind CSS for the utility-first CSS framework
- All the open-source contributors who made this project possible

---

**Made with ❤️ by the Prescripto Team**
