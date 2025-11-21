# 🚀 Quick Start - Test Authentication in 5 Minutes

## Prerequisites

✅ Chrome or Edge browser  
✅ Node.js installed  
✅ Git (to have cloned this repo)

---

## Step 1: Configure Supabase (30 seconds)

1. Go to [Supabase Dashboard](https://supabase.com/dashboard)
2. Navigate to your project → **Authentication** → **URL Configuration**
3. Add these two redirect URLs:

```
http://127.0.0.1:8080/#/auth/bridge
https://www.webeditai.com/#/auth/bridge
```

**⚠️ Important:** Include the `#` symbol!

---

## Step 2: Create Website `.env.local` (30 seconds)

Create a file: `Website (edit -ai) - 10,15,2025/.env.local`

```bash
VITE_SUPABASE_URL=https://eqfjkvjwsswjxkmomxax.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVxZmprdmp3c3N3anhrbW9teGF4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTYxMTU1MDYsImV4cCI6MjA3MTY5MTUwNn0.sh5d5Hj5hshIOndyAodK_rlP0K1pERYyWyNqNxp-E7k
VITE_SITE_URL=https://www.webeditai.com
```

---

## Step 3: Start the Website (1 minute)

```bash
cd "Website (edit -ai) - 10,15,2025"
npm install    # Only needed first time
npm run dev
```

✅ Website should be running on: `http://127.0.0.1:8080`

---

## Step 4: Load the Extension (1 minute)

1. Open Chrome/Edge
2. Go to: `chrome://extensions`
3. Toggle **"Developer mode"** ON (top right)
4. Click **"Load unpacked"**
5. Select folder: `thetool---webeditai-1`
6. You should see "WebEdit AI" extension loaded

---

## Step 5: Test Sign In (2 minutes)

### 5a. Open Extension Panel
1. Navigate to any website (e.g., `https://example.com`)
2. Click the **WebEdit AI** extension icon in your browser toolbar
3. Panel opens on the right side
4. Look for **"Sign in"** button in the header

### 5b. Click Sign In
1. Click the **"Sign in"** button
2. New tab opens: `http://127.0.0.1:8080/#/signup?from=extension`
3. You'll see: 🔌 **"Connecting to WebEdit AI Extension"**

### 5c. Complete Google OAuth
1. Check **"Agree to Terms & Privacy"**
2. Click **"Sign up with Google"**
3. Select your Google account
4. Grant permissions

### 5d. Success!
1. Redirected to bridge page (briefly)
2. Then to history page with green banner: ✓ **"Successfully signed in!"**
3. Go back to the original tab
4. Panel should show your email: `your.email@gmail.com`
5. Notification: **"Welcome back, your.email@gmail.com!"**

---

## ✅ Verification Checklist

Check each one:

- [ ] Website running on `http://127.0.0.1:8080`
- [ ] Extension loaded in Chrome
- [ ] Can open extension panel on any webpage
- [ ] "Sign in" button visible in panel header
- [ ] Click "Sign in" opens signup page with extension flag
- [ ] Google OAuth completes successfully
- [ ] Panel shows your email after sign in
- [ ] Can close and reopen panel - still signed in
- [ ] "History" button opens history page
- [ ] Can click email to sign out

---

## 🎉 You're Done!

The authentication is now working! Both the extension and website share the same user identity.

### What You Can Do Now:

1. **Test Sign Out:**
   - Click your email in the panel header
   - Confirm sign out
   - Panel returns to "Sign in" state

2. **Test History:**
   - Click "History" button
   - Opens history page on website
   - Shows your edit history (currently empty)

3. **Test Persistence:**
   - Close browser completely
   - Reopen and open panel
   - Should still show your email (session persisted)

---

## 🐛 Common Issues

### Issue: Website won't start
```bash
cd "Website (edit -ai) - 10,15,2025"
rm -rf node_modules package-lock.json
npm install
npm run dev
```

### Issue: Extension not loading
- Make sure you selected the `thetool---webeditai-1` folder
- Check for errors in `chrome://extensions`
- Try clicking "Reload" button on the extension

### Issue: "redirect_uri_mismatch" error
- Go back to Step 1
- Make sure you added BOTH redirect URLs
- Make sure they include the `#` symbol

### Issue: Panel doesn't show email after sign in
- Close the auth tab manually
- Close the extension panel
- Reopen the panel (click extension icon)
- Should now show your email

---

## 📖 Full Documentation

For detailed information, see: **SUPABASE_AUTH_INTEGRATION.md**

---

**Ready to use WebEdit AI with authentication! 🎊**

