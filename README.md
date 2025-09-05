# .github/workflows/security-audit.yml
name: 🛡️ Security Audit

on:
  schedule:
    - cron: '0 3 * * 1'
  push:
    branches: [main]

jobs:
  audit:
    runs-on: ubuntu-latest

    steps:
    - name: 📥 Checkout code
      uses: actions/checkout@v3

    - name: 🟢 Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'

    - name: 📦 Install dependencies
      run: npm ci

    - name: 🔍 Run npm audi
      run: npm audit --json > audit-report.json

    - name: 📤 Upload audit report
      uses: actions/upload-artifact@v3
      with:
        name: security-audit-report
        path: audit-report.json
