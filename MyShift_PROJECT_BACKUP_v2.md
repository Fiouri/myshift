# MyShift v2.0 — Complete Project Backup

> Generated: 2026-04-06 (updated)
> Version: 2.0.0 (Build 1)
> Developer: Any WeCon (info@anywecon.com)
> Package: com.anywecon.myshift
> GitHub: https://github.com/Fiouri/myshift

---

## 1. PROJECT OVERVIEW

MyShift v2.0 is a **complete rewrite** of the MyShift app — a React Native (Expo) mobile application for Greek hourly workers (ωρομίσθιοι εργαζόμενοι) to track shifts, calculate earnings with full Greek labor law compliance, and understand their rights.

- **Target Audience:** Greek hourly workers (waiters, cleaners, warehouse workers, delivery, retail, etc.)
- **Platform:** Android-first, offline-first, built for budget phones
- **Developer:** Any WeCon
- **Contact:** info@anywecon.com
- **GitHub:** https://github.com/Fiouri/myshift
- **Phase:** Phase 1 — Foundation (complete)

---

## 2. PROJECT STRUCTURE

```
F:\Projects\MyShift\
├── App.tsx                            — Root component: ErrorBoundary, AuthListener, AdMob, ThemeProvider
├── index.ts                           — Entry point: registerRootComponent
├── app.json                           — Expo config: name, slug, plugins, package, icons
├── eas.json                           — EAS Build config: preview (APK), production (AAB), submit
├── package.json                       — Dependencies & scripts
├── package-lock.json                  — Lock file
├── tsconfig.json                      — TypeScript strict config with path aliases (@/*)
├── babel.config.js                    — Babel: babel-preset-expo + reanimated plugin
├── jest.config.js                     — Jest config: ts-jest, node env, moduleNameMapper
├── CLAUDE.md                          — Project conventions & coding standards
├── .gitignore                         — Git ignore rules
│
├── assets/
│   ├── adaptive-icon.png              — Android adaptive icon
│   ├── favicon.png                    — Web favicon
│   ├── icon.png                       — App icon
│   └── splash-icon.png               — Splash screen icon
│
├── docs/
│   ├── index.html                     — GitHub Pages landing (redirects to privacy)
│   └── privacy-policy.html           — Bilingual privacy policy (EL/EN, GDPR)
│
├── __tests__/
│   ├── calculations.test.ts          — 58 tests: payroll engine, bonuses, insurance
│   ├── dateUtils.test.ts             — 38 tests: date formatting, week ranges, Greek names
│   └── greekHolidays.test.ts         — 10 tests: Orthodox Easter, fixed/moving holidays
│
└── src/
    ├── components/
    │   ├── domain/
    │   │   ├── index.ts               — Barrel export for domain components
    │   │   ├── BreakChips.tsx         — Break duration selector chips (0, 15, 30, 45, 60 min)
    │   │   ├── DayTypeBadge.tsx       — Badge showing day type (regular/saturday/sunday/holiday)
    │   │   ├── EarningsBreakdown.tsx  — Step-by-step earnings calculation display
    │   │   ├── EarningsCard.tsx       — Gross/Insurance/Net earnings summary card
    │   │   ├── OvertimeProgressBar.tsx — Yearly 150h overtime limit progress bar
    │   │   └── ShiftPresetButton.tsx  — Morning/Afternoon/Night shift preset button
    │   └── ui/
    │       ├── index.ts               — Barrel export for UI components
    │       ├── Badge.tsx              — Generic badge component
    │       ├── Button.tsx             — Button: primary, outline, danger, ghost variants
    │       ├── Card.tsx               — Card container with optional elevation
    │       ├── Chip.tsx               — Selectable chip/tag component
    │       ├── Divider.tsx            — Horizontal divider line
    │       ├── EmptyState.tsx         — Empty state with icon, title, message, action
    │       ├── FABButton.tsx          — Floating Action Button (+)
    │       └── ProgressBar.tsx        — Configurable progress bar with thresholds
    │
    ├── constants/
    │   ├── appConfig.ts               — App version, AdMob IDs, URLs, defaults, limits
    │   └── lawConstants.ts            — ALL Greek labor law rates, premiums, bonuses, holidays
    │
    ├── database/
    │   ├── database.ts                — SQLite service: init, CRUD, queries, stats, backup
    │   └── migrations.ts              — Migration system: v1 initial schema
    │
    ├── hooks/
    │   └── useDatabase.ts             — useInitDatabase, useMonthData, useWeekData hooks
    │
    ├── i18n/
    │   ├── setup.ts                   — i18next config: el/en, fallback el
    │   ├── el.json                    — Greek translations (~448 lines)
    │   └── en.json                    — English translations (~448 lines)
    │
    ├── lib/
    │   └── supabase.ts                — Supabase client with AsyncStorage auth
    │
    ├── navigation/
    │   ├── AppNavigator.tsx           — Stack + Bottom Tab navigator (React Navigation v7)
    │   └── types.ts                   — Navigation param types + global declaration
    │
    ├── screens/
    │   ├── index.ts                   — Barrel export for all 14 screens
    │   ├── HomeScreen.tsx             — Dashboard: greeting, monthly/weekly summary, overtime, profile
    │   ├── CalendarScreen.tsx         — Calendar grid with entry indicators, holiday colors
    │   ├── DailyEntryScreen.tsx       — Shift entry form: presets, time, break, day type, live calc
    │   ├── ReportsScreen.tsx          — Monthly report: stats, hours breakdown bars, export
    │   ├── YearlyReportScreen.tsx     — Yearly report: 12-month table, totals, overtime, PDF export
    │   ├── OnboardingScreen.tsx       — 3-slide intro + profile setup (rate, date, schedule, employer)
    │   ├── CalculatorScreen.tsx       — Quick what-if calculator (rate, time, day type, schedule)
    │   ├── BonusesScreen.tsx          — Christmas/Easter/Vacation bonus calculator with step breakdown
    │   ├── LegalInfoScreen.tsx        — FAQ accordion with Greek labor law info + SEPE contact
    │   ├── SettingsScreen.tsx         — Profile, appearance, break default, notifications, clear data
    │   ├── MoreScreen.tsx             — Menu: Calculator, Bonuses, Legal, Settings, Backup, About
    │   ├── AuthScreen.tsx             — Supabase auth: sign in, sign up, forgot password
    │   ├── CloudBackupScreen.tsx      — Create/list/restore/delete cloud backups
    │   └── AboutScreen.tsx            — Version, developer, rate app, privacy policy, contact
    │
    ├── services/
    │   ├── backupService.ts           — Supabase cloud backup: create, list, restore, delete
    │   ├── exportService.ts           — PDF + CSV export via expo-print + expo-sharing
    │   └── notificationService.ts     — Daily reminder + overtime alert via expo-notifications
    │
    ├── store/
    │   ├── index.ts                   — Barrel export for all 5 stores
    │   ├── entriesStore.ts            — Zustand: month/week entries, stats, overtime (single source of truth)
    │   ├── profileStore.ts            — Zustand: worker profile from SQLite
    │   ├── settingsStore.ts           — Zustand + persist: theme, language, break, notifications, onboarding
    │   ├── authStore.ts               — Zustand: Supabase auth session state
    │   └── premiumStore.ts            — Zustand + persist: premium/freemium state
    │
    ├── theme/
    │   ├── index.ts                   — Barrel export for theme system
    │   ├── ThemeContext.tsx            — ThemeProvider + useTheme hook (light/dark/system)
    │   ├── colors.ts                  — Complete palette (50-900 shades), light + dark themes, WCAG verified
    │   ├── typography.ts              — 12 text styles: h1-h3, body, small, caption, button, amounts
    │   └── spacing.ts                 — 8px base grid, border radii, shadows, MIN_TOUCH_TARGET=48px
    │
    ├── types/
    │   └── index.ts                   — All TypeScript interfaces: 20+ types
    │
    └── utils/
        ├── calculations.ts            — Core payroll engine: hours, earnings, insurance, bonuses
        ├── dateUtils.ts               — Greek date formatting, week/month ranges, ISO helpers
        ├── formatters.ts              — Currency (€), hours (ω), percentage, Greek locale number
        └── greekHolidays.ts           — Orthodox Easter (Meeus), 14 public holidays, holiday lookup
```

