# VERICORE Test

A small email OTP verification test page.

## Flow

1. Enter an email address and press **Go**.
2. The page asks the VERICORE Worker to send a one-time code.
3. Enter the 6-digit code and press **Verify code**.
4. If the server confirms the code, the page displays **It Works!**
5. Press **Again** to restart the flow.

## Important backend requirement

GitHub Pages only hosts the frontend. It cannot send email or securely verify OTPs by itself.

The page is configured to call these Cloudflare Worker endpoints:

- `POST https://vericore.journey-home-verify.workers.dev/api/send-code`
  - Request JSON: `{"email":"person@example.com"}`
  - Expected response JSON: `{"challengeId":"..."}`
- `POST https://vericore.journey-home-verify.workers.dev/api/verify-code`
  - Request JSON: `{"challengeId":"...","code":"123456"}`
  - Expected success response: HTTP 200 with JSON.

**Those API routes must be implemented in the Worker before real emails can be sent.** The frontend does not contain Gmail credentials, refresh tokens, or OTP secrets.

The Worker must allow the GitHub Pages origin `https://dwiptamoyforleo-lang.github.io` in its CORS policy and must enforce OTP expiry, attempt limits, one-time use, and sending rate limits.

## GitHub Pages

In the repository settings, open **Pages**, choose **Deploy from a branch**, select **main** and **/(root)**, then save. The site URL will be:

https://dwiptamoyforleo-lang.github.io/vericore-test/
