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

I'm a **Staff Software Engineer** at **Western Governors University** working on the **Open edX® platform**. With a background in both software engineering and design, I focus on building scalable systems and improving the tools and experiences used by educators, developers, and learners.

<!--
A bit about me — I bridge engineering and design, which is part of why I built this tool. Both perspectives kept telling me that tests should produce more than pass/fail.
-->

---
transition: slide-left
level: 2
---

# The Problem

E2E tests are expensive. Three kinds of expensive.

<div v-click className="mb-6">

<iconoir-code class="inline-block align-middle font-size-3xl" /> **Creation Cost** — Time and expertise that most organizations don't have in abundance.

</div>

<div v-click className="mb-6">

<iconoir-timer class="inline-block align-middle font-size-3xl" /> **Runtime Cost** — Every test run takes time on developer machines or CI compute time.

</div>

<div v-click className="mb-6">

<iconoir-tools class="inline-block align-middle font-size-3xl" /> **Maintenance Cost** — Keeping selectors up to date and test environments stable.

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
layout: center
---

<img src="/video/theres-more.gif" class="mx-auto max-w-none w-60vh" />

<!--
But wait, there's more!
-->

---
transition: slide-left
level: 2
---

<h1 text-2xl!>More Problems</h1>

<div v-click className="mb-6">

<iconoir-accessibility class="inline-block align-middle font-size-3xl" /> **Accessibility** — Audits are usually manual and don't happen often enough. Violations accumulate over time without being fixed.

</div>

<div v-click className="mb-6">

<iconoir-book class="inline-block align-middle font-size-3xl" /> **Documentation** — Writing documentation is a manual process that is time consuming for something that has a very short shelf life.

</div>

<div v-click className="mb-6">

<iconoir-camera class="inline-block align-middle font-size-3xl" /> **Screenshots** — UI changes ship constantly and screenshots go out of date quickly.

</div>

<div v-click>

<iconoir-eye class="inline-block align-middle font-size-3xl" /> **Visual Regression** — Subtle layout changes and UI drift are hard to detect between releases.

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

The parts of the Open edX platform worth **documenting** are almost always the parts worth writing **E2E, A11y, and Visual Regression tests** for. So why maintain multiple separate efforts?

<div class="mt-6" v-click>
Introducing:
<div class="flex gap-12 code-lg">
<div class="whitespace-nowrap">

## openedx-e2e-tests

<img src="/qr-github-repo.png" class="w-64 mt-4" />

</div>
<div>

A comprehensive **Playwright** testing library for the Open edX platform with automated documentation generation, accessibility testing, and visual regression capabilities.

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

<div v-click className="mb-6">

<iconoir-check-circle class="inline-block align-middle font-size-3xl" /> **Test Coverage** — `playwright/test` Validate critical user flows across MFEs, authentication, and LMS workflows.

</div>
<div v-click className="mb-6">

<iconoir-accessibility class="inline-block align-middle font-size-3xl" /> **Accessibility Reports** — `assertA11y` WCAG violations caught during every test run, with visual evidence and story-ready descriptions.

</div>
<div v-click className="mb-6">

<iconoir-book class="inline-block align-middle font-size-3xl" /> **Documentation** — `TestdocTest` Auto-generated user guides with annotated screenshots, step by step.

</div>
<div v-click>

<iconoir-eye class="inline-block align-middle font-size-3xl" /> **Visual Regression** — `VisualRegression` Pixel-level diff detection with red-highlighted changes. Catches what functional tests miss.

</div>

<!--
Four artifacts from one test: the pass/fail signal you already get from Playwright, plus accessibility reports, generated documentation, and visual regression diffs. We'll walk through each one.
-->

---
layout: api-section
---

<iconoir-rocket class="font-size-6xl" />

# Quickstart

<!--
I'll go through the basic usage of each feature before digging deeper into each method.
-->

---

<h1 text-2xl!>E2E Tests</h1>

<div class="code-lg">

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