**Total: 72 files | ~12,834 lines of code** (excluding node_modules, .git, package-lock.json)

---

## 3. TECH STACK

| Category | Technology | Version |
|----------|-----------|---------|
| Framework | React Native | 0.81.5 |
| Platform | Expo SDK | 54.0.33 |
| Language | TypeScript | ~5.9.2 |
| React | React | 19.1.0 |
| State | Zustand | ^5.0.12 |
| Persistence (local) | expo-sqlite | ~16.0.10 |
| Persistence (async) | @react-native-async-storage/async-storage | 2.2.0 |
| Navigation | @react-navigation/native | ^7.2.2 |
| Navigation (tabs) | @react-navigation/bottom-tabs | ^7.15.9 |
| Navigation (stack) | @react-navigation/native-stack | ^7.14.10 |
| i18n | i18next + react-i18next | ^26.0.3 / ^17.0.2 |
| Backend | @supabase/supabase-js | ^2.101.1 |
| Ads | react-native-google-mobile-ads | ^16.3.1 |
| Date utils | date-fns | ^4.1.0 |
| Animations | react-native-reanimated | ~4.1.1 |
| Gestures | react-native-gesture-handler | ~2.28.0 |
| Icons | @expo/vector-icons (Ionicons) | ^15.1.1 |
| Haptics | expo-haptics | ~15.0.8 |
| Notifications | expo-notifications | ~0.32.16 |
| Print/PDF | expo-print | ~15.0.8 |
| Sharing | expo-sharing | ~14.0.8 |
| File System | expo-file-system | ~19.0.21 |
| Localization | expo-localization | ~17.0.8 |
| Device info | expo-device | ~8.0.10 |
| Fonts | expo-font | ~14.0.11 |
| Safe Area | react-native-safe-area-context | ~5.6.0 |
| Screens | react-native-screens | ~4.16.0 |
| Status Bar | expo-status-bar | ~3.0.9 |
| Testing | Jest + ts-jest | ^29.7.0 / ^29.4.9 |
| Build | Babel (babel-preset-expo) | ^54.0.10 |

---

## 4. CREDENTIALS

### Supabase
- **URL:** `https://ikxobpgrnqdpxaawkmgk.supabase.co`
- **Anon Key:** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImlreG9icGdybnFkcHhhYXdrbWdrIiwicm9sZSI6ImFub24iLCJpYXQiOjE3NzAwNTY5ODIsImV4cCI6MjA4NTYzMjk4Mn0.bjD8mLvfxg0KeOUwoWIHQUl8Wu8AhsKEahjEy3N1XcE`
- **Auth Storage:** AsyncStorage with auto-refresh + persistent sessions

### AdMob (Android)
- **App ID:** `ca-app-pub-6290882379191140~2054915018`
- **Ad Unit (App Open):** `ca-app-pub-6290882379191140/8540723893`
- **Safe import:** Wrapped in try/catch; never crashes if module unavailable
- **Dev mode:** Ads disabled in `__DEV__`

### EAS / Expo
- **Owner:** fiouri
- **Slug:** GreekPayrollApp (from v1, backward compatible)
- **Project ID:** `e18bd7bc-0bc6-4c92-9fbb-67a4f0eb953a`
- **Package:** `com.anywecon.myshift`

### Privacy Policy
- **URL:** `https://fiouri.github.io/myshift/privacy-policy.html` *(LIVE)*
- **Contact Email:** `info@anywecon.com`
- **Play Store:** `https://play.google.com/store/apps/details?id=com.anywecon.myshift`

---

## 5. ARCHITECTURE DECISIONS

### Why React Native + Expo (SDK 54)
- Proven in v1 (SDK 54, React 19, RN 0.81)
- Excellent TypeScript support with strict mode
- EAS Build for Play Store deployment
- expo-sqlite for offline-first local storage
- Large ecosystem (notifications, file system, sharing, print)
- New Architecture enabled (`newArchEnabled: true`)

