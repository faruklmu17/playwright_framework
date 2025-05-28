# Playwright React Testing Framework

A modular, production-ready Playwright + TypeScript framework for testing React applications (or any web apps). Built-in support for:

* **Page Object Model** (POM) for clean, maintainable test code
* **Data-driven testing** via JSON/CSV fixtures
* **Visual regression** with baseline snapshots and automatic diffs
* **Screenshots & videos** on failures for easier debugging
* **Environment configuration** using `.env` files
* **CLI & CI integration** with GitHub Actions and JUnit/HTML reports

---

## 🚀 Quickstart

1. **Clone & install**

   ```bash
   npm ci
   npx playwright install
   ```
2. **Configure**

   * Copy `.env.example` to `.env`
   * Set `BASE_URL=https://faruk-hasan.com/automation`
3. **Run tests**

   ```bash
   npx playwright test
   ```
4. **View reports**

   ```bash
   npx playwright show-report
   ```

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
├── reports/                # JUnit & HTML test reports
├── playwright.config.ts    # Test runner config & baseURL
├── .env.example            # Environment variable template
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
cases.forEach(c => {
  test(`login: ${c.email}`, async ({ page }) => { /* ... */ });
});
```

### Visual Regression

Capture golden snapshots and compare on every run:

```ts
await expect(page).toHaveScreenshot('baseline/signup-form.png');
```

### CI & Reporting

* GitHub Actions runs `npm ci` → `npx playwright test`
* Produces JUnit XML and HTML reports in `reports/`

---

## 🤝 Contributing

1. Fork this repo
2. Add new tests or pages under `src/`
3. Update fixtures in `fixtures/data`
4. Ensure tests pass locally and in CI
5. Submit a pull request with a clear description

---

> Happy testing! 🧪
