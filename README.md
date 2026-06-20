{
  "name": "adul-ai",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "next": "14.2.5",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "lucide-react": "^0.383.0"
  },
  "devDependencies": {
    "tailwindcss": "^3.4.4",
    "postcss": "^8.4.38",
    "autoprefixer": "^10.4.19"
  }
}/** @type {import('next').NextConfig} */
const nextConfig = {};

module.exports = nextConfig;module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./app/**/*.{js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};node_modules/
.next/
.env
.env.local@tailwind base;
@tailwind components;
@tailwind utilities;

html, body {
  margin: 0;
  padding: 0;
  background: #171717;
}import "./globals.css";

export const metadata = {
  title: "Adul AI",
  description: "Tumhara apna AI saathi — padhai, baat-cheet, aur creative ideas",
  manifest: "/manifest.json",
};

export const viewport = {
  themeColor: "#171717",
  width: "device-width",
  initialScale: 1,
};

export default function RootLayout({ children }) {
  return (
    <html lang="hi">
      <body>{children}</body>
    </html>
  );
}import AdulAI from "./AdulAI";

export default function Page() {
  return <AdulAI />;
}export async function POST(request) {
  try {
    const { messages, system } = await request.json();

    const apiKey = process.env.ANTHROPIC_API_KEY;
    if (!apiKey) {
      return Response.json(
        { error: "Server mein ANTHROPIC_API_KEY set nahi hai." },
        { status: 500 }
      );
    }

    const response = await fetch("https://api.anthropic.com/v1/messages", {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        "x-api-key": apiKey,
        "anthropic-version": "2023-06-01",
      },
      body: JSON.stringify({
        model: "claude-sonnet-4-6",
        max_tokens: 1000,
        system,
        messages,
      }),
    });

    if (!response.ok) {
      const errText = await response.text();
      return Response.json({ error: errText }, { status: response.status });
    }

    const data = await response.json();
    return Response.json(data);
  } catch (err) {
    return Response.json({ error: String(err) }, { status: 500 });
  }
}{
  "name": "Adul AI",
  "short_name": "Adul AI",
  "description": "Tumhara apna AI saathi - padhai, baat-cheet, aur creative ideas",
  "start_url": "/",
  "display": "standalone",
  "background_color": "#171717",
  "theme_color": "#171717",
  "icons": [
    { "src": "/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