### Why SQLite (expo-sqlite) over AsyncStorage/MMKV
- Native performance on Android
- SQL queries for complex aggregations (monthly stats, yearly overtime)
- Indexed queries for fast calendar lookups (`idx_entries_date`, `idx_entries_year_month`)
- No network dependency — true offline-first
- Transaction support for bulk imports/clear
- Migration system for schema evolution

### Why Zustand over Redux/Context
- Minimal boilerplate, TypeScript-native
- Persist middleware for AsyncStorage (settings, premium)
- No provider nesting required
- Easy to test (plain functions)
- 5 focused stores instead of one monolithic store

### Why entriesStore as Single Source of Truth
- v1 had separate overtimeStore that caused data drift
- ALL entry data (including overtime hours) lives in entriesStore
- Database is the persistent store; Zustand is the in-memory cache

### Why react-i18next
- Bilingual support (Greek + English)
- All user-facing strings externalized
- JSON translation files with dot-notation keys
- Zero hardcoded strings in components

### Why React Navigation v7
- Industry standard for React Native navigation
- Bottom tabs (Home, Calendar, Reports, More) + Native Stack
- Deep linking support for future features
- Type-safe navigation params

---

## 6. COMPLETED FEATURES

### Core
- [x] **Onboarding** — 3-slide intro + profile setup (rate with gross/net toggle, hire date, schedule type, employer)
- [x] **Daily Entry** — Full shift entry form with time inputs, shift presets (morning/afternoon/night), break duration chips, day type override, notes
- [x] **Live Calculation** — Real-time earnings preview as user fills form
- [x] **Calendar View** — Monthly grid with entry indicators, holiday coloring, net pay per day
- [x] **Monthly Reports** — Stats summary, hours breakdown bar chart, gross/insurance/net totals
- [x] **Yearly Reports** — 12-month breakdown table, yearly totals, overtime progress

### Calculations (100% Greek Labor Law Compliant)
- [x] **Hours Breakdown** — Regular (≤8h), Overwork (9th hour), Overtime (10th+)
- [x] **Night Hours** — 22:00-06:00 with midnight crossing + pro-rata break distribution (break deducted proportionally from night/regular hours)
- [x] **Premiums** — Cumulative stacking: night +25%, Sunday/holiday +75%, Saturday fixed +30% or rotating +40%
- [x] **Overwork Premium** — 9th hour: +20% (Ν.4808/2021)
- [x] **Overtime Premium** — 10th+ hour: +40% (≤150h/year) or +60% (>150h/year)
- [x] **Insurance** — Regular income: 13.37%, Overtime income: 6.37%, Premiums: 0% exempt (Ν.5184/2025 Art.35)
- [x] **Christmas Bonus** — Max 25 daily wages, period May 1 - Dec 31
- [x] **Easter Bonus** — Max 15 daily wages, period Jan 1 - Apr 30
- [x] **Vacation Allowance** — Max 13 daily wages, full year accrual
- [x] **Overtime Progress** — Yearly 150h limit with visual progress bar

### Gross/Net Toggle (Μικτά/Καθαρά)
- [x] **Onboarding** — User can enter rate as gross or net
- [x] **Calculator** — Rate mode toggle with live conversion
- [x] **Settings** — Profile rate mode with equivalent display
- [x] **Conversion functions** — `grossToNet()` and `netToGross()` in calculations.ts
- [x] **DB always stores GROSS** — Net entered → converted to gross before storage
- [x] **Equivalent display** — Shows the other mode's amount (e.g., "≈ €4.57 net") on all rate inputs

### UI/UX
- [x] **Dark Mode** — Full light/dark/system theme with WCAG AA contrast ratios
- [x] **Greek Locale** — Month names, day names (nominative + genitive), date formatting
- [x] **i18n** — Greek (default) + English, ~448 translation lines per language
- [x] **Accessibility** — All interactive elements have labels, roles, minimum 48x48 touch targets
- [x] **Error Boundary** — Top-level crash recovery with friendly message
- [x] **Pull to Refresh** — HomeScreen data reload
- [x] **Unsaved Changes Warning** — DailyEntryScreen warns before leaving with changes
- [x] **FAB Button** — Floating action button for quick entry creation
- [x] **Empty States** — Friendly empty states with icons and action buttons
- [x] **Monospace Amounts** — Currency displayed in monospace font for trust signal

### Data & Sync
- [x] **SQLite Database** — Offline-first with migration system
- [x] **Cloud Backup** — Create/list/restore/delete via Supabase
- [x] **Auth** — Supabase email auth: sign in, sign up, forgot password
- [x] **Export PDF** — Monthly + yearly reports as styled HTML→PDF
- [x] **Export CSV** — Monthly entries as CSV with UTF-8 BOM for Excel Greek support

### Notifications
- [x] **Daily Reminder** — Configurable daily shift reminder (default 20:00)
- [x] **Overtime Alert** — Warning at 80% of yearly 150h limit
- [x] **Permission Handling** — Request + Android notification channel

### Settings
- [x] **Profile Management** — Edit hourly rate, hire date, schedule type, employer
- [x] **Theme Selection** — Light / Dark / System
- [x] **Language Switch** — Ελληνικά / English
- [x] **Default Break** — Configurable default break duration
- [x] **Clear All Data** — Full wipe with confirmation

### Legal
- [x] **Legal Info FAQ** — Expandable accordion with labor law questions/answers
- [x] **SEPE Contact** — Labor inspectorate phone number (1555)
- [x] **Disclaimer** — Legal disclaimer on all calculation screens
- [x] **Privacy Policy** — Bilingual HTML page with GDPR compliance

### Monetization
- [x] **AdMob App Open Ad** — Shows on production launch (safe import, never crashes)
- [x] **Premium Store** — Zustand + persist for freemium flag (ready for RevenueCat)

---

## 7. PENDING FEATURES

