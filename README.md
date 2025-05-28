# Playwright React Testing Framework

A modular, production-ready Playwright + TypeScript framework for testing React applications (or any web apps). Built-in support for:

- **Page Object Model** (POM) for clean, maintainable test code
- **Data-driven testing** via JSON/CSV fixtures
- **Visual regression** with baseline snapshots and automatic diffs
- **Screenshots & videos** on failures for easier debugging
- **Monocart HTML reporting** for interactive dashboards
- **Environment configuration** using `.env` files
- **CLI & CI integration** with GitHub Actions and JUnit/HTML/Monocart reports

---

## 📦 Installation & Setup

Dependencies are managed via `package.json`—no separate `requirements.txt` required. After cloning:

```bash
cd your-repo
npm ci                   # installs exact versions from package-lock.json
npx playwright install   # downloads browser binaries
````

You can also scaffold a one-line setup script in `package.json`:

```json
{
  "scripts": {
    "setup": "npm ci && npx playwright install"
  }
}
```

Then simply run:

```bash
npm run setup
```

---

## 🚀 Quickstart

1. **Configure**

   * Copy `.env.example` to `.env`
   * Set `BASE_URL=https://faruk-hasan.com/automation`

2. **Run tests**

   ```bash
   npx playwright test
   ```

3. **View reports**

   * Standard HTML: `npx playwright show-report`
   * Monocart report: open `reports/monocart/index.html`

---

## 📁 Project Structure

```text
├── fixtures/               # Test fixtures & data
│   └── data/               # JSON/CSV for data-driven tests
├── src/
│   ├── pages/              # Page Object Models
│   ├── utils/              # Helpers (data loaders, loggers)
│   └── tests/              # E2E & visual specs
├── visual-regression/      # Baseline & diff screenshots
├── reports/                # JUnit & HTML reports
│   └── monocart/           # Monocart HTML report files
├── .github/workflows/ci.yml# CI pipeline
├── playwright.config.ts    # Test runner config & baseURL
├── .env.example            # Environment variable template
├── tsconfig.json           # TS compiler options
├── .eslintrc.js/.prettierrc# Linting & formatting
├── package.json            # Dependencies & scripts
└── README.md               # (this file)
```

---

## ✨ Key Capabilities

### Page Object Model

Encapsulate selectors and actions in classes under `src/pages`:

```ts
class SignupPage {
  constructor(private page: Page) {}
  async goto() { await this.page.goto('/signup.html'); }
  async fillForm(email: string, pass: string) { /* ... */ }
  async submit() { /* ... */ }
}
```

### Data-Driven Testing

Load test cases from JSON/CSV in `fixtures/data` and loop through:

```ts
const cases = loadJSON('fixtures/data/login-data.json');
cases.forEach(c =>
  test(`login: ${c.email}`, async ({ page }) => {
    /* ... */
  })
);
```

### Visual Regression

Capture golden snapshots and compare on every run:

```ts
await expect(page).toHaveScreenshot('baseline/signup-form.png');
```

### Monocart HTML Reporting

Produce a rich, interactive test dashboard:

1. **Install**

   ```bash
   npm install -D monocart-reporter
   ```

2. **Configure** in `playwright.config.ts`:

   ```ts
   reporter: [
     ['list'],
     ['html', { outputFolder: 'reports/html', open: 'never' }],
     ['monocart-reporter', {
       name: 'Framework Test Report',
       outputFile: 'reports/monocart/index.html'
     }]
   ],
   outputDir: 'reports/monocart',
   ```

3. **Run tests** and open the report:

   ```bash
   npx playwright test
   open reports/monocart/index.html
   ```

### CI & Reporting

* GitHub Actions runs `npm run setup` → `npx playwright test`
* Generates JUnit XML, standard HTML, and Monocart HTML reports in `reports/`

---

## 🌐 GitHub Pages Deployment

Host your standard HTML report or Monocart dashboard on GitHub Pages with minimal setup:

1. **Enable Pages**

   * In your GitHub repo, go to **Settings → Pages**.
   * Under **Source**, select the branch (`main` or `gh-pages`) and folder (`/root` or `/docs`).

2. **Publish Reports**
   If you choose the `docs/` folder, add this step to your CI workflow (`.github/workflows/ci.yml`):

   ```yaml
   - name: Move HTML report
     run: |
       mkdir -p docs/reports
       cp -R reports/html/* docs/reports/
   ```

   Commits to `main` will automatically update `https://<username>.github.io/<repo>`.

3. **Automated Deployment**
   Alternatively, use `peaceiris/actions-gh-pages` to deploy from `reports/html` or `reports/monocart`:

   ```yaml
   - name: Deploy Reports to GitHub Pages
     uses: peaceiris/actions-gh-pages@v3
     with:
       publish_dir: reports/html
       publish_branch: gh-pages
   ```

Add this after your test job to push the latest report.

---

## 🤝 Contributing

1. Fork this repo
2. Add new tests or pages under `src/`
3. Update fixtures in `fixtures/data`
4. Ensure tests pass locally and in CI (`npm run setup` → `npx playwright test`)
5. Submit a pull request with a clear description

---

> Happy testing! 🧪
