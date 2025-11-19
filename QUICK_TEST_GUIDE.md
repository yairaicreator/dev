# Quick Test Guide - OAuth Sign In

## 🎯 Where's the Button?

Open your extension popup and you'll see a **big white button** with the Google logo that says **"Sign in with Google"**. You can't miss it - it's right below the account info section!

## ⚡ Quick Start (3 Steps)

### Step 1: Configure Supabase (One-time setup)
Go to [Supabase Dashboard](https://supabase.com/dashboard/project/eqfjkvjwsswjxkmomxax/auth/url-configuration) and add these redirect URLs:

```
http://127.0.0.1:8080/#/auth/bridge
https://www.webeditai.com/#/auth/bridge
```

**Important:** Make sure to include the `#` in the URLs!

### Step 2: Start Your Website
```bash
cd "Website (edit -ai) - 10,15,2025"
npm run dev
```

Should start on `http://127.0.0.1:8080`

### Step 3: Reload Extension & Test
1. Go to `chrome://extensions`
2. Find "WebEdit AI"
3. Click the reload button (🔄)
4. Click the extension icon in your toolbar
5. Click the **"Sign in with Google"** button (white button with Google logo)
6. Complete Google OAuth
7. Extension should show your email!

---

## 🎨 Visual Guide

### Before Sign In:
```
╔═══════════════════════════════╗
║        WebEdit AI             ║
╠═══════════════════════════════╣
║  👤  Not signed in            ║
║      Click to sign in         ║
╠═══════════════════════════════╣
║  ┌─────────────────────────┐  ║
║  │ [G] Sign in with Google │  ║ ← CLICK THIS!
║  └─────────────────────────┘  ║
╠═══════════════════════════════╣
║  💬 AI Chat                   ║
║  📝 Visual Edit               ║
║  📚 History                   ║
╚═══════════════════════════════╝
```

### After Sign In:
```
╔═══════════════════════════════╗
║        WebEdit AI             ║
╠═══════════════════════════════╣
║  ✓  your.email@gmail.com      ║
║      Your Name                ║
╠═══════════════════════════════╣
║  ┌─────────────────────────┐  ║
║  │      Sign Out           │  ║ ← Click to log out
║  └─────────────────────────┘  ║
╠═══════════════════════════════╣
║  💬 AI Chat                   ║
║  📝 Visual Edit               ║
║  📚 History                   ║
╚═══════════════════════════════╝
```

---

## 🔍 What to Look For

### In the Browser Tab (after clicking button):
1. Opens: `http://127.0.0.1:8080/#/auth/start`
2. Redirects to Google OAuth
3. After login: `http://127.0.0.1:8080/#/auth/bridge`
4. Shows: "Sign-in successful! You can close this tab now."

### In the Console (F12 on the bridge page):
```
🔐 WebEdit AI: Bridge listener initialized on http://127.0.0.1:8080/#/auth/bridge
✅ Received auth session from bridge page
✅ Session forwarded to background: {ok: true}
```

### In Extension Service Worker Console:
(Go to `chrome://extensions` → WebEdit AI → "Inspect views: service worker")
```
💾 Storing Supabase session
✅ Session stored successfully
```

### In Extension Popup (after reopening):
Should show your email address instead of "Not signed in"

---

## 🐛 Troubleshooting

### "I don't see the button!"
- **Solution:** Reload the extension at `chrome://extensions`
- Make sure you accepted all the changes to the popup files

### "Button opens a blank page"
- **Solution:** Make sure your website is running on `http://127.0.0.1:8080`
- Check that the dev server is actually running (`npm run dev`)

### "OAuth completes but extension doesn't show my email"
- **Solution:** 
  1. Check Supabase redirect URLs include the `#` 
  2. Look at browser console on the bridge page for errors
  3. Close and reopen the extension popup

### "Google says 'redirect_uri_mismatch'"
- **Solution:** You need to add `http://127.0.0.1:8080/#/auth/bridge` to Supabase redirect URLs
- Go to Supabase Dashboard → Authentication → URL Configuration

---

## ✅ Success Checklist

- [ ] Website running on `http://127.0.0.1:8080`
- [ ] Extension loaded and reloaded
- [ ] Supabase redirect URLs configured with `#`
- [ ] Can see "Sign in with Google" button in popup
- [ ] Button opens auth page
- [ ] Google OAuth completes
- [ ] Bridge page shows success message
- [ ] Extension popup shows email after reopening

---

## 📸 Screenshot Locations

The button styling:
- **Background:** White (`#ffffff`)
- **Border:** Light gray shadow
- **Google Logo:** 4-color Google icon (blue, red, yellow, green)
- **Text:** "Sign in with Google" in gray
- **Hover:** Slightly lifts up with shadow

It looks exactly like the official Google Sign-in button!

---

## 🎉 What's Working

You can now:
- ✅ Click "Sign in with Google" button in extension
- ✅ Complete Google OAuth via Supabase
- ✅ Session automatically stored in extension
- ✅ Popup shows your email when logged in
- ✅ Click "Sign Out" to log out
- ✅ Session persists across browser restarts

---

**Need Help?**
Check the console logs in:
1. Bridge page (F12 on `/#/auth/bridge`)
2. Extension service worker (`chrome://extensions` → Inspect views)
3. Extension popup (right-click popup → Inspect)