- [ ] **Play Store Submission** — First production AAB build + store listing
- [ ] **Multi-Employer Support** — Track shifts for multiple employers
- [ ] **RevenueCat Integration** — In-app purchases for premium features
- [ ] **Date Picker** — Native date/time picker instead of text input
- [ ] **PDF Branding** — Custom logo in exported PDF reports
- [ ] **Widget** — Android home screen widget for quick entry
- [ ] **Backup Auto-Sync** — Automatic cloud backup on entry save
- [ ] **Push Notifications** — Remote push via Expo Push API
- [ ] **Payslip Scanner** — OCR for paper payslip verification
- [ ] **Tax Calculator** — Annual tax estimation based on total earnings

---

## 8. DATABASE SCHEMA

### SQLite Tables (expo-sqlite `myshift.db`)

#### `_migrations`
| Column | Type | Notes |
|--------|------|-------|
| version | INTEGER PRIMARY KEY | Migration version number |
| name | TEXT NOT NULL | Migration name |
| applied_at | TEXT | datetime('now') |

#### `worker_profile`
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PRIMARY KEY | Always 1 (single profile) |
| hourly_rate | REAL NOT NULL | CHECK > 0, always stored as GROSS |
| hire_date | TEXT NOT NULL | ISO YYYY-MM-DD |
| schedule_type | TEXT NOT NULL | 'fixed' or 'rotating' (default: 'rotating') |
| employer_name | TEXT | Default '' |
| hours_for_daily_wage | INTEGER | Default 8, CHECK 1-12 |
| created_at | TEXT | datetime('now') |
| updated_at | TEXT | datetime('now') |

#### `daily_entries`
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | |
| date | TEXT NOT NULL UNIQUE | ISO YYYY-MM-DD (one entry per day) |
| start_time | TEXT NOT NULL | HH:MM |
| end_time | TEXT NOT NULL | HH:MM |
| break_minutes | INTEGER | Default 0, CHECK ≥ 0 |
| shift_preset | TEXT | 'morning', 'afternoon', 'night', 'custom' |
| day_type | TEXT NOT NULL | 'regular', 'saturday', 'sunday', 'holiday' |
| holiday_name | TEXT | i18n key or null |
| total_hours | REAL NOT NULL | CHECK ≥ 0 |
| regular_hours | REAL | Default 0 |
| overwork_hours | REAL | Default 0 |
| overtime_hours | REAL | Default 0 |
| night_hours | REAL | Default 0 |
| saturday_hours | REAL | Default 0 |
| sunday_hours | REAL | Default 0 |
| gross_pay | REAL NOT NULL | |
| insurance | REAL NOT NULL | |
| net_pay | REAL NOT NULL | |
| notes | TEXT | Default '' |
| created_at | TEXT | datetime('now') |
| updated_at | TEXT | datetime('now') |

**Indexes:**
- `idx_entries_date ON daily_entries(date)`
- `idx_entries_year_month ON daily_entries(substr(date,1,7))`

#### `custom_presets`
| Column | Type | Notes |
|--------|------|-------|
| id | INTEGER PRIMARY KEY AUTOINCREMENT | |
| name | TEXT NOT NULL | |
| start_time | TEXT NOT NULL | HH:MM |
| end_time | TEXT NOT NULL | HH:MM |
| sort_order | INTEGER | Default 0 |

#### `settings`
| Column | Type | Notes |
|--------|------|-------|
| key | TEXT PRIMARY KEY | |
| value | TEXT NOT NULL | |

Default settings: theme='system', language='el', defaultBreakMinutes='0', notificationsEnabled='1', overtimeNotificationsEnabled='1', onboardingCompleted='0'

### Supabase Tables

#### `backups`
| Column | Type | Notes |
|--------|------|-------|
| id | uuid | Primary key |
| user_id | uuid | Foreign key to auth.users |
| backup_data | jsonb | Full backup: profile + entries + settings |
| device_name | text | nullable |
| entries_count | integer | |
| created_at | timestamptz | |

---

## 9. CALCULATION FORMULAS

### Hours Breakdown
```
totalHours = (endTime - startTime - breakMinutes) / 60
regularHours = min(totalHours, 8)
overworkHours = min(max(totalHours - 8, 0), 1)    // 9th hour only
overtimeHours = max(totalHours - 9, 0)             // 10th+ hour
nightHours = overlap with 22:00-06:00 window (pro-rata break deduction)
saturdayHours = totalHours if dayType='saturday', else 0
sundayHours = totalHours if dayType='sunday' or 'holiday', else 0
```

### Earnings Breakdown
```
basePay = regularHours × hourlyRate
overworkPay = overworkHours × hourlyRate × 1.20          // Ν.4808/2021
overtimePay (≤150h/year) = hours × hourlyRate × 1.40     // Ν.4808/2021
overtimePay (>150h/year) = hours × hourlyRate × 1.60     // Ν.4808/2021
nightPremium = nightHours × hourlyRate × 0.25             // ΚΥΑ 18310/1946
saturdayPremium (fixed) = satHours × hourlyRate × 0.30    // Ν.3846/2010
saturdayPremium (rotating) = satHours × hourlyRate × 0.40 // Ν.5053/2023
sundayHolidayPremium = sunHours × hourlyRate × 0.75       // ΚΥΑ 8900/1946

grossPay = basePay + overworkPay + overtimePay + nightPremium + saturdayPremium + sundayHolidayPremium
```

**Premiums are CUMULATIVE** — night + sunday stack!

### Insurance (Ν.5184/2025 Art.35)
```
Regular income (basePay + overworkPay): 13.37%
  - Pension: 6.67%
  - Health: 2.15%
  - Supplementary: 3.25%
  - Unemployment: 1.20%
  - Welfare: 0.10%

Overtime income: 6.37%

Premium income (night, saturday, sunday/holiday): 0% EXEMPT

netPay = grossPay - regularInsurance - overtimeInsurance
```

