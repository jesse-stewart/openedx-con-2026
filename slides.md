---
theme: ./theme
title: "Beyond Test Coverage: E2E · A11y · Documentation · Visual Regression Testing"
drawings:
  persist: false
transition: view-transition
comark: true
duration: 35min
layout: title-slide
---

<!--
I'm very happy to be here :)
Presenting something I am very passionate about.
-->

---
layout: about-slide
transition: view-transition
---

I'm a **Staff Software Engineer** at **Western Governors University** working on the **Open edX platform**. With a background in both software engineering and design, I focus on building scalable systems and improving the tools and experiences used by educators, developers, and learners.

<!--
I'm very happy to be here :)
Presenting something I am very passionate about.
-->

---
transition: slide-left
level: 2
---

# The Problem

E2E tests are expensive. Three kinds of expensive.

<div v-click>

**Creation Cost** — Time and expertise that most organizations don't have in abundance.

</div>

<div v-click>

**Runtime Cost** — Every test run takes time on developer machines or CI compute time.

</div>

<div v-click>

**Maintenance Cost** — Keeping selectors up to date and test environments stable.

</div>

<div v-click>

Given all that cost, most teams extract **one output**: a pass/fail signal. That's leaving significant value on the table.

</div>

<!--
Writing tests is something I am not excited about.
- They are expensive to Create
- They are expensive to Run
- They are expensive to Maintain

I'm not going to fix those things. But I can show you how to extract more value from your tests.
-->

---
transition: slide-left
level: 2
---

# More Problems

But wait, there's more!

<div v-click>

**Accessibility** — Audits are usually manual and don't happen often enough. Violations accumulate over time without being fixed.

</div>

<div v-click>

**Documentation** — Writing documentation is a manual process that is time consuming for something that has a very short shelf life.

</div>

<div v-click>

**Screenshots** — UI changes ship constantly and screenshots go out of date quickly.

</div>

<div v-click>

**Visual Regression** — Subtle layout changes and UI drift are hard to detect between releases.

</div>

<!--
What I can fix!
- A11y audits baked right into your tests.
- Documentation as an artifact of running tests.
- Production ready screenshots
- Visual regression tests that expose UI drift
-->

---
transition: slide-left
level: 2
---

# The Solution

The parts of Open edX worth **documenting** are almost always the parts worth writing **E2E, A11y, and Visual Regression tests** for. So why maintain multiple separate efforts?

<div class="mt-6" v-click>
Introducing:
<div class="flex gap-12">
<div class="whitespace-nowrap">

## openedx-e2e-tests

<img src="/qr-github-repo.png" class="w-64 mt-4" />

</div>
<div>

A comprehensive **Playwright** testing library for Open edX with automated documentation generation, accessibility testing, and visual regression capabilities.

https://github.com/WGU-Open-edX/openedx-e2e-tests

```bash
npm install openedx-e2e-tests
```

</div>
</div>
</div>

<!--
I developed a set of tools that work along side Playwright to more easily write tests that are more valuable than test/fail.

Feel free to look at the code while I talk. This can be installed in any frontend code repo, or as a standalone test suite.
-->

---

# One Codebase. Four Outputs.

Write the test once. Get everything else for (almost) free.

<div v-click>

**Test Coverage** — `playwright/test` Validate critical user flows across MFEs, authentication, and LMS workflows.

</div>
<div v-click>

**Accessibility Reports** — `assertA11y` WCAG violations caught during every test run, with visual evidence and story-ready descriptions.

</div>
<div v-click>

**Documentation** — `TestdocTest` Auto-generated user guides with annotated screenshots, step by step.

</div>
<div v-click>

**Visual Regression** — `VisualRegression` Pixel-level diff detection with red-highlighted changes. Catches what functional tests miss.

</div>

<!--
I will show you how a single test can be expanded to include:
- A11y reports, with detailed screenshots of violations.
- Documentation, with production ready screenshots.
- Pixel level diff detection with highlighted changes
-->

---

```ts
// Vanilla Playwright — one output: pass/fail
import { test } from '@playwright/test';

test('user can log in', async ({ page }) => {
  await page.goto('/login');
  await page.fill('#username', process.env.TEST_USERNAME!);
  await page.fill('#password', process.env.TEST_PASSWORD!);
  await page.click('button[name="sign-in"]');
});
```

