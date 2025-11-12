# 🔍 Iframe Troubleshooting Guide

## The Root Cause: Mixed Content Blocking

**THIS WAS THE REAL ISSUE!** 

When your Amplify app runs on HTTPS (e.g., `https://yourapp.amplifyapp.com`), browsers block HTTP iframes for security reasons. This is called "Mixed Content Blocking."

## The Complete Fix

### 1. ✅ Use HTTPS URLs for Wayback Machine
Changed from:
```javascript
const WAYBACK_SNAPSHOT_URL = 'http://web.archive.org/web';
```
To:
```javascript
const WAYBACK_SNAPSHOT_URL = 'https://web.archive.org/web';
```

### 2. ✅ Use the `id_` Flag
The Wayback Machine's identity mode for embeddable content:
```
https://web.archive.org/web/19961017235908id_/http://www2.yahoo.com:80/
```

### 3. ✅ Configure CSP Headers in amplify.yml
Allow iframes from web.archive.org:
```yaml
customHeaders:
  - pattern: '**'
    headers:
      - key: 'Content-Security-Policy'
        value: "frame-src 'self' https://web.archive.org http://web.archive.org;"
```

### 4. ✅ Add Debug Info
Show the URL being loaded so you can verify it's correct.

## How to Verify the Fix

### After Deployment (2-3 minutes):

1. **Open your Amplify app in browser**
2. **Open DevTools Console** (F12)
3. **Summon yahoo.com**
4. **Check the debug URL** - Should show:
   ```
   Loading: https://web.archive.org/web/19961017235908id_/http://www2.yahoo.com:80/
   ```
5. **Look for console errors**:
   - ❌ "Mixed Content" error = Still using HTTP (shouldn't happen now)
   - ❌ "Refused to frame" = CSP issue (check amplify.yml deployed)
   - ✅ "Iframe loaded successfully" = IT WORKS! 🎉

### Test Locally:

1. **Restart your backend:**
   ```bash
   node backend-server.js
   ```

2. **Start frontend:**
   ```bash
   cd frontend
   npm run dev
   ```

3. **Test at http://localhost:5173**
   - Should work with both HTTP and HTTPS Wayback URLs locally

## Common Issues & Solutions

### Issue 1: Still seeing blank iframe
**Check:** Is the URL using HTTPS?
- Open DevTools → Network tab
- Look for the API call to `/api/resurrect`
- Check the `snapshotUrl` in the response
- Should start with `https://` not `http://`

**Fix:** Make sure you pulled the latest code and redeployed

### Issue 2: "Refused to frame" error
**Check:** CSP headers
- Open DevTools → Network tab
- Click on your page HTML
- Check Response Headers
- Look for `Content-Security-Policy`
- Should include `frame-src ... https://web.archive.org`

**Fix:** Verify `amplify.yml` has the customHeaders section

### Issue 3: Iframe loads but shows Wayback toolbar
**Check:** Is the URL using `id_` flag?
- The URL should have `id_` after the timestamp
- Example: `.../19961017235908id_/http://...`

**Fix:** Update backend-server.js and lambda-handler.js

### Issue 4: Works locally but not in production
**This is the mixed content issue!**
- Local dev uses HTTP, so HTTP iframes work
- Production uses HTTPS, so HTTP iframes are blocked
- Solution: Use HTTPS Wayback URLs (already fixed)

## Testing Different Snapshots

Some snapshots work better than others. Try these:

```javascript
// Yahoo 1996 (usually works great)
https://web.archive.org/web/19961017235908id_/http://www2.yahoo.com:80/

// Space Jam 1996 (iconic!)
https://web.archive.org/web/19961219235908id_/http://www.warnerbros.com/spacejam/

// CNN 1996
https://web.archive.org/web/19961219235908id_/http://www.cnn.com/

// Amazon 1999
https://web.archive.org/web/19991013010720id_/http://www.amazon.com/
```

## Final Checklist

- [ ] Backend uses `https://web.archive.org`
- [ ] URLs include `id_` flag
- [ ] `amplify.yml` has customHeaders with CSP
- [ ] Code is committed and pushed
- [ ] Amplify deployment completed (check AWS Console)
- [ ] Browser cache cleared (Ctrl+Shift+R)
- [ ] DevTools Console shows no errors
- [ ] Debug URL shows HTTPS

## Still Not Working?

1. **Check Amplify build logs** - Did the deployment succeed?
2. **Verify the environment** - Is VITE_API_URL set correctly?
3. **Try the fallback link** - Does it open the Wayback Machine?
4. **Test with different sites** - Some snapshots may have their own restrictions

## The Gothic Archaeologist Says...

*"The spirits were trapped in three layers of digital barriers: the Wayback Machine's anti-embedding protection, the mixed content security policy, and the Content Security Policy headers. With HTTPS incantations and the id_ ritual, the séance chamber is now properly configured. The ghosts shall manifest!"* 🔮

---

**Last Updated:** After fixing the HTTPS mixed content issue
**Status:** Should be working now! 👻