</div>

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
<h1 text-2xl!>E2E Tests</h1>

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
<video src="/video/running-tests.mov" autoplay loop muted class="aspect-square"></video>

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
<h1 text-2xl!>E2E Tests</h1>

Rich HTML reports with screenshots, traces, and test results. Open in your browser to drill into failures.

::default::
<video src="/video/test-results.mov" autoplay loop muted class="aspect-square"></video>

<!--
Playwright provides some test results, failed test have a screen recording to help you debug the failing test. This is great.
-->

---
layout: center
---

<img src="/video/ariel-moooore.gif" class="mx-auto max-w-none w-60vh" />

<!--
I want more
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>A11y Tests</h1>

Generate A11y directly from your E2E tests. Violations are compiled into a dedicated report with screenshots, descriptions, and remediation guidance.

::default::
<video src="/video/a11y-test-results.mp4" autoplay loop muted class="aspect-square"></video>

<!--
Our a11y reports include high resolution screenshots that highlight each violation, which provides valuable feedback during remediation.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

````md magic-move
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

```ts {1,3,8}
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

</div>

<!--
We lean on axe-core/playwright for most of the heavy lifting here, but we augment it a bit for our purpose.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!>Generated Documentation</h1>

Automatically generate RST or Markdown files from your E2E with production ready screenshots.

::default::
<video src="/video/generated-documentation.mov" autoplay loop muted class="aspect-square"></video>

<!--
The results are production-quality screenshots and step-by-step instructions. You can confidently deploy your documentation knowing that it accurately reflects the code your just deployed.
-->

---

<h1 text-2xl!>Generated Documentation</h1>

````md magic-move
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

```ts {1,3,6-11,16-20}
// + Testdoc — now generates annotated documentation too
import { test } from '@playwright/test';
import { assertA11y, TestdocTest } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into the Open edX platform.',
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
<h1 text-2xl!>Visual Regression Testing</h1>

Pixel-by-pixel comparison against a stored baseline. Differences are flagged with red-highlighted diffs.

::default::
<div class="vr-slideshow aspect-[5/3.75]">
  <div class="vr-slide">
    <img src="/screenshots/visual-regression-basline.png" alt="Visual regression baseline screenshot" />
    <div class="vr-caption">1. Baseline screenshot used for comparison</div>
  </div>
  <div class="vr-slide" v-click="1">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">2. New screenshot after code changes</div>
  </div>
  <div class="vr-slide" v-click="2">
    <img src="/screenshots/visual-regression-diff.png" alt="Visual regression diff screenshot" />
    <div class="vr-caption">3. Pixel diff highlighting visual changes</div>
  </div>
  <div class="vr-slide" v-click="3">
    <img src="/screenshots/visual-regression-test-results.png" alt="Visual regression test results" />
    <div class="vr-caption">4. Visual regression test summary report</div>
  </div>
</div>

<!--
Visual regression in four steps: a baseline screenshot is captured on the first run, subsequent runs capture a new screenshot, pixelmatch generates a diff with the changes highlighted in red, and the results land in a summary report you can review.
-->

---

<h1 text-2xl!>Visual Regression Testing</h1>

````md magic-move
```ts
// Testdoc
import { test } from '@playwright/test';
import { assertA11y, TestdocTest } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into the Open edX platform.',
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

```ts {1,3,10,16}
// + VisualRegression — pixel-level diff on every run
import { test } from '@playwright/test';
import { assertA11y, TestdocTest, VisualRegression } from 'openedx-e2e-tests';

