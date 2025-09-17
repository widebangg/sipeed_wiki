# คู่มือใช้งาน GitHub Pro สำหรับ Lichee Console 4A

---

## 1. ตรวจสอบสถานะ GitHub Pro

- ไปที่ [GitHub Billing Settings](https://github.com/settings/billing)
- หรือดูที่หน้าโปรไฟล์ของคุณ จะเห็น badge “PRO” แสดงอยู่

---

## 2. สร้าง/จัดการ Private Repository

- คลิกปุ่ม “New repository” บนหน้า [Repositories](https://github.com/new)
- ตั้งชื่อ repo และเลือก “Private”
- รายละเอียดอื่นๆ สามารถตั้งค่าได้ตามต้องการ
- **GitHub Pro** สร้าง private repo ได้ไม่จำกัด

---

## 3. ใช้งาน GitHub Actions

- เข้า repo ที่ต้องการ → แท็บ “Actions”
- เลือก template workflow ที่ต้องการ หรือสร้างไฟล์ใหม่ที่ `.github/workflows/ai_automation.yml`
- ตัวอย่าง Workflow สำหรับ Lichee Console 4A, Documentation, หรือ Python:

```yaml
name: Lint, Test, AI Analysis, Deploy Docs

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Lint Markdown
        uses: DavidAnson/markdownlint-cli2-action@v16
        with:
          config: .markdownlint.json
      - name: Lint Python (ถ้ามี)
        run: |
          pip install flake8
          flake8 .

  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - name: Run tests (Python example)
        run: |
          pip install -r requirements.txt
          pytest

  codeql:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: python
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3

  ai_review:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - name: Reviewdog with Copilot (AI code review)
        uses: reviewdog/action-copilot@v1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          reporter: github-pr-review
        continue-on-error: true

  autolabel:
    runs-on: ubuntu-latest
    needs: [test, codeql]
    steps:
      - uses: actions/checkout@v4
      - name: Auto-label PRs
        uses: actions/labeler@v5
        with:
          repo-token: ${{ secrets.GITHUB_TOKEN }}

  deploy_docs:
    runs-on: ubuntu-latest
    needs: [ai_review, autolabel]
    steps:
      - uses: actions/checkout@v4
      - name: Deploy Docs
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs
```

---

## 4. ใช้งาน GitHub Packages

- อ่านวิธีใช้งานแต่ละแบบ (npm, Docker, Maven ฯลฯ) ได้ที่ [GitHub Docs: About GitHub Packages](https://docs.github.com/en/packages)

---

## 5. ดู Security & Insights

- เข้า repo → แท็บ “Insights” หรือ “Security”
- ดูข้อมูลเช่น
    - **Dependency graph** (แผนผัง dependencies)
    - **Security alerts** (แจ้งเตือนช่องโหว่)
    - **Traffic** (จำนวนคนเข้า, clone, fork)
    - **Profile views** (จำนวนคนเข้าชมโปรไฟล์)

---

## 6. ขอความช่วยเหลือ/Support

- ไปที่ [GitHub Support](https://support.github.com/contact)
- บัญชี Pro ได้รับ Priority Support

---

## Tips พิเศษ

- **ถ้าเป็นนักศึกษา**: สมัคร [GitHub Student Developer Pack](https://education.github.com/pack) เพื่อรับสิทธิ์ใช้ฟรีและเครื่องมือพิเศษเพิ่ม
- **ต่อยอดกับ AI/Actions/Extensions**:
    - ใช้ Actions กับ AI workflow (เช่นรันโมเดล, deploy อัตโนมัติ)
    - ใช้ Copilot (ถ้าสมัครเพิ่ม)
    - สร้าง automation ของตัวเอง
- **เรียนรู้เพิ่มเติม**: ดูเอกสารและตัวอย่างที่ [GitHub Docs](https://docs.github.com/)

---

## ต้องการคำแนะนำเฉพาะด้าน?
- สร้าง CI/CD อัตโนมัติด้วย Actions
- การใช้งาน Packages
- ตั้งค่าความปลอดภัย
- หรือเรื่องอื่นๆ

**แจ้งได้เลย! ทีมซัพพอร์ตและ Community พร้อมช่วยเหลือ**
