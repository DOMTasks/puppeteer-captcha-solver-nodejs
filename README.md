
# DOM Tasks

Integrate DeathByCaptcha (Text CAPTCHA and reCAPTCHA) into Puppeteer in seconds.

```ts
await solver.solve({
  type: "recaptcha",
  page,
});
```

✔ **No** polling loops

✔ **No** API requests

✔ **No** token injection

✔ **No** balance fetching

✔ **No** manual retry logic


---
### 🎥 See it in action

<table>
  <tr>
    <td width="48%">
      <h4>Automate Image CAPTCHAs in seconds with puppeteer-captcha</h4>
      <a href="https://www.youtube.com/watch?v=5aHVrQcuPZQ" target="_blank">
        <img src="https://github.com/user-attachments/assets/fd6d8395-3e2f-4576-a1ca-3400468a559b" alt="Image CAPTCHA automation demo with puppeteer-captcha">
      </a>
    </td>
    <td width="4%"></td>
    <td width="48%">
      <h4>Automate reCaptcha in seconds with puppeteer-captcha</h4>
      <a href="https://www.youtube.com/watch?v=u-Eomx0o7bg" target="_blank">
        <img src="https://github.com/user-attachments/assets/6907be35-fd5e-4feb-8dfc-2d32ede34801" alt="ReCAPTCHA v2 automation demo with puppeteer-captcha">
      </a>
    </td>
  </tr>
</table>


⭐ **If this project helped you, please consider starring the repository.**


## Getting Started

DOM Tasks 'puppeteer-captcha' SDK integrates with DeathByCaptcha to solve captchas.

Before you begin: 