<!--
This is a vanilla Playwright test. Not a particularly complex example. It:
- goes to the login page
- fills in username and password
- clicks the submit button
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>Running Tests</h1>

Run tests headlessly, with a visible browser, in an interactive UI, or with the debugger attached.

```bash
# Run all tests
playwright test

 # With visible browser
playwright test --headed

# Interactive UI mode
playwright test --ui

# With debugger
playwright test --debug

```

::default::
<video src="/video/08-running-tests.mov" autoplay loop muted class="aspect-square"></video>

<!--
Playwright provides a set of tools that make writing and running tests easier.
 - headed mode allows you to watch the tests run in the browser.
- UI mode is helpful because it allows you to debug failing tests with a timeline view.
- Debug mode opens Playwright Inspector, which provides a visual interface to inspect elements, edit locators, and view logs.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>Test Results</h1>

Rich HTML reports with screenshots, traces, and test results. Open in your browser to drill into failures.

```bash
# View Playwright HTML report
npm run report
```

::default::
<video src="/video/09-test-results.mov" autoplay loop muted class="aspect-square"></video>

<!--
Playwright provides some test results, failed test have a screen recording to help you debug the failing test. This is great.
-->

---
layout: center
---

<img src="/video/09-ariel-moooore.gif" class="mx-auto" />

<!--
But I'm not satisifed and I want more from my Playwright tests
-->

---

````md magic-move {lines: true}
```ts
// Vanilla Playwright
import { test } from '@playwright/test';

test('user can log in', async ({ page }) => {
  await page.goto('/login');
  await page.fill('#username', process.env.TEST_USERNAME!);
  await page.fill('#password', process.env.TEST_PASSWORD!);
  await page.click('button[name="sign-in"]');
});
```

```ts {3,8}
// + assertA11y — now also runs an accessibility audit
import { test } from '@playwright/test';
import { assertA11y } from 'openedx-e2e-tests';

test('user can log in', async ({ page }) => {
  await page.goto('/login');

  await assertA11y(page, { report: true }, testInfo);

  await page.fill('#username', process.env.TEST_USERNAME!);
  await page.fill('#password', process.env.TEST_PASSWORD!);
  await page.click('button[name="sign-in"]');
});
```
````

<!--
We lean on axe-core/playwright for most of the heavy lifting here, but we augment it a bit for our purpose.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>A11y Test Results</h1>

Violations are compiled into a dedicated report with screenshots, descriptions, and remediation guidance.

```bash
# View accessibility reports
npm run report:a11y
```

::default::
<video src="/video/11-a11y-test-results.mov" autoplay loop muted class="aspect-square"></video>

<!--
Our a11y reports include high resolution screenshots that highlight each violation, which provides valuable feedback during remediation.
-->

---

````md magic-move {lines: true}
```ts
// assertA11y
import { test } from '@playwright/test';
import { assertA11y } from 'openedx-e2e-tests';

test('user can log in', async ({ page }) => {
  await page.goto('/login');
  
  await assertA11y(page, { report: true }, testInfo);

  await page.fill('#username', process.env.TEST_USERNAME!);
  await page.fill('#password', process.env.TEST_PASSWORD!);
  await page.click('button[name="sign-in"]');
});
```

```ts {3,6-11,16-20}
// + Testdoc — now generates annotated documentation too
import { test } from '@playwright/test';
import { assertA11y, TestdocTest } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into Open edX.',
  });

  await testdoc.initialize();
  await page.goto('/login');

  await assertA11y(page, { report: true }, testInfo);

  await testdoc.fill('#username', process.env.TEST_USERNAME!, 'Enter Username');
  await testdoc.fill('#password', process.env.TEST_PASSWORD!, 'Enter Password');
  await testdoc.click('button[name="sign-in"]', 'Submit Login');

  await testdoc.generateRST();
});
```
````

<!--
Generating documentation requires a bit of bootstrapping for the document header, then we can just write tests normally.

Replacing our fill and click calls with testdoc will document those steps in our documentation files.

Then we tell the test to generate RST or Markdown files.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>Generated Documentation</h1>