### Bonuses (hourly workers)
```
dailyWage = hourlyRate × hoursForDailyWage (default 8)

Christmas Bonus (Ν.1082/1980, Ν.4554/2018):
  Period: May 1 - Dec 31
  dailyWagesEarned = min(workingDays / 13 × 2, 25)
  grossAmount = dailyWagesEarned × dailyWage
  insurance = grossAmount × 13.37%
  netAmount = grossAmount - insurance

Easter Bonus (Ν.1082/1980, Ν.4554/2018):
  Period: Jan 1 - Apr 30
  dailyWagesEarned = min(workingDays / 8 × 1, 15)
  grossAmount = dailyWagesEarned × dailyWage
  insurance = grossAmount × 13.37%

Vacation Allowance (Ν.539/1945, Art.3):
  Period: Jan 1 - Dec 31
  dailyWagesEarned = min(workingDays / 8 × 0.5, 13)
  grossAmount = dailyWagesEarned × dailyWage
  insurance = grossAmount × 13.37%
```

### Gross/Net Conversion
```
grossToNet(gross) = gross × (1 - 0.1337)
netToGross(net) = net / (1 - 0.1337)
```

---

## 10. ΑΡΙΘΜΗΤΙΚΗ ΕΠΑΛΗΘΕΥΣΗ

### Scenario A: Regular 8h day (Mon-Fri)
```
Rate: €5.28/h | Hours: 08:00-16:00 | Break: 0 | Day: regular
Regular: 8h | Overwork: 0 | Overtime: 0 | Night: 0
Base: 8 × €5.28 = €42.24
Insurance: €42.24 × 13.37% = €5.65
Net: €42.24 - €5.65 = €36.59
✅ PASS
```

### Scenario B: 10h day with overtime
```
Rate: €5.28/h | Hours: 08:00-18:00 | Break: 0 | Day: regular
Regular: 8h | Overwork: 1h | Overtime: 1h
Base: 8 × €5.28 = €42.24
Overwork: 1 × €5.28 × 1.20 = €6.34
Overtime: 1 × €5.28 × 1.40 = €7.39
Gross: €42.24 + €6.34 + €7.39 = €55.97
Regular insurance: (€42.24 + €6.34) × 13.37% = €6.50
Overtime insurance: €7.39 × 6.37% = €0.47
Net: €55.97 - €6.50 - €0.47 = €49.00
✅ PASS
```

### Scenario C: Night shift (22:00-06:00)
```
Rate: €5.28/h | Hours: 22:00-06:00 | Break: 0 | Day: regular
Regular: 8h | Night: 8h
Base: 8 × €5.28 = €42.24
Night premium: 8 × €5.28 × 0.25 = €10.56
Gross: €42.24 + €10.56 = €52.80
Insurance: €42.24 × 13.37% = €5.65 (premiums exempt)
Net: €52.80 - €5.65 = €47.15
✅ PASS
```

### Scenario D: Sunday work
```
Rate: €5.28/h | Hours: 08:00-16:00 | Break: 0 | Day: sunday
Regular: 8h | Sunday: 8h
Base: 8 × €5.28 = €42.24
Sunday premium: 8 × €5.28 × 0.75 = €31.68
Gross: €42.24 + €31.68 = €73.92
Insurance: €42.24 × 13.37% = €5.65 (premiums exempt)
Net: €73.92 - €5.65 = €68.27
✅ PASS
```

### Scenario E: Saturday (rotating schedule)
```
Rate: €5.28/h | Hours: 08:00-16:00 | Day: saturday | Schedule: rotating
Base: 8 × €5.28 = €42.24
Saturday premium: 8 × €5.28 × 0.40 = €16.90
Gross: €42.24 + €16.90 = €59.14
Insurance: €42.24 × 13.37% = €5.65
Net: €59.14 - €5.65 = €53.49
✅ PASS
```

### Scenario F: Saturday (fixed schedule)
```
Rate: €5.28/h | Hours: 08:00-16:00 | Day: saturday | Schedule: fixed
Saturday premium: 8 × €5.28 × 0.30 = €12.67
Gross: €42.24 + €12.67 = €54.91
Insurance: €42.24 × 13.37% = €5.65
Net: €54.91 - €5.65 = €49.26
✅ PASS
```

### Scenario G: Night + Sunday (cumulative)
```
Rate: €5.28/h | Hours: 22:00-06:00 | Day: sunday
Night premium: 8 × €5.28 × 0.25 = €10.56
Sunday premium: 8 × €5.28 × 0.75 = €31.68
Gross: €42.24 + €10.56 + €31.68 = €84.48
Insurance: €42.24 × 13.37% = €5.65 (premiums exempt)
Net: €84.48 - €5.65 = €78.83
✅ PASS
```

### Scenario H: Holiday work
```
Rate: €5.28/h | Hours: 08:00-16:00 | Day: holiday
Holiday premium: 8 × €5.28 × 0.75 = €31.68
Gross: €42.24 + €31.68 = €73.92
Insurance: €42.24 × 13.37% = €5.65
Net: €73.92 - €5.65 = €68.27
✅ PASS
```

### Scenario I: Christmas Bonus (full year)
```
Rate: €5.28/h | Hours/day: 8 | Hire: 2025-01-01 | Year: 2026
Period: May 1 - Dec 31 (full)
Working days: ~209 (Mon-Sat)
Daily wage: €5.28 × 8 = €42.24
Wages earned: min(209/13 × 2, 25) = min(32.15, 25) = 25
Gross: 25 × €42.24 = €1,056.00
Insurance: €1,056.00 × 13.37% = €141.19
Net: €1,056.00 - €141.19 = €914.81
✅ PASS
```

---

## 11. ΝΟΜΟΘΕΣΙΑ AUDIT

### A. Κατώτατος Μισθός
- **€880/μήνα** | **€35.20/ημέρα** | **€5.28/ώρα**
- ΦΕΚ Β' 1476/27-3-2025, ΚΥΑ 8233/2025
- Ισχύει από 01/04/2025
- ✅ Σωστά στο `lawConstants.ts`

### B. Ωράριο Εργασίας
- Νόμιμο ημερήσιο: 8 ώρες (Ν.4808/2021)
- Υπερεργασία: 9η ώρα (+20%)
- Υπερωρία: 10η+ ώρα (+40%, ή +60% μετά τις 150ω/έτος)
- ✅ Σωστά στους υπολογισμούς

### C. Νυχτερινά
- Περίοδος: 22:00-06:00 (ΚΥΑ 18310/1946)
- Προσαύξηση: +25%
- ✅ Σωστός υπολογισμός με midnight crossing