👉 **[Create a DeathByCaptcha account](https://deathbycaptcha.com/register?refid=6184513722b)**. When you're ready to solve captchas, simply **[add credits to it](https://deathbycaptcha.com/user-pay?refid=1237486458a)**; packages start at just **$5**.

The SDK examples below use it throughout, and you'll receive the API credentials needed in the next step.


Then:

📦 npm
```bash
npm install puppeteer-captcha
```
<br>
<br>

> # 🚀 Start Here
>
> **Don't scroll yet.**
>
> Choose the path that matches your use case.
>
> ### Step 1 - I want to...
>
>- 👉 **[Solve a text captcha](#solving-text-captcha)**
>- 👉 **[Solve a reCAPTCHA v2](#solving-recaptcha-v2)**
>
> Once you've selected a guide above, you'll jump directly to that section. Which we guide you step by step.

<br><br><br><br>

---
## Solving Text captcha

### Step 2 - I want to authenticate with...

- 👉 **[DBC Username & Password](#username-and-password-authentication)**
- 👉 **[DBC Authtoken (2FA)](#authtoken-authentication)**


### Username and password authentication


Authenticate using your DeathByCaptcha account credentials.

```js
import { CaptchaSolver } from "puppeteer-captcha";

const solver = new CaptchaSolver({
    auth: {
        type: "credentials",
        username: process.env.DBC_USERNAME,
        password: process.env.DBC_PASSWORD
    }
});
```

**`type`:** Should be set to 'credentials' for username and password authentication.

**`username`:** Your DeathByCaptcha account username.

**`password`:** Your DeathByCaptcha account password.

We recommend storing credentials in environment variables.


### Step 3 - Solve captcha

- 👉 **[Solve text captcha](#solving-text-captchas)**

___

### Authtoken authentication

**a) Setting up 2FA (authtoken) with DeathByCaptcha**

1. **[Log in](https://deathbycaptcha.com/login?refid=6184513722b)** to your account
2. Go to `User Settings` at the bottom of the page
3. Click on `Authentication options`
4. Check the box "Enable this to use 2FA authentication" and click on SUBMIT
5. Download Google Authenticator app, open it and click on the '+' button to add a new entry.
6. Scan the QR code with your phone through the app and insert the code from the app into 'Verification code' field on the site.
7. Copy and save your "Authentication Token" as an environment variable in your code and finally check the box "Enable this to use the token instead of User/Password combination" on the site. Authentication Token is ~156 characters long.

**Sample token:**
```Bash
CQ6dd4DzLk5y830M2S16TnNEAqetNp2VmAPPx6CjM5wxRYpS0zm0CTMH38iBKfC3qf4tB9d2XIpzI184ZJv0G2RMUUcHi60372MRxUkE5A71bDopA3aQum2029LlLwX4R61hwr7fR51p6zRADdhT9u07e9v6
```
**b) Initialize solver with token created:**

```js
import { CaptchaSolver } from "puppeteer-captcha";

const solver = new CaptchaSolver({
    auth: {
        type: "authtoken",
        authtoken: process.env.DBC_AUTHTOKEN
    }
});
```

**`type`:** Should be set to 'authtoken' for authoken authentication.

**`authtoken`:** Your DeathByCaptcha `authtoken`.

We recommend storing it in an environment variable instead of hardcoding it in your application.






## Solving Text Captchas

You can use the **`solve()`** function from your solver object to solve the challenge on the current page.


```js
const result = await solver.solve({
  type: "image",
  page,
  captcha: await page.$("#demoCaptcha_CaptchaImage"),
  input: await page.$("#captchaCode"),
});
```

**`type`:** The `type` property determines which challenge the toolkit should process. Depending on the selected type, additional properties may be required.

**`page`:** A Puppeteer `Page` instance representing the current browser page.

Example:

```js
const page = await browser.newPage();
```

**`captcha`:** A Puppeteer `Locator` representing the image challenge.

  Supported element types include:

  - `<img>`

  - `<canvas>`

  - `<svg>`

  - Container elements wrapping the challenge
    
Example:

```js
const captcha = await page.$("#captchaImage");
```

---

**`input`:** A Puppeteer `Locator` representing the text input where the challenge response should be entered.

Example:

```js
const input = await page.$("#input");
```


### Captcha solution metadata

You can log/get useful information returned as a result of calling the solve() method.

```js
const result = await solver.solve({
    type: "image",
    page,
    captcha,
    input
});

console.log(result);
```

Sample response:

```bash
{
  success: true,
  challenge: 'image',
  duration: 4124,
  id: '235368206',
  balance: 10.081007
}
```

The returned object contains:

**`success`** — Indicates operation success (however, if after submitting the form to complete the automation captcha is incorrect it should be reported as incorrectly solved).

**`challenge`** — The challenge type that was processed.

**`duration`** — Total execution time in milliseconds.

**`id`** — The captcha ID.

**`balance`** — Balance after solving the challenge.



### Reporting incorrect captchas:

You can use the `report()` function from your solver object.

```js
const result = await solver.solve({
    type: "image",
    page,
    captcha,
    input
});

await solver.report(result.id)
```

Some captcha responses will be incorrect, will not mark the challenge as solved since the text injected doesn't match that of the captcha image, you can code a condition to evaluate whether or not the captcha is marked as correctly solved on your target page, some sites may add a label to the DOM if solved correctly/may take other approaches.
With the `report(captcha_id)` method you can report those captchas that are incorrect and you won't get charged for them. Note that you shouldn't abuse this feature, abuse may get your account suspended if captchas are correct. 




## Text captcha full code example
`npm install puppeteer dotenv puppeteer-captcha`

```js
import puppeteer from "puppeteer";
import dotenv from "dotenv";
import { CaptchaSolver } from "puppeteer-captcha";

dotenv.config();

const SUBMISSION_URL =
  "https://captcha.com/demos/features/captcha-demo.aspx";

const solver = new CaptchaSolver({
  auth: {
    type: "credentials",
    username: process.env.DBC_USERNAME,
    password: process.env.DBC_PASSWORD,
  },
});

const browser = await puppeteer.launch({
  headless: false,
});

const page = await browser.newPage();

await page.goto(SUBMISSION_URL, {
  waitUntil: "networkidle2",
});

const captcha = await page.$("#demoCaptcha_CaptchaImage");
const input = await page.$("#captchaCode");

if (!captcha || !input) {
  throw new Error("Captcha elements not found.");
}

const result = await solver.solve({
  type: "image",
  page,
  captcha,
  input,
});

console.log(result);

await page.click("#validateCaptchaButton");
```

### Next Step — Error handling:

- 👉 [View SDK error handling section](#error-handling)

<br><br><br><br>

# Solving reCAPTCHA v2

### Step 2 - I want to authenticate with...

- 👉 **[DBC Username & Password](#username-password-authentication)**
- 👉 **[DBC Authtoken (2FA)](#authtoken-authentication-method)**


## Username password authentication


Authenticate using your DeathByCaptcha account credentials.

```js
import { CaptchaSolver } from "puppeteer-captcha";

const solver = new CaptchaSolver({
    auth: {
        type: "credentials",
        username: process.env.DBC_USERNAME,
        password: process.env.DBC_PASSWORD
    }
});
```
**`type`:** Should be set to 'credentials' for username and password authentication.

**`username`:** Your DeathByCaptcha account username.

**`password`:** Your DeathByCaptcha account password.

We recommend storing credentials in environment variables.

### Step 3 - Solve reCAPTCHAS

- 👉 **[Automate reCAPTCHA v2](#solving-recaptchas-v2)**

___

## Authtoken authentication method

**a) Setting up 2FA (authtoken) with DeathByCaptcha**

1. **[Log in](https://deathbycaptcha.com/login?refid=6184513722b)** to your account
2. Go to `User Settings` at the bottom of the page
3. Click on `Authentication options`
4. Check the box "Enable this to use 2FA authentication" and click on SUBMIT
5. Download Google Authenticator app, open it and click on the '+' button to add a new entry.
6. Scan the QR code with your phone through the app and insert the code from the app into 'Verification code' field on the site.
7. Copy and save your "Authentication Token" as an environment variable in your code and finally check the box "Enable this to use the token instead of User/Password combination" on the site. Authentication Token is ~156 characters long.

**Sample token:**
```Bash
CQ6dd4DzLk5y830M2S16TnNEAqetNp2VmAPPx6CjM5wxRYpS0zm0CTMH38iBKfC3qf4tB9d2XIpzI184ZJv0G2RMUUcHi60372MRxUkE5A71bDopA3aQum2029LlLwX4R61hwr7fR51p6zRADdhT9u07e9v6
```
**b) Initialize solver with token created:**

```js
import { CaptchaSolver } from "puppeteer-captcha";

const solver = new CaptchaSolver({
    auth: {
        type: "authtoken",
        authtoken: process.env.DBC_AUTHTOKEN
    }
});
```
**`type`:** Should be set to 'authtoken' for authoken authentication.

**`authtoken`:** Your DeathByCaptcha `authtoken`.

We recommend storing it in an environment variable instead of hardcoding it in your application.


## Solving reCAPTCHAs v2

```js
await solver.solve({
    type: "recaptcha",
    page,
 // proxy: "http://user:password@your-proxy-provider.com",
});
```

**`type`**

The challenge type.

```js
type: "recaptcha"
```

---

**`page`**

A Puppeteer `Page` instance representing the current browser page.

Example:

```js
const page = await browser.newPage();
```

**`proxy`**

OPTIONAL PARAMETER: A HTTP proxy string (only HTTP proxies supported) from your desired provider.

Example:

```js
proxy: "http://user:password@your-proxy-provider.com"
```

---

### Captcha solution metadata

You can log/get useful information returned as a result of calling the solve() method.

Method `solve()` returns a `Promise<SolveResult>`.

Example:

```js
const result = await solver.solve({
    type: "recaptcha",
    page,
});

console.log(result);
```

Sample response:

```bash
{
  success: true,
  challenge: 'recaptcha',
  duration: 14124,
  id: '235368206',
  balance: 10.091007
}
```

The returned object contains:

- **success** — Indicates operation success.

- **challenge** — The challenge type that was processed.

- **duration** — Total execution time in milliseconds.

- **id** — The captcha ID.

- **balance** — Balance after solving the challenge.



## ReCAPTCHA full code example
`npm install puppeteer dotenv puppeteer-captcha`

```js
import puppeteer from "puppeteer";
import { CaptchaSolver } from "puppeteer-captcha";
import dotenv from "dotenv";

dotenv.config();

const SUBMISSION_URL = "https://www.google.com/recaptcha/api2/demo";

const solver = new CaptchaSolver({
  auth: {
    type: "credentials",
    username: process.env.DBC_USERNAME,
    password: process.env.DBC_PASSWORD,
  },
});

const browser = await puppeteer.launch({
  headless: false,
});

const page = await browser.newPage();

await page.goto(SUBMISSION_URL, {
  waitUntil: "networkidle2",
});

const result = await solver.solve({
  type: "recaptcha",
  page,
});

console.log(result);
/*
{
  success: true,
  challenge: 'recaptcha',
  duration: 34124,
  id: '435368906',
  balance: 3.091
}
*/
await Promise.all([
  page.waitForNavigation({ waitUntil: "networkidle2" }).catch(() => {}),
  page.click("#recaptcha-demo-submit"),
]);

await browser.close();

```


## Error Handling


The SDK throws typed errors instead of generic `Error` objects.

#### Possible Errors

| Error                 | Description                                                |
| --------------------- | ---------------------------------------------------------- |
| NetworkError          | Network error, conexion lost.                              |
| TimeoutError          | The operation exceeded the configured timeout.             |
| CaptchaSolverError    | The provider returned an error.                            |



# Why DOM Tasks?

DOM Tasks is designed to feel like a native extension to Puppeteer rather than another automation framework.

Instead of forcing you to learn a new API, it integrates directly into your existing workflow.

```python
await solver.solve({
  type: "recaptcha",
  page,
});
```

Everything else remains under your control.

- Use your own waits.
- Use your own logging.
- Use your own retry strategy.
- Use your own browser configuration.
- Use your own proxies.
- Continue using Puppeteer exactly as you already do.

The SDK focuses on solving CAPTCHAs and immediately returns control back to your automation.

# RESPONSIBLE USE
We encourage the responsible and ethical use of automation technologies and does not endorse or encourage the misuse of this software to violate applicable laws, contractual obligations, or the rights of others.



# License

Copyright © 2026 DOM Tasks.

LICENSE GRANT

Subject to this License, you are granted permission to:

    ✓ Install and use this software.

    ✓ Use this software in personal projects and in commercial or internal business applications developed by you or your organization.

    ✓ Modify this software solely for your own internal use.


You may NOT:


    ✗ Redistribute this software, whether modified or unmodified.

    ✗ Publish or make available modified versions of this software to any third party.

    ✗ Sell, sublicense, rent, lease, assign, or otherwise transfer this software or any modified version of it.

    ✗ Remove, alter, or obscure any copyright notices, trademarks, branding, attribution, or license notices contained in the software.

    ✗ Represent modified versions as the original software.

    ✗ Use the software in violation of applicable laws.

    ✗ Use this software or any substantial portion of it to develop, distribute, or commercialize a competing software library, SDK, or similar product.


OWNERSHIP

    No ownership rights are transferred under this License. Ownership of the software and all intellectual property rights remain with the copyright holder.

    All rights not expressly granted under this License are reserved by the copyright holder.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND...

# Disclaimer:
This is an unofficial package and it is not affiliated or endorsed by the maintainers of Puppeteer, package name indicates compatibility.