test('user can log in', async ({ page }, testInfo) => {
  const testdoc = new TestdocTest(page, 'user-login', {
    title: 'How to Log In',
    overview: 'This guide shows you how to log into the Open edX platform.',
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

# Browser Support

Playwright runs your tests on three browser engines out of the box.

<div class="flex justify-center items-end gap-20 mt-12">
  <div v-click class="text-center">
    <img src="/browser-chromium.png" class="w-32 mx-auto" />
    <div class="mt-6 text-xl font-semibold">Chromium</div>
    <div class="text-sm opacity-70">Chrome, Edge</div>
  </div>
  <div v-click class="text-center">
    <img src="/browser-firefox.png" class="w-32 mx-auto" />
    <div class="mt-6 text-xl font-semibold">Firefox</div>
    <div class="text-sm opacity-70">Gecko engine</div>
  </div>
  <div v-click class="text-center">
    <img src="/browser-webkit.png" class="w-32 mx-auto" />
    <div class="mt-6 text-xl font-semibold">WebKit</div>
    <div class="text-sm opacity-70">Safari engine</div>
  </div>
</div>

<div v-click class="mt-12 text-sm opacity-80 max-w-3xl mx-auto text-center">

**BYOB?** Other Chromium-based browsers (Brave, Arc, Vivaldi, Orion) can be launched via `executablePath` — unofficial, but it works.

</div>

<!--
Playwright officially supports three engines: Chromium, Firefox, and WebKit.
Chrome and Edge are supported via the `channel` option — they use your installed browser instead of bundled Chromium.
Other Chromium-based browsers (Brave, Arc, Vivaldi) can be launched via `executablePath`, but it's not officially supported.
WebKit drives the engine behind Safari, but not Safari itself.
-->

---
transition: slide-left
level: 2
---

# Where Do We Run These Tests?

Three options, each with tradeoffs.

<div class="flex justify-center items-start gap-16 mt-12">
  <div class="text-center flex-1">
    <div v-click>
      <iconoir-laptop class="text-7xl mx-auto block" />
      <div class="mt-6 text-xl font-semibold">Developer Machine</div>
      <div class="text-sm opacity-70 mt-2">Run tests before commit / push</div>
    </div>
    <div class="mt-6 text-sm" v-click>
      <iconoir-timer class="inline-block align-middle font-size-xl" /> Time consuming
    </div>
  </div>
  <div class="text-center flex-1">
    <div v-click>
      <iconoir-git-pull-request class="text-7xl mx-auto block" />
      <div class="mt-6 text-xl font-semibold">PR Opened / Changed</div>
      <div class="text-sm opacity-70 mt-2">CI runs on every push</div>
    </div>
    <div class="mt-6 text-sm" v-click>
      <iconoir-dollar class="inline-block align-middle font-size-xl" /> Expensive
    </div>
  </div>
  <div class="text-center flex-1">
    <div v-click>
      <iconoir-git-merge class="text-7xl mx-auto block" />
      <div class="mt-6 text-xl font-semibold">PR Merged</div>
      <div class="text-sm opacity-70 mt-2">CI runs after merge to main</div>
    </div>
    <div class="mt-6 text-sm" v-click>
      <iconoir-warning-triangle class="inline-block align-middle font-size-xl" /> Too late to catch regressions
    </div>
  </div>
</div>

<!--
Where you run these tests determines what they cost you and what they catch.
- Locally: catches everything before it leaves your machine, but slow feedback loop.
- On PR: catches issues before merge, but CI minutes add up fast.
- On merge: cheapest signal, but the regression is already in main.
-->

---
layout: api-section
---

<iconoir-book class="font-size-6xl -mt-24" />

# Generating Documentation

Powered by `@playwright/test`

<!--
Now we'll dig into TestdocTest — the documentation generator. This is what turns a Playwright test into a step-by-step user guide.
-->

---

<h1 text-2xl!>Generating Documentation</h1>

<div class="code-lg">

````md magic-move
```ts
// Minimal setup
import { TestdocTest } from 'openedx-e2e-tests';
...
const testdoc = new TestdocTest(page, 'user-login');

await testdoc.initialize();
```

```ts {1, 5-13}
// Full constructor options
import { TestdocTest } from 'openedx-e2e-tests';
...
const testdoc = new TestdocTest(page, 'user-login', {
  title: 'How to Log In',
  overview: 'Step-by-step guide to logging into the Open edX platform.',
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

</div>

<!--
The constructor takes a page, a name for the output file, and a set of options that populate the header of your generated doc — title, overview, prerequisites, notes, related topics, and whether to number steps.
-->

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

<!--
And here's what that header looks like in the generated documentation — title, overview, and the supporting metadata laid out for the reader.
-->

---

<h1 text-2xl!>Generating Documentation</h1>

````md magic-move
```ts
import { TestdocTest } from 'openedx-e2e-tests';
...
// step() — screenshot with title and description
await testdoc.step({
  title: 'Navigate to the login page',
  description: 'Open the sign-in form from the home page.',
});
```

```ts {8-15}
import { TestdocTest } from 'openedx-e2e-tests';
...
await testdoc.step({
  title: 'Navigate to the login page',
  description: 'Open the sign-in form from the home page.',
});

// fill() — highlight input, fill value, screenshot
await testdoc.fill({
  selector: 'input[name="email"]',
  value: 'user@example.com',
  title: 'Enter your email address',
  elementOnly: true,  // crop screenshot to the element
  padding: 30,
});
```

```ts {17-22}
import { TestdocTest } from 'openedx-e2e-tests';
...
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

// click() — highlight element, click it, screenshot
await testdoc.click({
  selector: 'button[type="submit"]',
  title: 'Click Sign In',
  description: 'Submit your credentials.',
  elementOnly: true,
});
```
````

<!--
Three workhorse methods.
- step() takes a screenshot with a title and description — for navigation or context.
- fill() highlights the input, fills the value, and captures a screenshot. elementOnly crops the screenshot to just the input.
- click() highlights the element, clicks it, and screenshots it.
-->

---
layout: full
---

<pre class="aspect-video bg-gray">[SCREENSHOT] 
testdoc.step
testdoc.fill
testdoc.click
</pre>

<!--
And here's what those three methods produce in the generated guide — a numbered set of steps with cropped, annotated screenshots.
-->

---

<h1 text-2xl!>Generating Documentation</h1>

<div class="code-lg">

````md magic-move
```ts
import { TestdocTest } from 'openedx-e2e-tests';

// take a screenshot
await testdoc.screenshot({
  title: 'Dashboard loaded',
  description: "You're now logged in.",
});
```
```ts {8-9}
import { TestdocTest } from 'openedx-e2e-tests';

await testdoc.screenshot({
  title: 'Dashboard loaded',
  description: "You're now logged in.",
});

// add a note
await testdoc.note('Bookmark this page for quick access.');
```
```ts {10-13}
import { TestdocTest } from 'openedx-e2e-tests';

await testdoc.screenshot({
  title: 'Dashboard loaded',
  description: "You're now logged in.",
});

await testdoc.note('Bookmark this page for quick access.');

// generate document files
await testdoc.generateMarkdown();  // → documentation.md
await testdoc.generateRST();       // → documentation.rst
```
````

</div>

<!--
A few more helpers: screenshot() for arbitrary captures that aren't tied to an interaction, note() for callouts that show up as admonitions in the output, and generate() to emit the final file in either Markdown or RST.
-->

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

<!--
Same source test, two output formats. RST on the left for projects that publish with Sphinx, Markdown on the right for everyone else. The preview in the middle is identical.
-->

---
layout: api-section
---

<iconoir-accessibility class="font-size-6xl -mt-12" />

# A11y Tests

Powered by `@axe-core/playwright`

<!--
Next: accessibility. axe-core does the heavy lifting under the hood — we've layered on screenshots, reports, and a few ergonomic options that make it easier to adopt incrementally.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

```ts
import { checkA11y } from 'openedx-e2e-tests';

// checkA11y — returns results, never throws error
const results = await checkA11y(page);

console.log(`Violations: ${results.violations.length}`);
console.log(`Passes: ${results.passes.length}`);
```

</div>

<!--
The lowest-level primitive: checkA11y runs the scan and returns a results object. It never throws, so you can build custom assertions or just log the counts.
-->

---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
console.log
</pre>

<!--
And here's what the results look like at the console — a violations array and a passes array you can introspect.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

```ts
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  // assertA11y — throws error on violations
  await assertA11y(page, {}, testInfo);
});
```

</div>

<!--
For most use cases you want assertA11y. Same axe scan, but it throws if there are any violations — so the test fails and you get an actionable signal in CI.
-->

---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
assertA11y
</pre>

<!--
Side by side: checkA11y returns, assertA11y throws. Same scan underneath — pick the one that fits how you want failures to surface.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

````md magic-move
```ts
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');
  
  await assertA11y(page, {}, testInfo);
});
```

```ts {1,6-9}
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  // warnOnly — log instead of throw (good for onboarding)
  await assertA11y(page, {
    warnOnly: true,
  }, testInfo);
});
```
````

</div>

<!--
When you bolt accessibility onto an existing codebase, failing every test on day one isn't realistic. warnOnly logs violations instead of throwing — so you can adopt this gradually, fix violations over time, and flip the flag off when you're ready.
-->

---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
checkA11y
assertA11y
warnOnly
</pre>

<!--
With warnOnly, violations show up in the test output but the test still passes. Same data, softer enforcement.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

````md magic-move
```ts
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {
    warnOnly: true,
  }, testInfo);
});
```

```ts {1,6,9-10}
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  // + report — auto-generate HTML report after each run
  await assertA11y(page, {
    warnOnly: true,
    report: true,
    reportName: 'login-page',  // distinguish multiple checks in one test
  }, testInfo);
});
```
````

</div>

<!--
Pass report: true and you get a per-test HTML report with screenshots and remediation guidance. reportName lets you distinguish multiple A11y checks within a single test — for example, before and after a state change.
-->

---
layout: full
---
<pre class="aspect-video bg-gray">[SCREENSHOT]
report
</pre>

<!--
This is the report — violations grouped by rule, with screenshots and links to the axe documentation for each one. This is what you hand to a developer when you want them to fix something.
-->

---

<h1 text-2xl!>A11y Tests</h1>

<div class="code-lg">

````md magic-move
```ts
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  await assertA11y(page, {
    warnOnly: true,
    report: true,
    reportName: 'login-page',
  }, testInfo);
});
```

```ts {1,6,11-12}
import { assertA11y } from 'openedx-e2e-tests';