### D. Κυριακές / Αργίες
- Προσαύξηση: +75% (ΚΥΑ 8900/1946)
- ✅ Σωστά

### E. Σάββατο
- Σταθερό ωράριο: +30% (Ν.3846/2010)
- Εναλλασσόμενο: +40% (Ν.5053/2023)
- ✅ Σωστά, ο χρήστης επιλέγει τύπο ωραρίου

### F. Σωρευτικές Προσαυξήσεις
- Νυχτερινά + Κυριακή stack
- ✅ Σωστά

### G. Ασφαλιστικές Εισφορές (Εργαζόμενος)
- Κανονικό εισόδημα: 13.37% (Σύνταξη 6.67% + Υγεία 2.15% + Επικουρική 3.25% + Ανεργία 1.20% + Πρόνοια 0.10%)
- Υπερωριακό εισόδημα: 6.37%
- Προσαυξήσεις: 0% εξαιρούνται (Ν.5184/2025 Άρ.35)
- ✅ Σωστά

### H. Δώρα
- Χριστουγέννων: max 25 ημερομίσθια (Μάι 1 - Δεκ 31)
- Πάσχα: max 15 ημερομίσθια (Ιαν 1 - Απρ 30)
- Αδείας: max 13 ημερομίσθια
- Ασφάλιση δώρων: 13.37%
- ✅ Σωστά (Ν.1082/1980, Ν.4554/2018)

### I. Αργίες (14 συνολικά)
- 8 σταθερές: Πρωτοχρονιά, Θεοφάνεια, 25η Μαρτίου, Πρωτομαγιά, Κοίμηση Θεοτόκου, 28η Οκτωβρίου, Χριστούγεννα, 26 Δεκεμβρίου
- 6 κινητές (Ορθόδοξο Πάσχα): Καθαρά Δευτέρα, Μεγάλη Παρασκευή, Μεγάλο Σάββατο, Κυριακή Πάσχα, Δευτέρα Πάσχα, Αγίου Πνεύματος
- ✅ Σωστά (Meeus algorithm για Ορθόδοξο Πάσχα)

### J. Εκτός Έδρας
- Rates defined: 100% (κανένα), 80% (στέγη), 50% (σίτιση), 25% (στέγη + σίτιση)
- ΚΥΑ 21091/1946
- ✅ Defined στο `lawConstants.ts` (δεν εφαρμόζεται ακόμα στις οθόνες)

### K. Ετήσια Άδεια
- Πίνακας ημερών αδείας κατά έτη προϋπηρεσίας
- 5ήμερο και 6ήμερο εβδομάδα (Ν.539/1945)
- ✅ Defined στο `lawConstants.ts`

### L. ΣΕΠΕ
- Τηλέφωνο: 1555
- ✅ Εμφανίζεται στο LegalInfoScreen

### M. Disclaimer
- ✅ Εμφανίζεται σε: Onboarding, Bonuses, LegalInfo, About
- Κείμενο: "Τα ποσά είναι εκτιμήσεις. Συμβουλευτείτε λογιστή..."

---

## 12. APP SCREENS

| # | Screen | Lines | Description |
|---|--------|-------|-------------|
| 1 | HomeScreen | 319 | Dashboard: greeting, monthly summary, weekly stats, overtime progress, profile card, FAB |
| 2 | CalendarScreen | 484 | Monthly calendar grid with navigation, entry indicators, holidays, monthly footer |
| 3 | DailyEntryScreen | 673 | Full shift entry/edit form: presets, time, break, day type, notes, live calculation, save/delete |
| 4 | ReportsScreen | 395 | Monthly report: days/hours/earnings summary, hours breakdown bar chart, PDF/CSV export |
| 5 | YearlyReportScreen | 454 | Yearly report: 12-month table, totals, overtime progress, yearly PDF export |
| 6 | OnboardingScreen | 551 | 3-slide intro (FlatList pager) + profile setup form with gross/net toggle |
| 7 | CalculatorScreen | 366 | Standalone what-if calculator: rate, time, break, day type, schedule, result |
| 8 | BonusesScreen | 441 | Christmas/Easter/Vacation bonus calculators with expandable step breakdown |
| 9 | LegalInfoScreen | 199 | FAQ accordion with labor law Q&A + SEPE contact + disclaimer |
| 10 | SettingsScreen | 513 | Profile, appearance, default break, notifications, clear data |
| 11 | MoreScreen | 129 | Menu: Calculator, Bonuses, Legal, Settings, Cloud Backup, About |
| 12 | AuthScreen | 470 | Supabase auth: sign in, sign up, forgot password with error handling |
| 13 | CloudBackupScreen | 395 | Create/list/restore/delete cloud backups with status feedback |
| 14 | AboutScreen | 225 | Version, developer, rate app, privacy policy, contact, "Made in Greece" |

---

## 13. COMPONENT INVENTORY

### UI Components (8)
| Component | File | Purpose |
|-----------|------|---------|
| Card | Card.tsx | Container with padding, rounded corners, optional elevation |
| Button | Button.tsx | 4 variants: primary, outline, danger, ghost; loading state |
| FABButton | FABButton.tsx | Floating action button (+ icon) |
| Chip | Chip.tsx | Selectable chip/tag with press handler |
| ProgressBar | ProgressBar.tsx | Configurable with warning/danger thresholds |
| Badge | Badge.tsx | Generic badge/label component |
| Divider | Divider.tsx | Horizontal separator line |
| EmptyState | EmptyState.tsx | Icon + title + optional message + action button |

### Domain Components (6)
| Component | File | Purpose |
|-----------|------|---------|
| ShiftPresetButton | ShiftPresetButton.tsx | Morning/Afternoon/Night preset selector |
| BreakChips | BreakChips.tsx | Break duration chips: 0, 15, 30, 45, 60 minutes |
| EarningsCard | EarningsCard.tsx | Gross / Insurance / Net summary display |
| EarningsBreakdown | EarningsBreakdown.tsx | Step-by-step calculation breakdown list |
| OvertimeProgressBar | OvertimeProgressBar.tsx | Yearly 150h overtime limit progress |
| DayTypeBadge | DayTypeBadge.tsx | Day type indicator (regular/saturday/sunday/holiday) |

