# HashRouter OAuth Setup

## Important: Supabase Configuration Required

Since your website uses **HashRouter** instead of BrowserRouter, the OAuth redirect URLs include the hash (`#`) symbol. You need to update your Supabase configuration.

### 1. Update Supabase Redirect URLs

Go to your Supabase Dashboard:
1. Navigate to **Authentication** → **URL Configuration**
2. Add these redirect URLs to the **Redirect URLs** list:

```
http://127.0.0.1:8080/#/auth/bridge
https://www.webeditai.com/#/auth/bridge
```

**Note:** Make sure to include the `#` in the URLs!

### 2. How It Works with HashRouter

With HashRouter, URLs look like:
- Start page: `http://127.0.0.1:8080/#/auth/start`
- Bridge page: `http://127.0.0.1:8080/#/auth/bridge`

The hash part (`#/auth/bridge`) is handled by React Router on the client side.

### 3. Testing

1. **Start your website:**
   ```bash
   cd "Website (edit -ai) - 10,15,2025"
   npm run dev
   ```

2. **Load the extension:**
   - Go to `chrome://extensions`
   - Enable "Developer mode"
   - Click "Reload" on the WebEdit AI extension (or load it if not already loaded)

3. **Test OAuth:**
   - Click the WebEdit AI extension icon
   - You'll see a **big "Sign in with Google" button** with the Google logo
   - Click it
   - Should open: `http://127.0.0.1:8080/#/auth/start`
   - Redirects to Google OAuth
   - After authentication, redirects to: `http://127.0.0.1:8080/#/auth/bridge`
   - Session automatically saves to extension
   - Close the auth tab
   - Open extension popup again - should show your email!

### 4. What Changed for HashRouter

**Extension files updated:**
- `background/service-worker.js` - Now opens `/#/auth/start` instead of `/auth/start`
- `manifest.json` - Content script now matches all pages on your domain
- `content/bridge-listener.js` - Only activates when hash includes `/auth/bridge`

**Website files updated:**
- `src/pages/auth/Start.tsx` - redirectTo now includes the hash: `/#/auth/bridge`

---

## Troubleshooting

### Issue: "No session found" error
- **Cause:** Supabase redirect URLs don't include the hash
- **Solution:** Add `/#/auth/bridge` to Supabase redirect URLs (see step 1 above)

### Issue: Button doesn't appear
- **Solution:** Reload the extension at `chrome://extensions`

### Issue: Console shows "Ignored non-auth message"
- **Cause:** Normal - the content script ignores non-auth messages
- **Solution:** This is expected behavior

### Issue: Extension doesn't store session
- **Solution:** Check the bridge page console logs for errors. Should see:
  - `🔐 WebEdit AI: Bridge listener initialized on...`
  - `✅ Received auth session from bridge page`
  - `✅ Session forwarded to background`

---

## Where's the Button?

When you open the extension popup, you'll see:

```
┌─────────────────────────────┐
│   [Logo]                    │
├─────────────────────────────┤
│  👤  Not signed in          │
│      Click to sign in       │
├─────────────────────────────┤
│  ┌───────────────────────┐  │
│  │  [G]  Sign in with    │  │  ← THIS BUTTON!
│  │       Google          │  │
│  └───────────────────────┘  │
├─────────────────────────────┤
│  💬 AI Chat                 │
│  Visual Edit                │
│  History                    │
└─────────────────────────────┘
```

The button has:
- ✅ Google logo (4 colors)
- ✅ "Sign in with Google" text
- ✅ White background
- ✅ Clear hover effect

---

## After Signing In

Once you've signed in, the popup changes to:

```
┌─────────────────────────────┐
│   [Logo]                    │
├─────────────────────────────┤
│  ✓  your.email@gmail.com    │
│      Your Name              │
├─────────────────────────────┤
│  ┌───────────────────────┐  │
│  │    Sign Out           │  │  ← Click to sign out
│  └───────────────────────┘  │
├─────────────────────────────┤
│  💬 AI Chat                 │
│  Visual Edit                │
│  History                    │
└─────────────────────────────┘
```

---

**Updated:** November 18, 2025 (HashRouter compatibility)

