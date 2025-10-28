# Express Master - Acquisitions API

A production-ready **Express.js** backend application with modern security practices, JWT-based stateless authentication, and serverless PostgreSQL integration.

## 🎯 Project Overview

**Express Master** is a robust REST API built with **Node.js** and **Express**, designed for user authentication and management. It implements enterprise-grade security measures, rate limiting, bot detection, and follows clean architecture principles with separation of concerns.

---

## 🛠️ Tech Stack

### **Core Framework & Runtime**
- **[Express.js](https://expressjs.com/) v5.1.0** – Lightweight, fast, and unopinionated HTTP server framework
- **Node.js** – JavaScript runtime with native module support (ES modules)

### **Database & ORM**
- **[PostgreSQL](https://www.postgresql.org/)** – Powerful, open-source relational database
- **[Neon](https://neon.tech/)** – Serverless PostgreSQL platform with auto-scaling and HTTP API
- **[Drizzle ORM](https://orm.drizzle.team/) v0.44.5** – Modern, TypeScript-first SQL ORM with excellent developer experience
- **[Drizzle Kit](https://orm.drizzle.team/kit-docs/overview)** – Schema migration and studio tools

### **Authentication & Security**
- **[JWT (jsonwebtoken)](https://www.npmjs.com/package/jsonwebtoken) v9.0.2** – Stateless token-based authentication
- **[bcrypt](https://www.npmjs.com/package/bcrypt) v6.0.0** – Password hashing with salt rounds
- **[Helmet.js](https://helmetjs.github.io/) v8.1.0** – HTTP security headers middleware
- **[Arcjet](https://arcjet.com/) v1.0.0-beta.11** – AI-powered security with bot detection, rate limiting, and DDoS protection
- **[cookie-parser](https://www.npmjs.com/package/cookie-parser) v1.4.7** – Cookie parsing middleware

### **Data Validation**
- **[Zod](https://zod.dev/) v4.1.5** – TypeScript-first schema validation with runtime checks

### **Logging & Monitoring**
- **[Winston](https://www.npmjs.com/package/winston) v3.17.0** – Enterprise-level logging library with multiple transports
- **[Morgan](https://www.npmjs.com/package/morgan) v1.10.1** – HTTP request logger middleware

### **Utilities**
- **[CORS](https://www.npmjs.com/package/cors) v2.8.5** – Cross-Origin Resource Sharing middleware
- **[dotenv](https://www.npmjs.com/package/dotenv) v17.2.2** – Environment variable management

### **Development Tools**
- **[ESLint](https://eslint.org/)** – Code quality and style linting
- **[Prettier](https://prettier.io/)** – Code formatter
- **[Jest](https://jestjs.io/) v30.1.3** – Testing framework with supertest for HTTP assertions
- **[Supertest](https://www.npmjs.com/package/supertest) v7.1.4** – HTTP assertion library

### **DevOps & Containerization**
- **[Docker](https://www.docker.com/)** – Application containerization
- **[Docker Compose](https://docs.docker.com/compose/)** – Multi-container orchestration

---

## 📁 Project Structure

```
express-master/
├── src/
│   ├── app.js                    # Express app configuration
│   ├── index.js                  # Entry point
│   ├── server.js                 # Server initialization
│   │
│   ├── config/
│   │   ├── arcjet.js            # Arcjet security config
│   │   ├── database.js          # Neon PostgreSQL connection
│   │   └── logger.js            # Winston logger setup
│   │
│   ├── controllers/
│   │   ├── auth.controller.js   # Authentication logic (signup, signin, signout)
│   │   └── users.controller.js  # User management logic
│   │
│   ├── middleware/
│   │   ├── auth.middleware.js   # JWT token verification
│   │   └── security.middleware.js # Rate limiting & bot detection
│   │
│   ├── models/
│   │   └── user.model.js        # Drizzle ORM schema
│   │
│   ├── routes/
│   │   ├── auth.routes.js       # /api/auth endpoints
│   │   └── users.routes.js      # /api/users endpoints
│   │
│   ├── services/
│   │   ├── auth.service.js      # Business logic (hash, compare, create, auth user)
│   │   └── users.service.js     # User operations service
│   │
│   ├── utils/
│   │   ├── cookies.js           # Cookie utilities
│   │   ├── format.js            # Response formatting
│   │   └── jwt.js               # JWT sign/verify utilities
│   │
│   └── validations/
│       ├── auth.validation.js   # Zod schemas for signup/signin
│       └── users.validation.js  # Zod schemas for users
│
├── tests/
│   └── app.test.js              # Jest test suite
│
├── drizzle/                      # Database migrations
│   ├── 0000_happy_bedlam.sql
│   └── meta/
│
├── coverage/                     # Test coverage reports
│
├── scripts/
│   ├── dev.sh                   # Development Docker setup
│   └── prod.sh                  # Production Docker setup
│
├── workflows/                    # GitHub Actions CI/CD
│   ├── docker-build-and-push.yml
│   ├── lint-and-format.yml
│   └── tests.yml
│
├── Dockerfile                    # Container image definition
├── docker-compose.dev.yml        # Dev environment (local Neon)
├── docker-compose.prod.yml       # Prod environment (Neon serverless)
├── drizzle.config.js             # Migration config
├── eslint.config.js              # ESLint rules
├── jest.config.mjs               # Jest configuration
├── package.json                  # Dependencies & scripts
└── README.md                     # This file
```

---

## 🏗️ Architecture

### **Layered Architecture**

```
Routes → Controllers → Services → Database
         ↓
      Middleware (Auth, Security, Validation)
         ↓
      Utilities (JWT, Cookies, Formatting)
```

1. **Routes** – Define HTTP endpoints
2. **Controllers** – Handle request/response, call services
3. **Services** – Core business logic (auth, user ops)
4. **Database** – Drizzle ORM queries
5. **Middleware** – Cross-cutting concerns (auth, rate limiting, security)
6. **Utilities** – Reusable functions (JWT, cookies, formatting)

### **Authentication Flow**

```
User Signup/Signin
    ↓
Validate Input (Zod)
    ↓
Hash Password (bcrypt) / Verify Password
    ↓
Create User / Fetch User
    ↓
Generate JWT Token
    ↓
Set HttpOnly Cookie
    ↓
Return User + Token
```

---

## 🔐 Security Features

### **1. JWT-Based Stateless Authentication**
- Tokens are signed with a secret and verified on each request
- No server-side session storage required
- Claims include `id`, `email`, `role`, and expiration (`1d`)

### **2. Password Security**
- Passwords hashed with **bcrypt** (10 salt rounds)
- Never stored in plain text
- Compared securely on login

### **3. HTTP Security Headers (Helmet)**
- Mitigates XSS, clickjacking, MIME sniffing attacks
- Sets `Content-Security-Policy`, `X-Frame-Options`, etc.

### **4. Bot Detection & Rate Limiting (Arcjet)**
- **Shield Mode**: Detects and blocks malicious traffic
- **Bot Detection**: Blocks automated requests (except search engines)
- **Sliding Window Rate Limiting**: Role-based limits
  - **Admin**: 20 req/min
  - **User**: 10 req/min
  - **Guest**: 5 req/min

### **5. Cookie Security**
- **HttpOnly**: Prevents XSS cookie theft
- **Secure**: Only sent over HTTPS (production)
- **SameSite=Strict**: Prevents CSRF attacks

### **6. Input Validation**
- **Zod** schemas validate all inputs
- Prevents injection attacks and data inconsistencies

### **7. CORS Protection**
- Configurable cross-origin requests
- Whitelist trusted domains in production

---

## 🚀 Getting Started

### **Prerequisites**
- Node.js 18+
- npm or yarn
- PostgreSQL (or Neon serverless account)
- Docker & Docker Compose (optional)

### **1. Clone & Install**

```bash
git clone https://github.com/hongminglow/express-master.git
cd express-master
npm install
```

### **2. Environment Setup**

Create `.env` file in the root directory:

```env
# Server
NODE_ENV=development
PORT=3000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/acquisitions

# JWT
JWT_SECRET=your-secret-key-please-change-in-production

# Arcjet Security
ARCJET_KEY=your-arcjet-key

# Logging
LOG_LEVEL=info
```

### **3. Database Setup**

**For development (local PostgreSQL):**

```bash
npm run db:generate   # Generate migration
npm run db:migrate    # Run migrations
```

**For production (Neon serverless):**

Update `DATABASE_URL` in `.env` to your Neon connection string, then run migrations.

**View database in studio:**

```bash
npm run db:studio
```

### **4. Start Server**

**Development (with auto-reload):**

```bash
npm run dev
```

**Production:**

```bash
npm start
```

**Using Docker:**

```bash
# Development
npm run dev:docker

# Production
npm run prod:docker
```

### **5. Test API**

```bash
# Health check
curl http://localhost:3000/health

# API status
curl http://localhost:3000/api

# Sign up
curl -X POST http://localhost:3000/api/auth/sign-up \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com","password":"password123"}'

# Sign in
curl -X POST http://localhost:3000/api/auth/sign-in \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'
```

---

## 📚 API Endpoints

### **Authentication**

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/sign-up` | Register new user |
| POST | `/api/auth/sign-in` | Login user |
| POST | `/api/auth/sign-out` | Logout user |

### **Users**

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/users` | Fetch all users (admin only) |
| GET | `/api/users/:id` | Fetch user by ID |
| PUT | `/api/users/:id` | Update user |
| DELETE | `/api/users/:id` | Delete user |

### **Health & Status**

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/` | Welcome message |
| GET | `/health` | Health check (status, uptime) |
| GET | `/api` | API status |

---

## 🧪 Testing

Run test suite with coverage:

```bash
npm test
```

Run tests with watch mode:

```bash
npm test -- --watch
```

View coverage report:

```bash
open coverage/lcov-report/index.html
```

---

## 📋 Scripts

```json
{
  "start": "node src/index.js",                  // Start production server
  "dev": "node --watch src/index.js",            // Start with auto-reload
  "lint": "eslint .",                            // Check code style
  "lint:fix": "eslint . --fix",                  // Auto-fix linting issues
  "format": "prettier --write .",                // Format code
  "format:check": "prettier --check .",          // Check formatting
  "db:generate": "drizzle-kit generate",         // Generate migration
  "db:migrate": "drizzle-kit migrate",           // Run migration
  "db:studio": "drizzle-kit studio",             // Open DB studio
  "dev:docker": "sh ./scripts/dev.sh",           // Docker dev setup
  "prod:docker": "sh ./scripts/prod.sh",         // Docker prod setup
  "test": "NODE_OPTIONS=--experimental-vm-modules jest"  // Run tests
}
```

---

## 🔄 Data Flow Example: User Signup

```
1. Client POST /api/auth/sign-up with { name, email, password, role }
                ↓
2. Express middleware: helmet, cors, bodyParser, cookieParser
                ↓
3. Security middleware: Arcjet bot detection & rate limiting
                ↓
4. Controller (signup):
   - Validate input with Zod
   - Call auth.service.createUser()
                ↓
5. Service (createUser):
   - Hash password with bcrypt
   - Check if email exists
   - Insert into users table (Drizzle ORM)
   - Return user (without password)
                ↓
6. Controller:
   - Generate JWT token
   - Set HttpOnly cookie
   - Return 201 + user + token
                ↓
7. Client stores token/cookie for future requests
```

---

## 🌐 Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `NODE_ENV` | Runtime environment | `development`, `production` |
| `PORT` | Server port | `3000` |
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://user:pass@host/db` |
| `JWT_SECRET` | Secret for signing tokens | `your-secret-key` |
| `JWT_EXPIRES_IN` | Token expiration | `1d` |
| `ARCJET_KEY` | Arcjet API key for security | `your-arcjet-key` |

---

## 📦 Dependencies Overview

### **Production Dependencies**

| Package | Version | Purpose |
|---------|---------|---------|
| express | ^5.1.0 | HTTP server framework |
| jsonwebtoken | ^9.0.2 | JWT creation & verification |
| bcrypt | ^6.0.0 | Password hashing |
| drizzle-orm | ^0.44.5 | SQL ORM |
| @neondatabase/serverless | ^1.0.1 | Neon DB client |
| zod | ^4.1.5 | Schema validation |
| helmet | ^8.1.0 | Security headers |
| @arcjet/node | ^1.0.0-beta.11 | Bot detection & rate limiting |
| winston | ^3.17.0 | Logging |
| morgan | ^1.10.1 | HTTP request logging |
| cors | ^2.8.5 | CORS middleware |
| cookie-parser | ^1.4.7 | Cookie parsing |
| dotenv | ^17.2.2 | Environment variables |

### **Development Dependencies**

| Package | Version | Purpose |
|---------|---------|---------|
| jest | ^30.1.3 | Testing framework |
| supertest | ^7.1.4 | HTTP testing |
| eslint | ^9.35.0 | Linting |
| prettier | ^3.6.2 | Code formatting |
| drizzle-kit | ^0.31.4 | DB migrations |

---

## 🐛 Troubleshooting

### **"Automated requests are not allowed" (403)**
- **Cause**: Arcjet bot detection triggered by Postman default user-agent
- **Solution**: Add `User-Agent: Mozilla/5.0` header in Postman, or disable bot detection in development

### **Database Connection Failed**
- Ensure `DATABASE_URL` is correct in `.env`
- For Neon development, ensure Docker container `neon-local` is running
- Check `NODE_ENV` is set correctly

### **JWT Token Invalid**
- Verify `JWT_SECRET` matches between sign and verify
- Check token expiration with `jwt.decode(token)`
- Ensure token format: `Bearer <token>` in headers

### **Port Already in Use**
- Change `PORT` in `.env`
- Or kill existing process: `lsof -ti:3000 | xargs kill -9`

---

## 📖 Documentation Links

- [Express.js Docs](https://expressjs.com/)
- [Drizzle ORM](https://orm.drizzle.team/)
- [Neon PostgreSQL](https://neon.tech/docs)
- [JWT Best Practices](https://tools.ietf.org/html/rfc7519)
- [Arcjet Security](https://docs.arcjet.com/)
- [Zod Validation](https://zod.dev/)

---

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

---

## 📝 License

This project is licensed under the **ISC License** – see `package.json` for details.

---

## 👨‍💻 Author

**hongminglow** – [GitHub Profile](https://github.com/hongminglow)

---

## 🙏 Acknowledgments

- Express.js community
- Drizzle ORM for excellent TypeScript support
- Neon for serverless PostgreSQL
- Arcjet for security infrastructure

---

**Last Updated**: October 29, 2025

**Status**: Production Ready ✅