---

## 14. STATE MANAGEMENT

### 5 Zustand Stores

#### entriesStore (Single Source of Truth)
```
Fields: currentMonth, currentYear, monthEntries[], weekEntries[],
        weekStart, weekEnd, monthlyStats, weeklyStats, yearlyOvertimeTotal
Methods: setCurrentMonth, setMonthEntries, setWeekEntries, setWeekRange,
         setMonthlyStats, setWeeklyStats, setYearlyOvertimeTotal,
         addEntry, updateEntry, removeEntry
Persist: NO (data loaded from SQLite on demand)
```

#### profileStore
```
Fields: profile (WorkerProfile | null), isLoaded
Methods: setProfile, updateField, clearProfile
Persist: NO (data loaded from SQLite)
```

#### settingsStore
```
Fields: theme, language, defaultBreakMinutes, notificationsEnabled,
        overtimeNotificationsEnabled, onboardingCompleted, customPresets[]
Methods: setTheme, setLanguage, setDefaultBreakMinutes, setNotificationsEnabled,
         setOvertimeNotificationsEnabled, setOnboardingCompleted,
         setCustomPresets, addCustomPreset, removeCustomPreset
Persist: YES (AsyncStorage, key: 'myshift-settings')
```

#### authStore
```
Fields: user, session, isAuthenticated, isLoading
Methods: setSession, setLoading, clearAuth
Persist: NO (Supabase handles session persistence)
```

#### premiumStore
```
Fields: isPremium
Methods: setPremium
Persist: YES (AsyncStorage, key: 'myshift-premium')
```

---

## 15. i18n

| Language | Code | Status | Lines |
|----------|------|--------|-------|
| Greek | el | Default, fallback | ~448 |
| English | en | Complete | ~448 |

**Setup:** i18next + react-i18next, initialized in `src/i18n/setup.ts`

**Key Structure (dot-notation):**
- `tabs.*` — Tab bar labels
- `home.*` — Home screen texts
- `entry.*` — Daily entry form
- `calendar.*` — Calendar screen (monthNames, dayNames arrays)
- `reports.*` — Reports & exports
- `calculator.*` — Calculator screen
- `bonuses.*` — Bonus calculations
- `legal.*` — Legal info FAQ (questions array)
- `settings.*` — Settings screen
- `auth.*` — Authentication
- `backup.*` — Cloud backup
- `about.*` — About screen
- `onboarding.*` — Onboarding slides + setup
- `notifications.*` — Notification texts
- `earnings.*` — Earnings breakdown labels
- `bonus.*` — Bonus calculation steps
- `holidays.*` — 14 Greek holiday names
- `rateMode.*` — Gross/net labels + help
- `disclaimer.*` — Legal disclaimer text
- `errors.*` — Error messages
- `common.*` — Shared: ok, cancel, save, delete, etc.
- `accessibility.*` — Accessibility labels

---

## 16. SERVICES

### AdMob Service (App.tsx)
- Safe `require()` import — never crashes
- App Open Ad on production launch
- Disabled in `__DEV__`
- Ad Unit: `ca-app-pub-6290882379191140/8540723893`

### Notification Service (notificationService.ts)
- `requestNotificationPermission()` — Android channel setup + permission request
- `scheduleDailyReminder(hour, minute, title, body)` — Recurring daily notification
- `scheduleOvertimeAlert(current, limit, title, body)` — Immediate at 80% threshold
- `cancelDailyReminder()` / `cancelOvertimeAlert()` / `cancelAllNotifications()`

### Backup Service (backupService.ts)
- `createBackup(deviceName?)` — Serialize profile + entries + settings → Supabase jsonb
- `listBackups()` — Fetch user's backups, max 10, newest first
- `restoreBackup(id)` — Clear local → restore profile + entries + settings
- `deleteBackup(id)` — Remove from Supabase

### Export Service (exportService.ts)
- `exportMonthlyPdf(entries, stats, monthName, year)` — HTML template → expo-print → share
- `exportYearlyPdf(monthlyData, year, totals)` — Yearly HTML template → expo-print → share
- `exportMonthlyCsv(entries, monthName, year)` — CSV with UTF-8 BOM → expo-file-system → share

---

## 17. COMMANDS

```bash
# Development
npx expo start                        # Dev server
npx expo start --android              # Dev with Android device

# Type checking
npx tsc --noEmit                      # Must show 0 errors

# Testing
npx jest                              # Run all 106 tests
npx jest --watch                      # Watch mode
npx jest __tests__/calculations.test.ts  # Run specific test file

# Health check
npx expo-doctor                       # Check for issues

# Build
eas build --profile preview           # Preview APK (internal testing)
eas build --profile production        # Production AAB (Play Store)

# Submit
eas submit --profile production       # Submit to Play Store (requires play-store-key.json)
```

---

## 18. TESTING

| Test File | Tests | Lines | Coverage |
|-----------|-------|-------|----------|
| calculations.test.ts | 58 | 596 | Payroll engine: hours, earnings, insurance, bonuses |
| dateUtils.test.ts | 38 | 286 | Date formatting, week ranges, Greek month/day names |
| greekHolidays.test.ts | 10 | 170 | Orthodox Easter algorithm, fixed/moving holidays |
| **Total** | **106** | **1,052** | Core calculation + date + holiday logic |

**Test Environment:** Node (not jsdom — pure logic tests)
**Runner:** Jest 29 + ts-jest
**Module Resolution:** `@/*` mapped to `<rootDir>/src/*`

---

## 19. CODE REVIEW SCORES

