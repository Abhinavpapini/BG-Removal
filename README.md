# Background Removal App

A full-stack web application that allows users to remove backgrounds from images using AI technology. The app features user authentication, credit-based system, and payment integration.

## 🌟 Features

- **AI-Powered Background Removal**: Uses Clipdrop API for precise background removal
- **User Authentication**: Clerk-based authentication system
- **Credit System**: Users get 5 free credits upon signup, can purchase more
- **Payment Integration**: Razorpay integration for credit purchases
- **Responsive Design**: Works on desktop and mobile devices
- **Real-time Processing**: Instant background removal with loading states
- **Download Functionality**: Download processed images directly

## 🏗️ Tech Stack

### Frontend
- **React 19** - Frontend library
- **Vite** - Build tool and development server
- **Tailwind CSS 4.0** - Styling framework
- **React Router DOM** - Client-side routing
- **Clerk React** - Authentication
- **Axios** - HTTP client
- **React Toastify** - Toast notifications
- **React Icons** - Icon library

### Backend
- **Node.js** - Runtime environment
- **Express.js** - Web application framework
- **MongoDB** - Database (with Mongoose ODM)
- **JWT** - JSON Web Tokens for authentication
- **Multer** - File upload handling
- **Razorpay** - Payment gateway
- **Clipdrop API** - AI background removal
- **SVIX** - Webhook handling
- **CORS** - Cross-origin resource sharing

## 📁 Project Structure

```
client/                     # Frontend React application
├── src/
│   ├── components/        # Reusable React components
│   │   ├── BgSlider.jsx  # Background removal demo slider
│   │   ├── Footer.jsx    # Application footer
│   │   ├── Header.jsx    # Landing page header
│   │   ├── Navbar.jsx    # Navigation bar with auth
│   │   ├── Steps.jsx     # How-it-works steps
│   │   ├── Testimonials.jsx # Customer testimonials
│   │   └── Upload.jsx    # File upload component
│   ├── context/
│   │   └── AppContext.jsx # Global state management
│   ├── pages/
│   │   ├── BuyCredit.jsx # Credit purchase page
│   │   ├── Home.jsx      # Landing page
│   │   └── Result.jsx    # Image processing result
│   ├── assets/           # Static assets and configurations
│   └── App.jsx          # Main application component
├── package.json
├── vite.config.js
└── vercel.json

server/                    # Backend Express application
├── config/
│   └── mongodb.js        # Database connection
├── controllers/
│   ├── ImageController.js # Image processing logic
│   └── UserController.js  # User management & payments
├── middlewares/
│   ├── auth.js          # JWT authentication middleware
│   └── multer.js        # File upload configuration
├── models/
│   ├── transactionModel.js # Payment transaction schema
│   └── userModel.js       # User data schema
├── routes/
│   ├── ImageRoutes.js    # Image processing endpoints
│   └── userRoutes.js     # User management endpoints
├── server.js            # Main server file
└── package.json
```

## 🚀 API Endpoints

### User Management
- `POST /api/user/webhooks` - Clerk webhook handler for user events
- `GET /api/user/credits` - Get user's current credit balance
- `POST /api/user/pay-razor` - Initialize Razorpay payment
- `POST /api/user/verify-razor` - Verify payment and add credits

### Image Processing
- `POST /api/image/remove-bg` - Remove background from uploaded image

## 💡 Key Functions & Components

### Frontend Components

#### AppContext (Global State Management)
- **Functions**:
  - `loadCreditsData()` - Fetches user's credit balance
  - `removeBg(image)` - Handles background removal process
- **State**:
  - `credit` - User's current credit balance
  - `image` - Currently selected image
  - `resultImage` - Processed image with removed background

#### Navbar
- **Functions**:
  - Displays user authentication state
  - Shows credit balance for authenticated users
  - Handles sign-in/sign-out actions

#### Upload Component
- **Functions**:
  - File selection and upload
  - Integrates with `removeBg()` function
  - Supports drag-and-drop functionality

#### Result Page
- **Functions**:
  - Displays original and processed images side by side
  - Loading state during processing
  - Download functionality for processed images

