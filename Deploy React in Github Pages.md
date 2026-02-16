# 🚀 Deploying a Vite + React App to GitHub Pages

This guide explains how to deploy a React project (created using Vite) to GitHub Pages.

---

## ✅ Step 1: Install gh-pages

Run this inside your project folder:

```bash
npm install gh-pages --save-dev
```

---

## ✅ Step 2: Update `vite.config.js`

Because GitHub Pages serves your project from:

```
https://yourusername.github.io/your-repo-name/
```

You must set the `base` property.

Edit `vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/your-repo-name/'   // ⚠️ Replace with your repository name
})
```

Example:

```js
base: '/ToDoReact/'
```

---

## ✅ Step 3: Update `package.json`

Add the `homepage` property:

```json
"homepage": "https://yourusername.github.io/your-repo-name",
```

Example:

```json
"homepage": "https://aiswaryalb.github.io/ToDoReact",
```

Then update the `scripts` section:

```json
"scripts": {
  "dev": "vite",
  "build": "vite build",
  "preview": "vite preview",
  "predeploy": "npm run build",
  "deploy": "gh-pages -d dist"
}
```

---

## ✅ Step 4: Push Changes to GitHub

```bash
git add .
git commit -m "Configured project for GitHub Pages"
git push origin main
```

---

## ✅ Step 5: Deploy to GitHub Pages

Run:

```bash
npm run deploy
```

This will:
- Build the project
- Create a `gh-pages` branch
- Upload the production files

---

## ✅ Step 6: Enable GitHub Pages

1. Go to your GitHub repository
2. Click **Settings**
3. Click **Pages**
4. Under **Source**, select:
   - Branch: `gh-pages`
   - Folder: `/ (root)`
5. Click **Save**

---

## 🌍 Your App Will Be Live At

```
https://yourusername.github.io/your-repo-name/
```

Example:

```
https://aiswaryalb.github.io/ToDoReact/
```

---

## 🔄 Updating the Website After Changes

Whenever you update your code:

```bash
git add .
git commit -m "update"
git push
npm run deploy
```

---

## ⚠️ Important Notes

- Always keep `base: '/your-repo-name/'` in `vite.config.js`
- If your page loads blank, check the `base` path
- You must run `npm run deploy` every time you want to update the live site

---

🎉 Done! Your React app is now live on GitHub Pages.
