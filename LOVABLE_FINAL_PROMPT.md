# LOVABLE_FINAL_PROMPT - QueueLess v6

השתמש בפרומפט הזה אם רוצים לבנות או לשחזר את QueueLess ב-Lovable.

## Context

Build a simple MVP web app called QueueLess for a single barber shop named “מספרת רמת גן”.

The product solves appointment chaos caused by WhatsApp, phone calls and manual calendars.

Keep the app simple. Do not add features beyond this prompt.

Current live deployment:

- Main domain: `https://queue-less-rho.vercel.app/`
- Direct app URL: `https://queue-less-rho.vercel.app/QueueLess_v5.html`
- GitHub repo: `https://github.com/yitzhakyosef/QueueLess`

## Language and UX

- UI language: Hebrew only.
- Direction: RTL.
- Visual style: real barber shop website, warm colors, service cards, clean dashboard.
- Responsive basic layout for desktop and mobile.
- Do not claim full mobile-first implementation.

## Tech Rules

- Use localStorage only.
- No Supabase.
- No backend.
- No real SMS.
- No payments.
- No multi-business.
- No inventory.
- Keep `index.html` as the Vercel entrypoint that opens `QueueLess_v5.html`.

## User Types

### Customer

Customer has registration, login and OTP.

### Owner

Owner enters with demo code `RG2026`.

Owner does not need OTP.

## Customer Registration

Create a registration screen with required fields:

- Full name in Hebrew only.
- Israeli mobile phone, 10 digits, starts with `05`.
- National ID, exactly 9 digits.
- Email address.
- Personal password, exactly 4 digits.

Validation:

- Name hint: “יש להזין שם בעברית בלבד”.
- Password hint: “הסיסמה חייבת להכיל 4 ספרות”.
- Invalid password error: “יש להזין סיסמה בת 4 ספרות”.
- Duplicate phone error: “מספר הנייד הזה כבר רשום במערכת. יש לעבור להתחברות”.
- Duplicate national ID error: “תעודת הזהות הזו רשומה על לקוח אחר”.

On successful registration:

- Save user to `queueless-users`.
- Log the customer in automatically.
- Show the customer home screen.

## Customer Login

Login screen has only:

- Phone.
- Password.

Rules:

- Phone not found: “לא נמצא לקוח עם מספר הנייד הזה. יש להירשם תחילה”.
- Add direct link to registration.
- Wrong password: “סיסמה שגויה. נסה שוב”.
- Correct phone and password: show OTP screen.

Do not use a shared customer password.

## OTP Screen

Use a separate OTP screen.

- Show the customer phone.
- One input for 6 digits.
- Fixed demo code: `123456`.
- Define one constant: `DEMO_OTP_CODE`.
- Add this exact code comment near the constant:

```js
// במערכת אמיתית הקוד ייווצר אקראית ויישלח דרך שירות SMS חיצוני, ולא יישמר בצד הלקוח.
```

- Wrong OTP: “קוד האימות שגוי. נסה שוב”.
- Correct OTP: complete login.
- Add a back button to login.
- Show a clear Hebrew demo note that no real SMS is sent.

## Services

- תספורת גבר, 30 דקות, 70 ₪.
- עיצוב זקן, 20 דקות, 40 ₪.
- תספורת + זקן, 45 דקות, 100 ₪.
- תספורת ילד, 25 דקות, 50 ₪.

## Booking

Required fields:

- Service.
- Date.
- Time.
- Phone.

Appointment starts as `pending`.

Slot locking:

- Slot is taken if status is `pending` or `confirmed`.
- Slot is available if status is `cancelled`.
- Do not let a customer select a taken slot.

Business explanation for code comments:

The owner reviews appointment requests once per day around 22:00, so a pending request must lock the slot immediately.

## My Appointments

Show only appointments where `appointment.customer_id` equals the logged-in customer ID.

Display:

- Service.
- Date.
- Time.
- Status.

## Owner Dashboard

Owner dashboard includes:

- Pending appointments.
- Today appointments.
- Future appointments.
- Cancelled appointments.
- All appointments.
- Customers tab.
- Blocked dates area.

Owner can approve and cancel appointments.

## Customers Tab

Show all users from `queueless-users`.

Display:

- Full name.
- Phone.
- Appointment count.
- Last appointment date.

Add search by name or phone.

Empty state: “עדיין אין לקוחות רשומים”.

Do not show passwords anywhere.

Show national ID only inside an expanded customer card.

## Blocked Dates

Add a normal owner dashboard tab named “חסימת תאריכים”.

This tab must be part of the regular dashboard tabs. When selected, show only the blocked dates content. Do not show appointment list content, customer list content, or content from another tab.

Owner can:

- Select “מתאריך”.
- Select “עד תאריך”.
- Click “חסום טווח”.
- Block one day by selecting the same date in both fields.
- See blocked ranges.
- Remove each blocked range separately.

Validation:

- Empty start or end date shows a Hebrew field error.
- End date before start date shows: “תאריך הסיום חייב להיות אחרי תאריך ההתחלה”.

Storage:

- Use the existing key `queueless-blocked-dates`.
- Save ranges as `{ id, start_date, end_date, created_at }`.
- Keep backward compatibility with old single-date string values by treating them as one-day ranges.

Customer side:

- If selected date is inside a blocked range, including start and end dates, show “המספרה סגורה בתאריך זה”.
- Do not show time slots.
- Block submission.

## Data Models

### User

```js
{
  id,
  full_name,
  phone,
  national_id,
  email,
  password,
  created_at
}
```

### Appointment

```js
{
  id,
  customer_id,
  customer_name,
  customer_phone,
  service_id,
  service_name,
  appointment_date,
  appointment_time,
  status,
  created_at,
  updated_at
}
```

## localStorage Keys

- `queueless-users`
- `queueless_ux_appointments`
- `queueless-blocked-dates`
- `queueless_ux_session`

## Final Checks

- Valid registration logs customer in.
- Invalid fields show Hebrew errors.
- Login has phone and password only.
- Correct login leads to OTP.
- OTP `123456` logs in.
- Pending slot is locked immediately.
- Cancelled slot becomes available.
- Blocked date blocks booking.
- Owner customer list works.
- Data survives refresh.
- Opening `index.html` opens the app.
