# Changes Summary — Crater Finance Web (Berani Fork)

> All changes compared to the original [bytefury/crater](https://github.com/bytefury/crater) upstream (`origin/master`).
> Breakdown by contributor, chronologically ordered.

---

## 1. Nikhil

| Date | Commit | Description |
|------|--------|-------------|
| 2021-12-27 | `b4147645` | Update `readme.md` |
| 2022-01-13 | `f5712aec` | Update `readme.md` |

**Files changed:** `readme.md`

---

## 2. dhruvbhattt

| Date | Commit | Description |
|------|--------|-------------|
| 2022-10-15 | `bbddd885` | Fixed report PDF issue |
| 2022-10-20 | `0c821b8a` | Removed telescope service provider file |
| 2023-08-25 | `f11437ce` | Added validation on requests |
| 2023-08-26 | `0dc89419` | Fixed minimum total issue |

**Files changed:**
- `app/Http/Requests/EstimatesRequest.php` — added validation
- `app/Http/Requests/InvoicesRequest.php` — added validation
- `app/Http/Requests/RecurringInvoiceRequest.php` — added validation
- `app/Http/Requests/SendEstimatesRequest.php` — validation tweaks
- `app/Http/Requests/SendInvoiceRequest.php` — validation tweaks
- `app/Http/Requests/SendPaymentRequest.php` — validation tweaks
- `resources/views/app/pdf/reports/*.blade.php` — report PDF fixes
- Removed telescope service provider file

---

## 3. radhika587

| Date | Commit | Description |
|------|--------|-------------|
| 2023-03-09 | `881f1a3b` | Fixed PDF URL issue |
| 2023-03-10 | `d6cc7564` | Fixed payment note issue |
| 2023-03-10 | `ba298cdb` | Fixed Logout issue |
| 2023-03-10 | `bdf8fa8b` | Fixed logout issue on payment & recurring-invoice view page |
| 2023-04-28 | `e1baac7e` | Upgraded Vite bundler |

**Files changed:**
- `app/Controllers/V1/Customer/EstimatePdfController.php` — PDF URL fix
- `app/Controllers/V1/Customer/InvoicePdfController.php` — PDF URL fix
- `resources/scripts/admin/views/payments/View.vue` — payment note fix
- `resources/scripts/admin/views/estimates/View.vue` — logout issue fix
- `resources/scripts/admin/views/invoices/View.vue` — logout issue fix
- `vite.config.ts` — upgraded Vite configuration
- `package.json` — Vite dependency bumps
- `resources/scripts/admin/components/estimate-invoice-common/CreateItemRow.vue` — tweaks
- `resources/scripts/admin/components/estimate-invoice-common/CreateItemRowTax.vue` — tweaks
- `resources/scripts/admin/components/estimate-invoice-common/CreateTotal.vue` — tweaks

---

## 4. yogesh-gohil

| Date | Commit | Description |
|------|--------|-------------|
| 2023-03-11 | `c1f2af51` | Added mail sender CRUD |
| 2023-03-11 | `b4aa254b` | Added mail-sender abilities |
| 2023-03-14 | `2bea727d` | Fixed migration and API changes |
| 2023-03-17 | `dea73bcd` | Refactored mail-sender |

**Files changed:**
- `app/Models/MailSender.php` — **new file** — Mail Sender Eloquent model
- `app/Controllers/V1/Admin/MailSender/MailSenderController.php` — **new file** — CRUD controller
- `app/Controllers/V1/Admin/MailSender/GetAllMailSendersController.php` — **new file** — listing endpoint
- `app/Http/Requests/MailSenderRequest.php` — **new file** — form request validation
- `app/Http/Requests/TestMailDriverRequest.php` — **new file** — test mail driver request
- `app/Http/Resources/MailSenderResource.php` — **new file** — API resource
- `app/Policies/MailSenderPolicy.php` — **new file** — authorization policy
- `app/Traits/MailTrait.php` — **new file** — reusable mail logic
- `resources/scripts/admin/stores/mail-sender.js` — **new file** — Pinia store
- `resources/scripts/admin/stub/mail-sender.js` — **new file** — ability stub
- `resources/scripts/admin/components/dropdowns/MailSenderIndexDropdown.vue` — **new file**
- `resources/scripts/admin/components/modal-components/MailSenderModal.vue` — **new file**
- `resources/scripts/admin/components/modal-components/MailSenderTestModal.vue` — **new file**
- `resources/scripts/admin/views/settings/mail-sender/Index.vue` — **new file**
- `resources/scripts/admin/views/settings/mail-sender/SmtpDriver.vue` — **new file**
- `resources/scripts/admin/views/settings/mail-sender/SesDriver.vue` — **new file**
- `resources/scripts/admin/views/settings/mail-sender/MailgunDriver.vue` — **new file**
- `database/migrations/2023_03_10_125202_create_mail_senders_table.php` — **new migration**
- `routes/api.php` — added mail-sender routes
- `config/abilities.php` — added mail-sender permissions
- `app/Providers/AuthServiceProvider.php` — registered MailSender policy
- `app/Controllers/V1/Admin/Settings/MailConfigurationController.php` — refactored
- Removed old mail-driver files: `BasicMailDriver.vue`, `MailgunMailDriver.vue`, `SesMailDriver.vue`, `SmtpMailDriver.vue`, `mail-driver.js` store

---

## 5. yashkanakiya

| Date | Commit | Description |
|------|--------|-------------|
| 2023-03-11 | `959aa257` | Added mail sender in settings UI |
| 2023-03-14 | `aede1f76` | Connected mail sender with API |
| 2023-03-16 | `aececb85` | Refactored mail sender |
| 2023-08-25 | `27660c6b` | Fixed initial tax-per-item issue |
| 2023-08-25 | `4fc67c74` | Removed commit in estimate storage |
| 2023-08-25 | `dbd75bbe` | Added changes in tax-per-item calculation |
| 2023-08-26 | `6d0edb4b` | Fixed table pagination filter issue |
| 2023-08-28 | `c14eb1b4` | Minor fix |
| 2023-08-28 | `9608ab62` | Removed compound interest and unused code |

**Files changed:**
- `resources/scripts/admin/views/settings/mail-sender/` — settings UI pages
- `resources/scripts/admin/admin-router.js` — added mail-sender route
- `resources/scripts/admin/stores/estimate.js` — tax calculation changes
- `resources/scripts/admin/stores/invoice.js` — tax calculation changes
- `resources/scripts/locales/en.json` — new translation keys for mail-sender
- `resources/scripts/admin/components/modal-components/SendEstimateModal.vue` — mail-sender integration
- `resources/scripts/admin/components/modal-components/SendInvoiceModal.vue` — mail-sender integration
- `resources/scripts/admin/components/modal-components/SendPaymentModal.vue` — mail-sender integration
- Various tax-per-item calculation fixes in backend models (`app/Models/Estimate.php`, `app/Models/Invoice.php`, `app/Models/Payment.php`)
- `app/Space/helpers.php` — helper tweaks for compound interest removal
- Removed compound interest logic

---

## 6. Fenton Martin (Berani maintainer)

| Date | Commit | Description |
|------|--------|-------------|
| 2024-02-20 | `d06d9882` | Updated `helpers.php` |
| 2024-02-20 | `c196d136` | Updated assets (logos, favicons, PDF preview images) |
| 2024-02-20 | `f7639265` | Updated colors (pace-loader, crater.scss theme variables) |
| 2024-02-20 | `e09912e0` | Merged `NickCrater-patch-1` branch |
| 2024-02-20 | `d30c6105` | Merged `origin/mail-sender` branch |
| 2024-02-20 | `ecde15d3` | Merged `origin/upgrade-vite` branch |
| 2024-02-20 | `ddadb9ca` | Merged `origin/tax-calculation-issue` branch |
| 2024-02-20 | `49c8a62a` | Merged `origin/report-pdf-issue` branch |
| 2024-02-20 | `4c9c3073` | Merged `origin/fix-pdf-url-issue` branch |
| 2024-02-20 | `7a536867` | Merged `origin/NickCrater-patch-2` branch |
| 2024-02-20 | `4ce42c05` | Merged `origin/fix-logout-issue` branch |
| 2024-02-20 | `85f4709f` | Merged `origin/fix-payment-note-issue` branch |
| 2025-03-27 | `95b5a994` | Merged PR #1 from `beranidigital/berani` (final integration) |

**Files changed (direct commits):**
- `resources/sass/components/pace-loader.scss` — color updates
- `resources/sass/crater.scss` — color variable changes (Berani branding)
- `resources/static/img/` — updated logo assets (crater-logo, favicons, PDF preview images)
- `public/build/img/` — removed old image assets, replaced with new ones
- `public/build/assets/` — rebuilt Vite assets with new branding
- `public/favicons/` — updated favicon set
- `app/Space/helpers.php` — changes in helper functions
- `readme.md` — updated with Berani branding

---

## 7. Ticlext-Altihaf

| Date | Commit | Description |
|------|--------|-------------|
| 2024-02-20 | `706ef2a0` | `feat: Remove module nav` |
| 2024-02-20 | `db1f0d90` | `fix: stuff` |
| 2024-02-20 | `cb7fda92` | `fix: stuff` |
| 2024-02-20 | `d8488485` | `fix: Stuff` |
| 2024-02-22 | `a7d4a200` | `fix: Real` |
| 2024-02-22 | `fa7e6859` | `fix: Real` |

**Files changed:**
- `resources/scripts/admin/admin-router.js` — removed module navigation entries
- `resources/scripts/admin/components/FeedbackAlert.vue` — **new file** — alert component
- `resources/scripts/admin/components/modal-components/TaxTypeModal.vue` — tax type modal changes
- `resources/views/app.blade.php` — layout changes
- `resources/scripts/components/base/BaseFileUploader.vue` — tweak
- `resources/scripts/components/base/BaseGlobalLoader.vue` — tweak
- `resources/scripts/components/base/BaseModal.vue` — tweak
- `resources/scripts/components/base/base-table/BaseTable.vue` — tweak
- Various CSS/sass files — theme polish
- Multiple `.vue` view files — module removal adjustments

---

## Summary: What Changed vs Original (bytefury/crater)

1. **Branding** — Replaced all Crater logos, favicons, colors with Berani branding
2. **Mail Sender feature** — Complete custom mail-sender system replacing the old mail-driver system; supports SMTP, SES, Mailgun drivers with per-company configuration
3. **Tax calculation fixes** — Tax-per-item calculation, minimum total validation, compound interest removed
4. **Bug fixes** — PDF URL issue, logout issue, payment note issue, report PDF issue, table pagination filter
5. **Vite upgrade** — Upgraded from old Vite version to newer one (affects all built assets)
6. **Module navigation removal** — Removed certain module navigation entries
7. **Infrastructure** — Removed telescope, validation improvements, hash IDs config, env example updates
8. **Readme updates** — Branded with Berani information
