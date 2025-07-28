# Contributing to Prescripto

Thank you for your interest in contributing to Prescripto! This document provides guidelines and instructions for contributing to this project.

## 🤝 How to Contribute

### Reporting Issues

1. **Search existing issues** first to avoid duplicates
2. **Use the issue template** when creating new issues
3. **Provide detailed information** including:
   - Steps to reproduce the issue
   - Expected vs actual behavior
   - Screenshots or error messages
   - Environment details (OS, browser, Node.js version)

### Suggesting Features

1. **Check the roadmap** to see if the feature is already planned
2. **Create a feature request** with detailed description
3. **Explain the use case** and how it benefits users
4. **Consider implementation complexity** and breaking changes

## 🔧 Development Setup

### Prerequisites

- Node.js (v14 or higher)
- npm or yarn
- MongoDB (local or Atlas)
- Git

### Getting Started

1. **Fork the repository**
   ```bash
   git clone https://github.com/yourusername/prescripto-full-stack.git
   cd prescripto-full-stack
   ```

2. **Install dependencies**
   ```bash
   # Backend
   cd backend && npm install
   
   # Frontend
   cd ../frontend && npm install
   
   # Admin
   cd ../admin && npm install
   ```

3. **Set up environment variables**
   - Copy `.env.example` to `.env` in each directory
   - Fill in your configuration values

4. **Start development servers**
   ```bash
   # Terminal 1 - Backend
   cd backend && npm run server
   
   # Terminal 2 - Frontend
   cd frontend && npm run dev
   
   # Terminal 3 - Admin
   cd admin && npm run dev
   ```

## 📝 Code Style and Standards

### General Guidelines

- **Follow existing code style** and patterns
- **Write clear, descriptive commit messages**
- **Add comments** for complex logic
- **Keep functions small** and focused
- **Use meaningful variable names**

### JavaScript/React Guidelines

- Use **ES6+ features** where appropriate
- Follow **React best practices**:
  - Use functional components with hooks
  - Implement proper error boundaries
  - Avoid unnecessary re-renders
- Use **destructuring** for props and state
- Implement **PropTypes** or TypeScript for type checking

### CSS/Styling Guidelines

- Use **Tailwind CSS classes** primarily
- Follow **mobile-first** responsive design
- Keep **custom CSS minimal**
- Use **semantic class names** when needed

### Backend Guidelines

- Follow **RESTful API** conventions
- Implement proper **error handling**
- Use **middleware** for common functionality
- Add **input validation** for all endpoints
- Write **secure code** (sanitize inputs, use HTTPS, etc.)

## 🧪 Testing

### Running Tests

```bash
# Backend tests
cd backend && npm test

# Frontend tests
cd frontend && npm test

# Admin tests
cd admin && npm test
```

### Writing Tests

- **Write unit tests** for utility functions
- **Write integration tests** for API endpoints
- **Write component tests** for React components
- **Aim for good coverage** but focus on critical paths

## 📋 Pull Request Process

### Before Submitting

1. **Create a feature branch**
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Follow coding standards
   - Add tests if applicable
   - Update documentation

3. **Test thoroughly**
   - Run all tests
   - Test in multiple browsers
   - Check responsive design

4. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add user profile validation"
   ```

### Commit Message Format

Use the conventional commits format:

- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation changes
- `style:` Code style changes (formatting, etc.)
- `refactor:` Code refactoring
- `test:` Adding or updating tests
- `chore:` Maintenance tasks

### Submitting the PR

1. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

2. **Create a Pull Request**
   - Use a clear, descriptive title
   - Fill out the PR template
   - Link related issues
   - Add screenshots for UI changes

3. **Respond to feedback**
   - Address review comments promptly
   - Make requested changes
   - Keep discussions constructive

## 🏗️ Project Structure

```
prescripto-full-stack/
├── backend/              # Express.js API server
│   ├── config/          # Database and service configs
│   ├── controllers/     # Route handlers
│   ├── middleware/      # Custom middleware
│   ├── models/         # Database models
│   └── routes/         # API routes
├── frontend/           # React.js patient portal
│   ├── src/
│   │   ├── components/ # Reusable components
│   │   ├── pages/      # Page components
│   │   ├── context/    # React context
│   │   └── assets/     # Static assets
└── admin/              # React.js admin panel
    └── src/
        ├── components/
        ├── pages/
        └── context/
```

## 🔒 Security Guidelines

### Backend Security

- **Validate all inputs** on the server side
- **Use parameterized queries** to prevent SQL injection
- **Implement rate limiting** for API endpoints
- **Use HTTPS** in production
- **Sanitize user-generated content**
- **Keep dependencies updated**

### Frontend Security

- **Validate inputs** on the client side (but don't rely on it)
- **Sanitize display data** to prevent XSS
- **Use HTTPS** for all API calls
- **Don't store sensitive data** in localStorage
- **Implement proper authentication** flows

## 📚 Documentation

### Code Documentation

- **Document complex functions** with JSDoc comments
- **Add README files** for new modules
- **Update API documentation** for endpoint changes
- **Include examples** in documentation

### User Documentation

- **Update user guides** for new features
- **Add screenshots** for UI changes
- **Keep installation instructions** current
- **Document breaking changes**

## 🐛 Debugging Tips

### Common Issues

1. **CORS errors**: Check backend CORS configuration
2. **Authentication failures**: Verify JWT token handling
3. **Database connection**: Check MongoDB URI and network
4. **File upload issues**: Verify Cloudinary configuration
5. **Payment failures**: Check payment gateway credentials

### Debugging Tools

- Use **browser dev tools** for frontend issues
- Use **Postman** for API testing
- Check **server logs** for backend issues
- Use **React Developer Tools** for component debugging

## 📞 Getting Help

- **GitHub Issues**: For bug reports and feature requests
- **Discussions**: For questions and general discussion
- **Discord/Slack**: For real-time community chat
- **Email**: For security issues or private concerns

## 🎉 Recognition

Contributors will be recognized in:
- README.md contributors section
- Release notes for significant contributions
- Hall of Fame for long-term contributors

Thank you for contributing to Prescripto! 🚀