| Metric | Score | Notes |
|--------|-------|-------|
| Type Safety | 10/10 | Strict mode, noUncheckedIndexedAccess, no `any` |
| Greek Law Compliance | 10/10 | All rates from ΦΕΚ, cumulative premiums, tiered insurance |
| Offline-First | 10/10 | SQLite local, Supabase optional for backup |
| Dark Mode | 10/10 | Every component themed, WCAG contrast verified |
| i18n | 10/10 | Zero hardcoded strings, bilingual EL/EN |
| Accessibility | 9/10 | Labels, roles, 48px touch targets (date picker missing) |
| Error Handling | 9/10 | Error boundary, try/catch on all DB ops, safe AdMob import |
| Performance | 9/10 | useCallback/useMemo throughout, SQL indexes, no module-level Dimensions |
| Code Organization | 10/10 | Clear separation: screens/components/stores/utils/services |
| Test Coverage | 8/10 | Core logic well tested; UI screens not unit tested |

---

## 20. PRIVACY POLICY

- **URL:** https://fiouri.github.io/myshift/privacy-policy.html *(LIVE)*
- **Location:** `docs/privacy-policy.html`
- **Developer:** Any WeCon
- **Contact:** info@anywecon.com
- **Languages:** Greek + English (bilingual)
- **GDPR Compliance:** Yes
- **Data Collection:**
  - Local only (SQLite on device)
  - Cloud backup optional (Supabase, user-initiated)
  - No tracking, no analytics beyond AdMob
  - AdMob collects standard ad data per Google's policy
- **Data Deletion:** "Clear All Data" in Settings

---

## 21. PLAY STORE CONFIG

### app.json
```json
{
  "expo": {
    "name": "MyShift",
    "slug": "GreekPayrollApp",
    "version": "2.0.0",
    "orientation": "portrait",
    "userInterfaceStyle": "automatic",
    "newArchEnabled": true,
    "android": {
      "adaptiveIcon": { "foregroundImage": "./assets/adaptive-icon.png", "backgroundColor": "#1B5E9E" },
      "edgeToEdgeEnabled": true,
      "package": "com.anywecon.myshift",
      "versionCode": 1
    },
    "plugins": ["expo-sqlite", "expo-localization", "expo-font",
      ["expo-notifications", { "color": "#1B5E9E" }],
      ["react-native-google-mobile-ads", { "androidAppId": "ca-app-pub-6290882379191140~2054915018" }]
    ],
    "extra": { "eas": { "projectId": "e18bd7bc-0bc6-4c92-9fbb-67a4f0eb953a" } },
    "owner": "fiouri"
  }
}
```

### eas.json
```json
{
  "cli": { "version": ">= 16.0.0", "appVersionSource": "local" },
  "build": {
    "preview": { "distribution": "internal", "android": { "buildType": "apk" } },
    "production": { "distribution": "store", "android": { "buildType": "app-bundle" }, "autoIncrement": true }
  },
  "submit": {
    "production": { "android": { "serviceAccountKeyPath": "./play-store-key.json", "track": "production" } }
  }
}
```

---

## 22. MONETIZATION

### Freemium Model
- **Free tier:** All features, AdMob App Open ad on launch
- **Premium tier:** Ad-free (flag in premiumStore, ready for RevenueCat)
- **Current state:** Only free tier active; premiumStore is a placeholder

---

## 23. KNOWN ISSUES

1. **Date Input:** Text input for dates (HH:MM, YYYY-MM-DD) instead of native picker — planned for v2.1
2. **Offsite Rates:** Defined in `lawConstants.ts` but not yet exposed in UI
3. **Vacation Days Table:** Defined in `lawConstants.ts` but not yet shown in screens
4. **No Automated UI Tests:** Only unit tests for pure logic functions
5. **New Architecture:** `newArchEnabled: true` may cause issues with some third-party libraries
6. **CSV Export:** Uses Greek column headers only (not localized)

### Resolved Bugs (v2.0)
- [x] **Night-hours pro-rata break fix** — Break minutes now deducted proportionally between night and regular hours instead of all from regular
- [x] **weekEntries sync fix** — HomeScreen weekEntries now sync correctly when navigating back from DailyEntry

---

## 24. VERSION HISTORY

| Version | Date | Notes |
|---------|------|-------|
| v1.x | 2025 | Original app: same Supabase, same AdMob, same EAS project |
| v2.0.0 | 2026-04-04 | Complete rewrite: Expo SDK 54, React 19, RN 0.81, new calculation engine, new UI |
| v2.0.0 | 2026-04-06 | Bug fixes: night-hours pro-rata break, weekEntries sync. Gross/net toggle complete. Privacy policy live. |

---

## 25. V1 LESSONS LEARNED

1. **Separate overtimeStore caused data drift** → v2 uses single entriesStore
2. **Hardcoded Greek strings** → v2 uses i18n everywhere
3. **No dark mode on some screens** → v2 themes every component
4. **Magic numbers in calculations** → v2 uses lawConstants.ts with law references
5. **No error boundary** → v2 has top-level ErrorBoundary
6. **No migration system** → v2 has versioned SQL migrations
7. **AdMob crashes when module missing** → v2 safe require() import
8. **Dimensions.get() at module level** → v2 uses useWindowDimensions hook

---

## 26. NEXT STEPS

### Latest APK Build
- **URL:** https://expo.dev/accounts/fiouri/projects/GreekPayrollApp/builds/38bafd60-8ce2-4d73-a76a-756d3a6b5939
- **Profile:** preview (internal APK)
- **Status:** Available for testing

### Immediate (v2.0 → Play Store)
1. ~~Run `npx tsc --noEmit` — verify 0 errors~~ ✅
2. ~~Run `npx jest` — verify all 106 tests pass~~ ✅
3. ~~Run `eas build --profile preview` — test APK on device~~ ✅ (see link above)
4. Run `eas build --profile production` — production AAB
5. Create Play Store listing (screenshots, description, icon)
6. Submit via `eas submit --profile production`

### Near-term (v2.1)
- Native date/time picker (replace text input)
- Offsite rates UI integration
- Vacation days calculator
- Backup auto-sync on entry save
- Push notifications via Expo Push API

### Mid-term (v2.2+)
- Multi-employer support
- RevenueCat premium subscriptions
- Payslip scanner (OCR verification)
- Tax estimation calculator
- Android home screen widget
- CSV export localization

---

> **Note:** This backup reflects the codebase as of 2026-04-06. All 72 files (12,834 lines) have been read and verified against this document.
