# MedBook WB - Doctor Booking Portal

A complete doctor booking system with user authentication, OTP verification, and persistent database storage.

## 🚀 Features

- ✅ **User Registration & Login** with email OTP verification
- ✅ **Persistent Database** - SQLite database stores all user data
- ✅ **Password Security** - Bcrypt hashing for password storage
- ✅ **Email OTP** - EmailJS integration for sending OTPs
- ✅ **Session Management** - Users stay logged in after browser restart
- ✅ **Appointment Booking** - Book and manage doctor appointments

## 📁 Project Structure

```
doctor_booking_Medbook/
├── backend_doctorbooking/     # Node.js backend
│   ├── server.js              # Main Express server
│   ├── database.js            # SQLite database logic
│   ├── package.json           # Backend dependencies
│   ├── .env                   # Configuration (EmailJS keys)
│   └── medbook.db            # SQLite database (auto-created)
├── index.html                 # Main application page
├── login.html                 # Login/Signup page
├── doctor.css                 # Styles
└── monojit.js                 # Frontend JavaScript
```

## 🛠️ Setup Instructions

### Step 1: Install Backend Dependencies

```bash
cd backend_doctorbooking
npm install
```

This will install:
- `express` - Web server
- `cors` - Cross-origin requests
- `bcryptjs` - Password hashing
- `better-sqlite3` - Database
- `dotenv` - Environment variables

### Step 2: Configure EmailJS

1. Go to [EmailJS Dashboard](https://dashboard.emailjs.com/)
2. Create an account (if you haven't)
3. **Create Email Service:**
   - Click "Email Services" → "Add New Service"
   - Choose Gmail or any email provider
   - Connect your account
   - Copy the **Service ID** (e.g., `service_abc123`)

4. **Create Email Template:**
   - Click "Email Templates" → "Create New Template"
   - Set **To Email**: `{{to_email}}`
   - Set **Subject**: `Your MedBook OTP Code`
   - Set **Body**:
     ```
     Hi {{to_name}},

     Your OTP code is: {{otp_code}}

     This code will expire in 10 minutes.

     Thanks,
     MedBook Team
     ```
   - Save and copy the **Template ID** (e.g., `template_xyz456`)

5. **Get Public Key:**
   - Click your profile → "Account" → "General"
   - Copy your **Public Key**

6. **Update Backend Configuration:**
   
   Open `backend_doctorbooking/.env` and replace:
   ```env
   EMAILJS_PUBLIC_KEY=your_public_key_here
   EMAILJS_SERVICE_ID=your_service_id_here
   EMAILJS_TEMPLATE_ID=your_template_id_here
   ```

7. **Update Frontend Configuration:**
   
   In `login.html` (around line 115), update:
   ```javascript
   const EMAILJS_PUBLIC_KEY  = 'your_public_key_here';
   const EMAILJS_SERVICE_ID  = 'your_service_id_here';
   const EMAILJS_TEMPLATE_ID = 'your_template_id_here';
   ```

### Step 3: Start the Backend Server

```bash
cd backend_doctorbooking
npm start
```

You should see:
```
╔════════════════════════════════════════════════╗
║  🏥 MedBook Backend Server Running            ║
║  📡 Port: 3001                                 ║
║  🌍 http://localhost:3001                      ║
║  💾 Database: SQLite (medbook.db)             ║
╚════════════════════════════════════════════════╝
✅ Database initialized successfully
```

### Step 4: Open the Frontend

1. Open `index.html` in your browser
2. Or use a local server:
   ```bash
   # Using Python
   python -m http.server 3000
   
   # Using Node.js http-server
   npx http-server -p 3000
   ```
3. Visit `http://localhost:3000`

## 🔑 How It Works

### 1. User Signup
- User fills name, email, mobile, password
- Frontend sends data to `/api/auth/signup`
- Backend validates and stores in database with hashed password
- User redirected to login

### 2. User Login
- User enters email
- Frontend calls `/api/auth/generate-otp`
- Backend generates 6-digit OTP and stores in database
- Frontend sends OTP via EmailJS
- User enters OTP
- Frontend calls `/api/auth/verify-otp`
- Backend verifies OTP and returns user data
- User logged in

### 3. Data Persistence
- All users stored in `medbook.db` SQLite database
- Passwords hashed with bcrypt
- OTPs expire after 10 minutes
- Users can logout and re-login anytime

## 📊 Database Schema

### Users Table
```sql
id           INTEGER PRIMARY KEY
name         TEXT NOT NULL
email        TEXT UNIQUE NOT NULL
mobile       TEXT NOT NULL
password     TEXT NOT NULL (hashed)
created_at   DATETIME
last_login   DATETIME
```

### OTP Verifications Table
```sql
id           INTEGER PRIMARY KEY
email        TEXT NOT NULL
otp          TEXT NOT NULL
purpose      TEXT NOT NULL (signup/login)
expires_at   DATETIME NOT NULL
verified     BOOLEAN DEFAULT 0
created_at   DATETIME
```

## 🔧 API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/api/auth/signup` | Register new user |
| POST | `/api/auth/generate-otp` | Generate OTP for login |
| POST | `/api/auth/verify-otp` | Verify OTP and login |
| GET | `/api/user/:email` | Get user data by email |
| POST | `/api/appointments` | Create appointment |
| GET | `/api/appointments/:email` | Get user appointments |

## 🐛 Troubleshooting

### "Failed to connect to backend"
- Make sure backend is running on port 3001
- Check `API_BASE_URL` in `login.html` matches your backend URL

### "Email failed to send (400)"
- Verify EmailJS credentials are correct
- Check Service ID and Template ID match your dashboard
- Make sure `to_email` variable exists in your EmailJS template

### "No account found with this email"
- User hasn't signed up yet
- Click "Sign Up" tab and create an account first

### Database locked error
- Close all connections to `medbook.db`
- Restart the backend server

## 🔒 Security Notes

### For Production:
1. **Change SESSION_SECRET** in `.env` to a random string
2. **Use HTTPS** for all API calls
3. **Add rate limiting** to prevent OTP spam
4. **Remove development OTP** from API response
5. **Add CORS whitelist** in production
6. **Use environment variables** for all sensitive data

## 📝 Future Enhancements

- [ ] Password reset via email
- [ ] Admin dashboard
- [ ] Doctor availability calendar
- [ ] Payment integration
- [ ] SMS OTP option
- [ ] Email verification on signup
- [ ] User profile management

## 🤝 Support

For issues or questions, check:
1. Backend logs in terminal
2. Browser console (F12)
3. EmailJS dashboard for email delivery status

---

Made with ❤️ for MedBook WB