Automatically generate RST or Markdown files from your E2E with production ready screenshots.

::default::
<video src="/video/13-generated-documentation.mov" autoplay loop muted class="aspect-square"></video>

<!--
The results are production-quality screenshots and step-by-step instructions. You can confidently deploy your documentation knowing that it accurately reflects the code your just deployed.
-->

---

````md magic-move {lines: true}
```ts
// Testdoc
import { test } from '@playwright/test';
import { assertA11y, TestdocTest } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into Open edX.',
  });

  await testdoc.initialize();
  await page.goto('/login');

  await assertA11y(page, { report: true }, testInfo);

  await testdoc.fill('#username', process.env.TEST_USERNAME!, 'Enter Username');
  await testdoc.fill('#password', process.env.TEST_PASSWORD!, 'Enter Password');
  await testdoc.click('button[name="sign-in"]', 'Submit Login');

  await testdoc.generateMarkdown();
});
```

```ts {3,10,16}
// + VisualRegression — pixel-level diff on every run
import { test } from '@playwright/test';
import { assertA11y, TestdocTest, VisualRegression } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into Open edX.',
  });
  const vr = new VisualRegression(page, testInfo);

  await testdoc.initialize();
  await page.goto('/login');

  await assertA11y(page, { report: true }, testInfo);
  await vr.captureAndCompare({ name: 'login-page' });

  await testdoc.fill('#username', process.env.TEST_USERNAME!, 'Enter Username');
  await testdoc.fill('#password', process.env.TEST_PASSWORD!, 'Enter Password');
  await testdoc.click('button[name="sign-in"]', 'Submit Login');

  await testdoc.generateRST();
});
```
````

<!--
Take screenshots throughout your test to compare your changes to the baselines screenshots. Baselines screenshots can be regenerated in CI/CD process when new code is merged or part of the PR process.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>Visual Regression Testing</h1>

Pixel-by-pixel comparison against a stored baseline. Differences are flagged with red-highlighted diffs.

::default::
<div class="aspect-square bg-gray">[VIDEO]</div>

---
layout: api-section
---

# Generating Documentation

Powered by `@playwright/test`

---

````md magic-move {lines: true}
```ts
// Minimal setup
const testdoc = new TestdocTest(page, 'user-login');

await testdoc.initialize();
```

```ts
// Full constructor options
const testdoc = new TestdocTest(page, 'user-login', {
  title: 'How to Log In',
  overview: 'Step-by-step guide to logging into Open edX.',
  prerequisites: ['Active account', 'Valid credentials'],
  notes: ['Use Forgot Password if needed'],
  relatedTopics: [
    { title: 'Create Account', url: '/signup' },
    'Password requirements',
  ],
  showNumbers: true,
});

await testdoc.initialize();
```
````
---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
TestdocTest(
  title
  overview
  prerequisites
  notes
  relatedTopics
  showNumbers
)
</pre>

---

````md magic-move {lines: true}
```ts
// step() — screenshot with title and description
await testdoc.step({
  title: 'Navigate to the login page',
  description: 'Open the sign-in form from the home page.',
});
```

```ts
// fill() — highlight input, fill value, screenshot
await testdoc.step({
  title: 'Navigate to the login page',
  description: 'Open the sign-in form from the home page.',
});

await testdoc.fill({
  selector: 'input[name="email"]',
  value: 'user@example.com',
  title: 'Enter your email address',
  elementOnly: true,  // crop screenshot to the element
  padding: 30,
});
```

```ts
// click() — highlight element, click it, screenshot
await testdoc.step({
  title: 'Navigate to the login page',
  description: 'Open the sign-in form from the home page.',
});

await testdoc.fill({
  selector: 'input[name="email"]',
  value: 'user@example.com',
  title: 'Enter your email address',
  elementOnly: true,
  padding: 30,
});

await testdoc.click({
  selector: 'button[type="submit"]',
  title: 'Click Sign In',
  description: 'Submit your credentials.',
  elementOnly: true,
});
```
````
---
layout: full
---

<pre class="aspect-video bg-gray">[SCREENSHOT] 
testdoc.step
testdoc.fill
testdoc.click
</pre>

---