#### BuyCredit Page
- **Functions**:
  - `paymentRazorpay(planId)` - Initiates payment process
  - `initPay(order)` - Handles Razorpay payment flow
  - Credit plan selection (Basic: 100 credits/$10, Advanced: 500 credits/$50, Business: 5000 credits/$250)

#### BgSlider Component
- **Functions**:
  - Interactive before/after slider
  - Demonstrates background removal quality
  - Uses CSS clip-path for visual effect

### Backend Controllers

#### UserController
- **clerkWebhooks(req, res)**:
  - Handles user creation, updates, and deletion from Clerk
  - Syncs user data with MongoDB
  - Assigns 5 free credits to new users

- **userCredits(req, res)**:
  - Returns user's current credit balance
  - Requires JWT authentication

- **paymentRazorpay(req, res)**:
  - Creates Razorpay order for credit purchase
  - Supports three plans: Basic, Advanced, Business
  - Creates transaction record in database

- **verifyRazorPay(req, res)**:
  - Verifies successful payment
  - Updates user's credit balance
  - Marks transaction as paid

#### ImageController
- **removeBgImage(req, res)**:
  - Processes uploaded image through Clipdrop API
  - Deducts 1 credit from user account
  - Returns base64 encoded processed image
  - Handles insufficient credits scenario

### Database Models

#### User Model
```javascript
{
  clerkId: String (unique),
  email: String (unique),
  photo: String,
  firstName: String,
  lastName: String,
  creditBalance: Number (default: 5)
}
```

#### Transaction Model
```javascript
{
  clerkId: String,
  plan: String,
  amount: Number,
  credits: Number,
  payment: Boolean (default: false),
  date: Number
}
```

### Middleware

#### Authentication (auth.js)
- **authUser()**: JWT token verification middleware
- Extracts clerkId from token
- Protects routes requiring authentication

#### File Upload (multer.js)
- **upload**: Multer configuration for image uploads
- Handles multipart/form-data
- Generates unique filenames with timestamps

## 🔧 Environment Variables

### Client (.env)
```env
VITE_CLERK_PUBLISHABLE_KEY=your_clerk_publishable_key
VITE_BACKEND_URL=your_backend_url
VITE_RAZORPAY_KEY_ID=your_razorpay_key_id
```

### Server (.env)
```env
MONGO_URI=your_mongodb_connection_string
CLERK_WEBHOOK_SECRET=your_clerk_webhook_secret
CLIPDROP_API=your_clipdrop_api_key
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
CURRENCY=USD
PORT=4000
```

## 📦 Installation & Setup

### Prerequisites
- Node.js (v16 or higher)
- MongoDB database
- Clerk account for authentication
- Clipdrop API key
- Razorpay account for payments

### Client Setup
```bash
cd client
npm install
npm run dev
```

### Server Setup
```bash
cd server
npm install
node server.js
```

## 🔄 Application Flow

1. **User Registration/Login**: Users authenticate via Clerk
2. **Credit Assignment**: New users receive 5 free credits
3. **Image Upload**: Users upload images via drag-and-drop or file picker
4. **Background Processing**: Image sent to Clipdrop API for processing
5. **Credit Deduction**: 1 credit deducted per successful processing
6. **Result Display**: Original and processed images shown side by side
7. **Download**: Users can download the processed image
8. **Credit Purchase**: Users can buy more credits via Razorpay

## 🎨 Pricing Plans

- **Basic**: $10 for 100 credits
- **Advanced**: $50 for 500 credits  
- **Business**: $250 for 5000 credits

## 🔒 Security Features

- JWT-based authentication
- Webhook signature verification
- Input validation and sanitization
- Secure file upload handling
- Payment verification

## 🚀 Deployment

The application is configured for deployment on Vercel with `vercel.json` files in both client and server directories.

## 📱 Responsive Design

- Mobile-first approach using Tailwind CSS
- Responsive grid layouts
- Touch-friendly interfaces
- Optimized for various screen sizes

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the ISC License.

## 🔗 External Services

- **Clerk**: User authentication and management
- **Clipdrop API**: AI-powered background removal
- **Razorpay**: Payment processing
- **MongoDB**: Database storage
- **Vercel**: Deployment platform

---

Built with ❤️ by Arbaz.dev
