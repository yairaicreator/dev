# WebEdit AI - Supabase Authentication Integration

## ✅ Implementation Complete

This document describes the complete Supabase authentication integration between the WebEdit AI Chrome extension and the website (https://www.webeditai.com).

---

## 🎯 Overview

The extension and website now share authentication state through Supabase. When a user signs in:
1. The extension opens the website's sign-in page
2. User completes Google OAuth via Supabase
3. The session is automatically sent to the extension
4. Both the website and extension share the same authenticated user

---

## 📁 Files Modified/Created

### Extension Files (`thetool---webeditai-1/`)

#### 1. **manifest.json** (Modified)
- Added `storage` and `tabs` permissions
- Added content script for `webeditai.com` domain (production and development)

#### 2. **bridge-listener.js** (New)
- Content script that runs on webeditai.com
- Listens for `WEBEDIT_AUTH` postMessage from the website
- Forwards Supabase session to background script

#### 3. **background.js** (Modified)
- Added auth message handlers:
  - `WEBEDIT_STORE_SESSION` - Stores session from bridge
  - `WEBEDIT_GET_SESSION` - Returns stored session
  - `WEBEDIT_OPEN_LOGIN` - Opens login page
  - `WEBEDIT_SIGN_OUT` - Clears session
  - `WEBEDIT_OPEN_HISTORY` - Opens history page
- Auto-detects dev/prod mode for correct URLs

#### 4. **contentScript.js** (Modified)
- Added authentication functions:
  - `checkAuthStatus()` - Checks current auth state
  - `updateAuthUI()` - Updates button based on auth state
  - `handleSignInClick()` - Opens login or signs out
  - `showNotification()` - Shows notifications in panel
- Sign in button now functional
- History button opens history page on website
- Listens for `WEBEDIT_SESSION_UPDATED` messages

#### 5. **panel.css** (Modified)
- Added styles for notification system
- Added signed-in state styling for buttons

### Website Files (`Website (edit -ai) - 10,15,2025/`)

#### 6. **src/pages/History.tsx** (Modified)
- Detects `from=extension` query parameter
- Posts session to extension via postMessage
- Shows success banner when auth completes from extension

#### 7. **src/pages/SignUp.tsx** (Modified)
- Detects `from=extension` query parameter
- Shows "Connecting to Extension" badge
- Redirects to `/auth/start` for extension flow
- Auto-checks "Connect to extension" checkbox

#### 8. **src/pages/auth/Bridge.tsx** (Modified)
- Already posts session to extension
- Now checks sessionStorage for extension flag
- Redirects to `/history?from=extension` after auth

---

## 🔄 Authentication Flow

### Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                     User clicks "Sign in" in Extension              │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  Extension opens: https://www.webeditai.com/#/signup?from=extension│
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  User clicks "Sign up with Google" → Redirects to /auth/start      │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  /auth/start initiates Google OAuth via Supabase                   │
│  redirectTo: https://www.webeditai.com/#/auth/bridge               │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  User authenticates with Google                                      │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  Redirected back to: https://www.webeditai.com/#/auth/bridge       │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  /auth/bridge:                                                      │
│  1. Gets Supabase session                                           │
│  2. Posts session via window.postMessage                            │
│  3. Extension content script captures it                            │
│  4. Redirects to /history?from=extension                            │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  /history page:                                                     │
│  1. Detects from=extension flag                                     │
│  2. Posts session again (redundant safety)                          │
│  3. Shows success banner                                            │
└────────────────────────────────┬────────────────────────────────────┘
                                 │
                                 v
┌─────────────────────────────────────────────────────────────────────┐
│  Extension stores session in chrome.storage.local                   │
│  Broadcasts WEBEDIT_SESSION_UPDATED to all tabs                     │
│  Panel shows: "Welcome back, user@email.com!"                       │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Setup Instructions

### 1. Supabase Configuration

Go to your Supabase Dashboard → Authentication → URL Configuration

Add these **Redirect URLs**:

```
https://www.webeditai.com/#/auth/bridge
http://127.0.0.1:8080/#/auth/bridge
```

**Important:** Make sure the URLs include the `#` symbol (for HashRouter).

### 2. Website Environment Variables

Create `.env.local` in the website directory (`Website (edit -ai) - 10,15,2025/`):

```bash
# Supabase Configuration
VITE_SUPABASE_URL=https://eqfjkvjwsswjxkmomxax.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImVxZmprdmp3c3N3anhrbW9teGF4Iiwicm9sZSI6ImFub24iLCJpYXQiOjE3NTYxMTU1MDYsImV4cCI6MjA3MTY5MTUwNn0.sh5d5Hj5hshIOndyAodK_rlP0K1pERYyWyNqNxp-E7k

# Website URL (used for OAuth redirects)
VITE_SITE_URL=https://www.webeditai.com
```

### 3. Start the Website (Development)

```bash
cd "Website (edit -ai) - 10,15,2025"
npm install  # if needed
npm run dev
```

The website should start on `http://127.0.0.1:8080`

### 4. Load the Extension

1. Open Chrome/Edge
2. Go to `chrome://extensions`
3. Enable "Developer mode"
4. Click "Load unpacked"
5. Select the `thetool---webeditai-1` folder
6. Note the extension ID (you'll see it under the extension name)

### 5. Reload Extension After Code Changes

Whenever you make changes to the extension files:
1. Go to `chrome://extensions`
2. Find "WebEdit AI"
3. Click the reload button (🔄)

---

## 🧪 Testing the Authentication Flow

### Step 1: Open the Extension Panel

1. Navigate to any website (e.g., `https://example.com`)
2. Click the WebEdit AI extension icon in your toolbar
3. The panel should open on the right side
4. You should see a "Sign in" button in the header

### Step 2: Sign In

1. Click the "Sign in" button in the panel header
2. A new tab should open to `http://127.0.0.1:8080/#/signup?from=extension`
3. You'll see a blue banner: "Connecting to WebEdit AI Extension"
4. Check the "Agree to Terms & Privacy" checkbox
5. Click "Sign up with Google"

### Step 3: Complete OAuth

1. The page will redirect to Google OAuth
2. Select your Google account
3. Grant permissions
4. You'll be redirected back to the auth/bridge page
5. Then automatically redirected to the history page
6. You'll see a green success banner

### Step 4: Verify Extension

1. Go back to the tab where you opened the extension panel
2. The panel should show your email instead of "Sign in"
3. You'll see a notification: "Welcome back, user@email.com!"

### Step 5: Test Persistence

1. Close the extension panel
2. Reopen it by clicking the extension icon
3. Should still show your email (session persisted)
4. Close the browser completely and reopen
5. Open the panel again - should still be signed in

### Step 6: Test History Button

1. With the panel open, click the "History" button in the header
2. A new tab should open to the history page
3. You should see your edit history (currently empty)

### Step 7: Test Sign Out

1. Open the extension panel
2. Click on your email (now acts as a sign out button)
3. Confirm sign out in the dialog
4. The panel should show "Sign in" again
5. A new tab opens asking if you want to sign out on the website too

---

## 🐛 Troubleshooting

### Issue: "Sign in" button doesn't work

**Solution:**
1. Reload the extension at `chrome://extensions`
2. Close and reopen the panel
3. Check browser console for errors (F12)

### Issue: Auth page shows "redirect_uri_mismatch"

**Solution:**
1. Go to Supabase Dashboard
2. Add redirect URLs with the `#` symbol:
   - `http://127.0.0.1:8080/#/auth/bridge`
   - `https://www.webeditai.com/#/auth/bridge`

### Issue: Session not received by extension

**Check these logs:**

1. **Bridge Page Console** (F12 on auth/bridge page):
   ```
   🔐 WebEdit AI: Bridge listener initialized
   ✅ Received auth session from bridge page
   ✅ Session forwarded to background: {ok: true}
   ```

2. **Extension Background Console** (`chrome://extensions` → Inspect views: service worker):
   ```
   💾 Storing Supabase session from website
   ✅ Session stored successfully
   ```

3. **Extension Panel Console** (Right-click panel → Inspect):
   ```
   🔐 Auth status: Signed in as user@email.com
   ```

### Issue: Panel doesn't update after sign in

**Solution:**
1. Close the auth tab manually
2. Close and reopen the extension panel
3. The panel should now show your email

### Issue: Website not running

**Solution:**
```bash
cd "Website (edit -ai) - 10,15,2025"
npm run dev
```

Make sure it starts on `http://127.0.0.1:8080`

---

## 🔒 Security Features

1. **Origin Verification**
   - Content script only accepts messages from `webeditai.com` or `127.0.0.1:8080`
   - Validates message type before processing

2. **Message Type Validation**
   - Only processes messages with type `WEBEDIT_AUTH`
   - Ignores all other postMessage events

3. **Secure Storage**
   - Session stored in `chrome.storage.local`
   - Not accessible to web pages
   - Only accessible to extension components

4. **Session Isolation**
   - Extension and website share session but each validates independently
   - No direct communication between extension and website APIs

---

## 📊 Session Storage Format

The extension stores the session in `chrome.storage.local` with this structure:

```javascript
{
  webedit_supabase_session: {
    access_token: "...",
    refresh_token: "...",
    expires_in: 3600,
    expires_at: 1234567890,
    token_type: "bearer",
    user: {
      id: "...",
      email: "user@email.com",
      // ... other user fields
    }
  },
  webedit_session_timestamp: 1234567890000
}
```

---

## 🔧 Development vs Production

The extension automatically detects dev/prod mode:

**Development Mode** (extension loaded unpacked):
- Website URL: `http://127.0.0.1:8080`
- Content script runs on `http://127.0.0.1:8080/*`

**Production Mode** (extension installed from store):
- Website URL: `https://www.webeditai.com`
- Content script runs on `https://www.webeditai.com/*`

---

## 🎨 UI Components

### Sign In Button States

**Not signed in:**
```
┌─────────────┐
│  Sign in    │  ← White text, pink background
└─────────────┘
```

**Signed in:**
```
┌─────────────────┐
│  user@email.com │  ← Green background, truncated email
└─────────────────┘
```

### Notifications

```
┌─────────────────────────────────────┐
│ ℹ Opening sign-in page...          │  Blue (info)
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ ✓ Welcome back, user@email.com!    │  Green (success)
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│ ⚠ Failed to sign in                │  Red (error)
└─────────────────────────────────────┘
```

---

## 📝 Message Types

### Extension → Background

| Type | Purpose | Response |
|------|---------|----------|
| `WEBEDIT_GET_SESSION` | Get stored session | `{ session, timestamp }` |
| `WEBEDIT_OPEN_LOGIN` | Open login page | `{ ok: true, tabId }` |
| `WEBEDIT_SIGN_OUT` | Clear session | `{ ok: true }` |
| `WEBEDIT_OPEN_HISTORY` | Open history page | `{ ok: true, tabId }` |
| `WEBEDIT_STORE_SESSION` | Store session (from bridge) | `{ ok: true }` |

### Background → Content Script

| Type | Purpose | Data |
|------|---------|------|
| `WEBEDIT_SESSION_UPDATED` | Session changed | `{ session }` or `{ session: null }` |
| `WEBEDIT_TOGGLE_PANEL` | Toggle panel | None |

### Website → Extension (via postMessage)

| Type | Purpose | Data |
|------|---------|------|
| `WEBEDIT_AUTH` | Send session to extension | `{ session }` |

---

## 🚀 Production Deployment

### Website Deployment

1. Build the website:
   ```bash
   cd "Website (edit -ai) - 10,15,2025"
   npm run build
   ```

2. Deploy the `dist/` folder to your hosting

3. Update Supabase redirect URLs to include production URL:
   ```
   https://www.webeditai.com/#/auth/bridge
   ```

### Extension Deployment

1. Update `manifest.json` with production `update_url` (if using Chrome Web Store)

2. The extension will automatically use production URLs when `update_url` is present

3. Test thoroughly in production before submitting to Chrome Web Store

---

## 🔄 Future Enhancements

1. **Automatic Session Refresh**
   - Implement token refresh before expiry
   - Handle expired sessions gracefully

2. **Multiple Auth Providers**
   - Support GitHub, Microsoft, etc.
   - Provider selection in extension

3. **Edit History Sync**
   - Store edits in Supabase
   - Real-time sync between devices

4. **User Profile in Panel**
   - Show avatar, name, email
   - Quick access to settings

5. **Session Encryption**
   - Encrypt session data in storage
   - Add additional security layer

---

## 📚 Resources

- [Supabase Auth Documentation](https://supabase.com/docs/guides/auth)
- [Chrome Extension Storage API](https://developer.chrome.com/docs/extensions/reference/storage/)
- [Chrome Extension Messaging](https://developer.chrome.com/docs/extensions/mv3/messaging/)

---

## ✅ Checklist for Success

- [ ] Supabase redirect URLs configured with `#` symbol
- [ ] Website `.env.local` file created with credentials
- [ ] Website running on `http://127.0.0.1:8080`
- [ ] Extension loaded and reloaded
- [ ] Can see "Sign in" button in extension panel
- [ ] Sign in opens correct URL with extension flag
- [ ] OAuth completes successfully
- [ ] Bridge page posts session to extension
- [ ] History page shows success banner
- [ ] Extension panel shows user email
- [ ] Session persists across panel close/reopen
- [ ] Session persists across browser restart
- [ ] History button opens history page
- [ ] Sign out works correctly

---

**Implementation Date:** November 21, 2025  
**Status:** ✅ Complete and Ready for Testing

