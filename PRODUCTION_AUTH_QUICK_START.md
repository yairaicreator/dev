# 🚀 Production Auth - Quick Start

## ⚡ 3-Step Setup

### Step 1: Configure Supabase (30 seconds)
Go to [Supabase Dashboard](https://supabase.com/dashboard) → Your Project → Authentication → URL Configuration

Add this redirect URL:
```
https://www.webeditai.com/#/auth/bridge
```

### Step 2: Deploy Website (if not already)
Your website at `https://www.webeditai.com` must be live with the latest code.

Verify it's working:
- Open `https://www.webeditai.com`
- Should see the homepage
- Try navigating to `https://www.webeditai.com/#/signup`
- Should see signup page

### Step 3: Reload Extension
1. Go to `chrome://extensions`
2. Find "WebEdit AI"
3. Click reload button (🔄)

---

## ✅ Test It Now (2 minutes)

### Test the Sign In Flow

1. **Open extension on any page**
   - Click WebEdit AI icon in toolbar
   - Panel opens on right side

2. **Click "Sign in"**
   - Should see white "Sign in" button in header
   - Click it

3. **Should open:**
   ```
   https://www.webeditai.com/#/signup?from=extension=1
   ```
   - You'll see blue banner: "🔌 Connecting to WebEdit AI Extension"
   - NOT `http://127.0.0.1:8080` (dev URL is gone!)

4. **Complete OAuth**
   - Check "Agree to Terms & Privacy"
   - Click "Sign up with Google"
   - Sign in with Google
   - Redirected to history page

5. **Go back to extension**
   - Close auth tab
   - Open extension panel again
   - **Should see your avatar!** (photo or first letter of email)

6. **Click avatar**
   - Dropdown menu appears
   - Try "View History" → opens production history page
   - Try "Sign Out" → signs you out

---

## 🎯 What Changed from Before

### ❌ OLD (Dev URLs)
- Extension checked if dev or prod mode
- Opened `http://127.0.0.1:8080` when unpacked
- Required local dev server running
- Showed directory listing if server wasn't running

### ✅ NEW (Production Only)
- **Always uses `https://www.webeditai.com`**
- No dev URL detection
- No local server needed
- Works immediately after loading extension

---

## 🔄 The Auth Flow Now

```
Extension                           Website
────────                           ────────
[Sign in] ──────────────────────> Opens: webeditai.com/#/signup?from=extension=1
                                  User completes Google OAuth
                                  Redirected to: webeditai.com/#/history
                                  Posts session via postMessage
[Avatar] <──────────────────────  Session captured by bridge-listener
Dropdown menu ───────────────────> Click "View History" opens website
[Sign out] ──────────────────────> Opens logout URL, website signs out
```

---

## 🎨 UI Components

### Not Signed In
```
┌─────────────────────────────┐
│ (Logo) [History] [Sign in] │  ← Click "Sign in"
└─────────────────────────────┘
```

### Signed In
```
┌─────────────────────────────┐
│ (Logo) [History] [👤 Avatar]│  ← Click avatar
│                  ┌─────────┐│
│                  │ 📚 History│
│                  │ 👋 Sign out│
│                  └─────────┘│
└─────────────────────────────┘
```

---

## ⚠️ Important Notes

### 1. No More Dev Server
- **You can't test auth with `http://127.0.0.1:8080`**
- Extension always uses production URL
- Website must be deployed and live

### 2. Chrome Storage
- Sessions stored in `chrome.storage.local`
- Persists across browser restarts
- Auto-cleared if expired

### 3. Bidirectional Sync
- Sign in/out on website → Extension updates
- Sign in/out on extension → Website updates
- Uses `postMessage` for communication

---

## 🐛 Common Issues

### "Sign in opens dev server / directory listing"
**This should NOT happen anymore!**  
If you see this:
1. Make sure you reloaded the extension
2. Check you accepted all the code changes
3. Verify `background.js` has production URLs only

### "Avatar doesn't appear after sign in"
1. Close the auth tab manually
2. Go back to original tab
3. Close extension panel
4. Reopen panel
5. Should show avatar now

### "Can't sign in - redirect error"
1. Check Supabase redirect URLs
2. Make sure you added: `https://www.webeditai.com/#/auth/bridge`
3. Include the `#` symbol!

---

## 📋 Quick Checklist

- [ ] Supabase redirect URL configured
- [ ] Website live at `https://www.webeditai.com`
- [ ] Extension reloaded at `chrome://extensions`
- [ ] Can see "Sign in" button in panel
- [ ] Click "Sign in" opens production URL (not 127.0.0.1)
- [ ] Google OAuth completes
- [ ] History page shows green success banner
- [ ] Extension panel shows avatar
- [ ] Can click avatar to see menu
- [ ] "View History" opens production page
- [ ] "Sign Out" works and shows success message

---

## 🎉 Success Criteria

**You'll know it's working when:**

1. ✅ Click "Sign in" opens `https://www.webeditai.com/#/signup?from=extension=1`
2. ✅ After OAuth, see avatar (photo or letter) instead of "Sign in"
3. ✅ Avatar menu has View History and Sign Out options
4. ✅ Session persists across browser restarts
5. ✅ Sign out works from both extension and website

**No more dev URLs anywhere!** 🎊

---

**Need help?** Check `AUTH_IMPLEMENTATION_COMPLETE.md` for detailed documentation.