test('login page is accessible', async ({ page }, testInfo) => {
  await page.goto('/login');

  // + disabledRules + exclude — fine-tune what gets scanned
  await assertA11y(page, {
    warnOnly: true,
    report: true,
    reportName: 'login-page',
    disabledRules: ['color-contrast'],   // known false positive
    exclude: ['.third-party-widget'],    // out of our control
  }, testInfo);
});
```
````

</div>

<!--
Two escape hatches you'll reach for eventually. disabledRules silences a specific axe rule — useful when you've reviewed a violation and determined it's a false positive in your context. exclude removes a DOM subtree from the scan entirely — useful for third-party widgets you can't fix.
-->

---
layout: api-section
---

<iconoir-eye class="font-size-6xl -mt-12" />

# Visual Regression

Powered by `pixelmatch`

<!--
Last API: visual regression. Pixelmatch under the hood does the diffing — we wrap it with the Playwright integration, baseline management, and reporting.
-->

---

<h1 text-2xl!>Visual Regression</h1>

<div class="code-lg">

```ts
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // Basic setup — first run creates the baseline
  await vr.captureAndCompare({
    name: 'login-page'
  });
});
```

</div>

<!--
Create a VisualRegression instance, then captureAndCompare. The first run captures and stores a baseline; every subsequent run compares against that baseline and flags any differences.
-->

---
layout: full
---

<div class="vr-slideshow vr-overlay">
  <div class="vr-slide">
    <img src="/screenshots/visual-regression-basline.png" alt="Visual regression baseline screenshot" />
    <div class="vr-caption">Before</div>
  </div>
  <div class="vr-slide" v-click="1">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">After</div>
  </div>
  <div class="vr-slide" v-click="2">
    <img src="/screenshots/visual-regression-diff.png" alt="Visual regression diff screenshot" />
    <div class="vr-caption">Diff</div>
  </div>
  <div class="vr-slide" v-click="3">
    <img src="/screenshots/visual-regression-test-results.png" alt="Visual regression test results" />
    <div class="vr-caption">Report</div>
  </div>
</div>

<!--
And here's what a basic comparison looks like — baseline, current, diff.
-->


---

<h1 text-2xl!>Visual Regression</h1>

<div class="code-lg">

````md magic-move
```ts
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  await vr.captureAndCompare({
    name: 'login-page'
  });
});
```

```ts {7,10}
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // + fullPage — capture the entire scrollable page
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
  });
});
```
````

</div>

<!--
A few options to tune the comparison.
- fullPage captures the entire scrollable page, not just the viewport. 
-->

---
layout: full
---

<div class="vr-slideshow vr-overlay">
  <div class="vr-slide">
    <img src="/screenshots/visual-regression-basline.png" alt="Visual regression baseline screenshot" />
    <div class="vr-caption">Before</div>
  </div>
  <div class="vr-slide" v-click="1">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">After</div>
  </div>
  <div class="vr-slide" v-click="2">
    <img src="/screenshots/visual-regression-diff.png" alt="Visual regression diff screenshot" />
    <div class="vr-caption">Diff</div>
  </div>
  <div class="vr-slide" v-click="3">
    <img src="/screenshots/visual-regression-test-results.png" alt="Visual regression test results" />
    <div class="vr-caption">Report</div>
  </div>
</div>

<!--
And here's what a basic comparison looks like — baseline, current, diff.
-->

---

<h1 text-2xl!>Visual Regression</h1>

<div class="code-lg">

````md magic-move
```ts {7,10}
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // + fullPage — capture the entire scrollable page
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
  });
});
```

```ts {7,11}
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // + mask — exclude dynamic content from comparison
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
    mask: ['.timestamp', '[data-testid="user-greeting"]'],
  });
});
```
````

</div>

<!--
- mask hides dynamic content like timestamps or personalized greetings that would otherwise create false positives.
-->

---
layout: full
---

<div class="vr-slideshow vr-overlay">
  <div class="vr-slide">
    <img src="/screenshots/visual-regression-basline.png" alt="Visual regression baseline screenshot" />
    <div class="vr-caption">Before</div>
  </div>
  <div class="vr-slide" v-click="1">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">After</div>
  </div>
  <div class="vr-slide" v-click="2">
    <img src="/screenshots/visual-regression-diff.png" alt="Visual regression diff screenshot" />
    <div class="vr-caption">Diff</div>
  </div>
  <div class="vr-slide" v-click="3">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">Fixed</div>
  </div>
</div>

<!--
And here's what a basic comparison looks like — baseline, current, diff.
-->

---

<h1 text-2xl!>Visual Regression</h1>

<div class="code-lg">

````md magic-move
```ts {7,11}
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // + mask — exclude dynamic content from comparison
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
    mask: ['.timestamp', '[data-testid="user-greeting"]'],
  });
});
```

```ts {7,12}
import { VisualRegression } from 'openedx-e2e-tests';

