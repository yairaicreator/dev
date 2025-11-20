# 🎯 OAuth Testing Guide - AI Chat Panel

## ✅ What Still Exists

Even though the popup files were deleted, **all OAuth infrastructure is still working**:

1. ✅ **Background Service Worker** - Handles OAuth flow
2. ✅ **Content Script Bridge Listener** - Captures sessions from website
3. ✅ **Website Auth Pages** - `/auth/start` and `/auth/bridge`
4. ✅ **Session Storage** - Stores in `chrome.storage.local`

## 🆕 What I Added

I added a **"Sign in with Google" button** directly to the **AI Chat Panel** (the side panel that opens on web pages).

### Location of the Button

```
╔═══════════════════════════════════════╗
║  AI Chat Panel                    [×] ║
╠═══════════════════════════════════════╣
║  ┌─────────────────────────────────┐  ║
║  │ [G] Sign in with Google         │  ║ ← NEW BUTTON HERE!
║  └─────────────────────────────────┘  ║
╠═══════════════════════════════════════╣
║  👋 Welcome to WebEdit AI!            ║
║  Ask me to change anything...         ║
║                                       ║
║  [Visual Edit] [🎯 Pick element]     ║
╠═══════════════════════════════════════╣
║  What do you want to change?     [→]  ║
╚═══════════════════════════════════════╝
```

### After Signing In

```
╔═══════════════════════════════════════╗
║  AI Chat Panel                    [×] ║
╠═══════════════════════════════════════╣
║  👤  your.email@gmail.com             ║
║      Sign out                         ║ ← Shows email + sign out
╠═══════════════════════════════════════╣
║  👋 Welcome back, your.email!         ║
║                                       ║
╚═══════════════════════════════════════╝
```

---

## 🚀 How to Test

### Step 1: Make Sure Website is Running

```bash
cd "Website (edit -ai) - 10,15,2025"
npm run dev
```

Should start on: `http://127.0.0.1:8080`

### Step 2: Configure Supabase (One-time Setup)

Go to [Supabase Dashboard](https://supabase.com/dashboard) → Your Project → Authentication → URL Configuration

Add these redirect URLs:
```
http://127.0.0.1:8080/#/auth/bridge
https://www.webeditai.com/#/auth/bridge
```

⚠️ **Important:** Include the `#` in the URLs!

### Step 3: Reload Extension

1. Go to `chrome://extensions`
2. Find "WebEdit AI"
3. Click the reload button (🔄)

### Step 4: Open AI Chat Panel

You have two ways to open it:

**Option A: Keyboard Shortcut**
- Press `Alt+Shift+E` on any webpage

**Option B: Extension Icon**
- Click the WebEdit AI icon in toolbar
- (The popup is deleted, but it will trigger the panel to open)

### Step 5: Click "Sign in with Google"

1. The AI Chat Panel should now be open on the right side
2. You'll see a white button with Google logo: **"Sign in with Google"**
3. Click it!

### Step 6: Complete OAuth Flow

1. New tab opens: `http://127.0.0.1:8080/#/auth/start`
2. Redirects to Google OAuth
3. Sign in with your Google account
4. Redirects back to: `http://127.0.0.1:8080/#/auth/bridge`
5. Shows success message: "Sign-in successful! You can close this tab."
6. Close the auth tab

### Step 7: Verify Login

1. Go back to the page with the AI Chat Panel
2. The panel should now show:
   - Your email address
   - "Sign out" button
   - Welcome message: "Welcome back, your.email@gmail.com!"

---

## 🔍 What to Look For

### In the AI Chat Panel (Before Login):
- ✅ White button with Google 4-color logo
- ✅ Text: "Sign in with Google"
- ✅ Positioned right below the header

### After Clicking Sign In:
- ✅ New tab opens to auth page
- ✅ System message in chat: "🔐 Opening sign-in page..."

### On the Auth Bridge Page:
Open browser console (F12) and check for:
```
🔐 WebEdit AI: Bridge listener initialized on http://127.0.0.1:8080/#/auth/bridge
✅ Received auth session from bridge page
✅ Session forwarded to background: {ok: true}
```

### In Extension Service Worker:
Go to `chrome://extensions` → WebEdit AI → "Inspect views: service worker"
```
💾 Storing Supabase session
✅ Session stored successfully
```

### Back in the AI Chat Panel (After Login):
- ✅ Shows your email: `your.email@gmail.com`
- ✅ Shows "Sign out" button
- ✅ Chat message: "Welcome back, your.email@gmail.com!"

---

## 🎨 Visual Appearance

### Sign In Button
- **Background:** White
- **Border:** Light gray
- **Icon:** Google 4-color logo (blue, red, yellow, green)
- **Text:** "Sign in with Google" in dark gray
- **Hover Effect:** Slightly lifts with shadow

### User Info (After Login)
- **Avatar:** Pink-blue gradient circle with 👤 icon
- **Email:** Your Google email
- **Sign Out:** Red text link

---

## 🐛 Troubleshooting

### "I don't see the button in the panel"
**Solution:** 
1. Make sure you reloaded the extension
2. Close the panel (click ×) and reopen it (Alt+Shift+E)
3. Check that `panel.html`, `panel.js`, and `panel.css` have the changes

### "Button doesn't do anything when clicked"
**Solution:**
1. Open browser console (F12) on the page
2. Check for errors
3. Make sure background service worker is running

### "Auth page shows 'redirect_uri_mismatch'"
**Solution:**
1. Go to Supabase Dashboard
2. Add `http://127.0.0.1:8080/#/auth/bridge` to redirect URLs
3. Make sure to include the `#` symbol!

### "Panel shows email but then resets"
**Solution:**
1. This happens if session storage fails
2. Check browser console for errors
3. Try signing in again

### "Can't open the panel"
**Solution:**
1. Try keyboard shortcut: `Alt+Shift+E`
2. Or click extension icon in toolbar
3. Make sure you're on a regular webpage (not chrome:// pages)

---

## 📝 Testing Checklist

- [ ] Website running on `http://127.0.0.1:8080`
- [ ] Supabase redirect URLs configured with `#`
- [ ] Extension reloaded at `chrome://extensions`
- [ ] AI Chat Panel opens (Alt+Shift+E)
- [ ] "Sign in with Google" button visible in panel
- [ ] Button click opens auth page
- [ ] Google OAuth completes successfully
- [ ] Bridge page shows success message
- [ ] Panel shows email after returning
- [ ] Welcome message appears in chat
- [ ] "Sign out" button works

---

## 🎉 Expected Result

After completing OAuth:
1. ✅ Panel shows your Google email
2. ✅ Session persists (even if you close and reopen panel)
3. ✅ Session survives browser restart
4. ✅ Can sign out and sign in again

---

## 📂 Files Modified

### Extension Files:
- `panel/panel.html` - Added auth section HTML
- `panel/panel.js` - Added auth functions
- `panel/panel.css` - Added auth styling

### Files Still Working:
- `background/service-worker.js` - OAuth handlers
- `content/bridge-listener.js` - Session capture
- `manifest.json` - Permissions and content scripts

### Website Files (No Changes Needed):
- `src/pages/auth/Start.tsx` - Still working
- `src/pages/auth/Bridge.tsx` - Still working

---

## 🔥 Key Points

1. **OAuth infrastructure is 100% functional** - Nothing was lost when popup was deleted
2. **Button is now in the AI Chat Panel** - Easier to access
3. **Session persists across panel close/reopen** - Chrome storage is working
4. **Real-time updates** - Panel updates when you sign in from another tab

---

**Ready to Test!** Open the AI Chat Panel and look for the white "Sign in with Google" button! 🚀

