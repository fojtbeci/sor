# Deploy & Setup Guide

This document explains step-by-step how to deploy the Beer Inventory App to **Netlify** (frontend) and **Render** (backend).

---

## 1. Local Development

### Backend
```bash
cd backend
npm install
npm run dev
```

### Frontend
```bash
cd frontend
npm install
npm run dev
```

Visit: [http://localhost:5173](http://localhost:5173)

---

## 2. GitHub Setup
```bash
git init
git add .
git commit -m "Initial commit"
gh repo create my-beer-inventory --public --source=. --remote=origin --push
```

---

## 3. Backend Deploy (Render)

1. Create a **Web Service** on Render.  
   - Root directory: `backend`  
   - Build command: `npm install`  
   - Start command: `npm start`  

2. Make sure `app.js` uses:
   ```js
   const PORT = process.env.PORT || 4000;
   app.listen(PORT, () => console.log("Backend running"));
   ```

3. Get your backend URL, e.g. `https://your-backend.onrender.com`

---

## 4. Frontend Deploy (Netlify)

1. Create a new **Netlify site** → Import from GitHub → Select repo.  
2. Build settings:  
   - Build command: `npm run build`  
   - Publish directory: `dist`  
3. Add environment variables:  
   - `VITE_API_URL = https://your-backend.onrender.com/api`  
   - `VITE_ACCESS_TOKEN = <generated token>`  
4. SPA redirect: already included via `frontend/public/_redirects`.

---

## 5. Token Usage

Generate a token:
```bash
curl -X POST https://your-backend.onrender.com/api/generate-token
```

Open your site with:
```
https://your-frontend.netlify.app/?token=THE_TOKEN
```

---

## 6. Notes

- SQLite is **not persistent** on Render. Use Render **Persistent Disk** or **Managed Postgres** for production.  
- `VITE_*` variables are embedded in the frontend bundle. Do not put sensitive secrets there.  