test('login page looks right', async ({ page }, testInfo) => {
  const vr = new VisualRegression(page, testInfo);
  await page.goto('/login');

  // + threshold — tune sensitivity for font rendering 
  await vr.captureAndCompare({
    name: 'login-page',
    fullPage: true,
    mask: ['.timestamp', '[data-testid="user-greeting"]'],
    threshold: 0.03,  // 0.1 default · 0.05 strict · 0.15 lenient
  });
});
```
````

</div>

<!--
- threshold controls how sensitive pixelmatch is — useful for working around minor font rendering differences across machines or browsers.
-->

---
layout: two-cols-aside
---

::aside::
<h1 text-2xl!><code>threshold</code></h1>

<div class="text-base opacity-80 mt-4 space-y-3">

<p>Per-pixel color distance — <em>not</em> the % of pixels that differ.</p>

<p>A pixel is flagged when its distance exceeds the threshold.</p>

</div>

::default::

<div class="grid grid-cols-[auto_auto_auto_auto_1fr] gap-x-5 gap-y-4 text-lg items-center">

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#f7f7f7"></div>
<div><code>0.03</code></div>
<div class="opacity-70 text-base">anti-aliasing</div>

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#f2f2f2"></div>
<div><code>0.05</code></div>
<div class="text-base font-bold">strict</div>

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#e5e5e5"></div>
<div><code>0.10</code></div>
<div class="text-base font-bold">default</div>

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#cdcdcd"></div>
<div><code>0.19</code></div>
<div class="opacity-70 text-base">disabled state</div>

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#6e6e6e"></div>
<div><code>0.55</code></div>
<div class="opacity-70 text-base">major tone shift</div>

<div class="w-14 h-14 border-2 border-black/30" style="background:#ffffff"></div>
<div class="opacity-50">→</div>
<div class="w-14 h-14 border-2 border-black/30" style="background:#000000"></div>
<div><code>1.00</code></div>
<div class="text-base font-bold">flags nothing</div>

</div>

<!--
Threshold is the per-pixel color distance allowed before a pixel counts as different. Anti-aliasing sits around 0.03, so the default 0.1 quietly ignores it. A hover-shade shift is about 0.05 — strict mode flags it, default lets it through. By 0.5 you're only catching major color swaps. 1.0 effectively turns the check off.
-->

---
layout: full
---

<div class="vr-slideshow vr-overlay">
  <div class="vr-slide">
    <img src="/screenshots/visual-regression-basline.png" alt="Visual regression baseline screenshot" />
    <div class="vr-caption">Before</div>
  </div>
  <div class="vr-slide" v-click="1">
    <img src="/screenshots/visual-regression-changes.png" alt="Visual regression changed screenshot" />
    <div class="vr-caption">After</div>
  </div>
  <div class="vr-slide" v-click="2">
    <img src="/screenshots/visual-regression-diff.png" alt="Visual regression diff screenshot" />
    <div class="vr-caption">Diff</div>
  </div>
</div>

<!--
And here's what a basic comparison looks like — baseline, current, diff.
-->

---
layout: api-section
---

# Wrapping up

<!--
That's the whole API. Before I wrap up, a few honest gaps and a pitch to contribute.
-->

---

<h1>New Problems</h1>
What still needs to be solved.

<div class="grid grid-cols-1 grid-rows-1">

<img v-click="[1, 2]" src="/video/new-problems.gif" class="col-start-1 row-start-1 mx-auto mt-12" />

<div v-click="2" class="col-start-1 row-start-1 grid grid-cols-3 gap-8 mt-12">

<div class="flex flex-col items-center text-center">

<iconoir-database class="font-size-6xl mb-4" />

## Bootstrapping
How do we bootstrap test environments with data that our tests rely on?

</div>

<div v-click="3" class="flex flex-col items-center text-center">

<iconoir-erase class="font-size-6xl mb-4" />

## Cleanup
How do we cleanup after tests run?

</div>

<div v-click="4" class="flex flex-col items-center text-center">

<iconoir-data-transfer-both class="font-size-6xl mb-4" />

## Mocking
Can we run tests against mock data and database data?

</div>

</div>

</div>

<!--
A few things this doesn't solve yet. How do we seed the test environment with the data our tests rely on? How do we clean up reliably after a run? And can we exercise the same tests against both real backends and mocked data? Open questions — I'd love to hear how you're solving them.
-->

---

# How to contribute

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

<!--
The library is on npm — install it in any frontend repo or stand it up as its own test suite. File bugs and feature requests on GitHub, and PRs are welcome. There's a lot here that would benefit from community feedback.
-->

---
layout: api-section
---

# Thank You

Questions? Feedback? Find me after the talk.

<img src="/beyond-test-coverage-e2e-a11y-documentation-an_stewart-balderrama_1177129_feedback-code.png" class="w-64 mt-4 -mb-10vh" />

<!--
Thanks for listening. Scan the QR for the talk feedback form — your input shapes future versions of this talk. And find me afterward if you have questions or want to swap notes on testing strategy.
-->
