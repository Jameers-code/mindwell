# 🩺 MindWell - Your Personal AI Health Assistant

## 🌟 Introduction

MindWell is a cutting-edge AI-powered health assistant designed to help you monitor and manage your mental well-being. 
Built with Next.js, Clerk, Stripe, Prisma, and MongoDB, MindWell provides real-time insights and personalized recommendations to keep you on top of your health.

## 🚀 Features

- **Real-time Health Monitoring:** Track your symptoms, medications, and overall wellness in one place.
- **AI-Powered Chat:** Get instant answers and health tips from your AI assistant.
- **Premium Features:** Unlock additional tools and functionalities with a one-time payment of $9.
- **Seamless Integration:** Built with modern tech stack for smooth performance and scalability.

## 💻 Tech Stack

- Tailwind CSS
- Shadcn UI
- Headless UI
- Clerk
- Tanstack Query
- Framer Motion
- Stripe
- Zustand
- Zod
- Gemini API
- Prisma
- MongoDB

## 🛠️ Installation & Setup

### Prerequisites
- Node.js 18+ 
- npm or pnpm

### Getting Started

1. Clone the repository:
```bash
git clone https://github.com/Jameers-code/mindwell.git
cd mindwell
```

2. Install dependencies:
```bash
npm install
# or
pnpm install
```

3. Set up environment variables:
Create a `.env.local` file in the root directory:
```bash
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=your_key_here
CLERK_SECRET_KEY=your_key_here
DATABASE_URL=your_mongodb_connection_string
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=your_key_here
STRIPE_SECRET_KEY=your_key_here
NEXT_PUBLIC_GEMINI_API_KEY=your_key_here
```

4. Set up Prisma:
```bash
npx prisma generate
npx prisma db push
```

5. Run the development server:
```bash
npm run dev
# or
pnpm dev
```

Open [http://localhost:3000](http://localhost:3000) to see the app.

## 📝 Building for Production

```bash
npm run build
npm start
```

## 📄 License

This project is open source and available under the MIT License.

---

Built with ❤️ by [Jameers-code](https://github.com/Jameers-code)