````md magic-move {lines: true}
```ts
// screenshot() + note() + generate output
await testdoc.step({ title: 'Navigate to the login page' });

...

await testdoc.screenshot({
  title: 'Dashboard loaded',
  description: "You're now logged in.",
});

await testdoc.note('Bookmark this page for quick access.');

await testdoc.generateMarkdown();  // → documentation.md
await testdoc.generateRST();       // → documentation.rst
```
````

---
layout: full
---

<div class="grid grid-cols-3" style="--slidev-code-font-size: 0.5rem; --slidev-code-line-height:0.625rem;">
<div class="h-full">

```rst
######################
RST
######################
```

</div>
<pre class="h-full bg-gray">[SCREENSHOT] 
[Preview]
</pre>
<div class="h-full">

```md
# markdown
```

</div>
</div>

---
layout: api-section
---

# A11y Tests

Powered by `@axe-core/playwright`

---

```ts
// checkA11y — returns results, never throws
import { checkA11y } from 'openedx-e2e-tests';

const results = await checkA11y(page);

console.log(`Violations: ${results.violations.length}`);
console.log(`Passes: ${results.passes.length}`);
```
---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
console.log
</pre>
---

```ts
// assertA11y — throws on violations
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {}, testInfo);
});
```
---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
assertA11y
</pre>
---

```ts
// warnOnly — log instead of throw (good for onboarding)
test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {
    warnOnly: true,
  }, testInfo);
});
```
---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
assertA11y
warnOnly
</pre>
---

```ts
// report — auto-generate HTML report after each run
test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {
    warnOnly: true,
    report: true,
    reportName: 'login-page',  // distinguish multiple checks in one test
  }, testInfo);
});
```
---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
report
</pre>
---

```ts
// disabledRules + exclude — fine-tune what gets scanned
test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {
    warnOnly: true,
    report: true,
    reportName: 'login-page',
    disabledRules: ['color-contrast'],   // known false positive
    exclude: ['.third-party-widget'],    // out of our control
  }, testInfo);
});
```

---
layout: api-section
---

# Visual Regression

Powered by `pixelmatch`

---

```ts
// Basic setup — first run creates the baseline
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);

  await page.goto('/login');
  await vr.captureAndCompare({ name: 'login-page' });
});
```

---
layout: full
---

<pre class="aspect-video bg-gray">[SCREENSHOT]
captureAndCompare
</pre>

---

````md magic-move {lines: true}
```ts
// Basic setup — first run creates the baseline
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);

  await page.goto('/login');
  await vr.captureAndCompare({ name: 'login-page' });
});
```

```ts
// + fullPage — capture the entire scrollable page
test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);

  await page.goto('/login');
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
  });
});
```

```ts
// + mask — exclude dynamic content from comparison
test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);

  await page.goto('/login');
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
    mask: ['.timestamp', '[data-testid="user-greeting"]'],
  });
});
```

```ts
// + threshold — tune sensitivity for font rendering differences
test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);

  await page.goto('/login');
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
    mask: ['.timestamp', '[data-testid="user-greeting"]'],
    threshold: 0.15,  // 0.1 default · 0.05 strict · 0.15 lenient
  });
});
```
````
---
layout: full
---

<pre class="aspect-video bg-gray">[SCREENSHOT]
captureAndCompare
threshold
</pre>

---
layout: api-section
---

# Wrapping up

---

# New Problems

What still needs to be solved

- How do we bootstrap test environments with data that our tests rely on?
- How do we cleanup after tests run?
- Can we run tests again mock data and database data?

---

# How to contrubute

<div class="flex gap-12">

<img src="/qr-github-repo.png" class="w-64 mt-4" />

<div>
<div v-click>

Install

```bash
npm install openedx-e2e-tests
```

</div>
<div v-click>

Report Bugs

```
https://github.com/WGU-Open-edX/openedx-e2e-tests/issues
```

</div>
<div v-click>

Contribute

```
https://github.com/WGU-Open-edX/openedx-e2e-tests
```

</div>
</div>
</div>

---
layout: api-section
---

# Thank You

Questions? Feedback? Find me after the talk.

<img src="/beyond-test-coverage-e2e-a11y-documentation-an_stewart-balderrama_1177129_feedback-code.png" class="w-64 mt-4 -mb-10vh" />
