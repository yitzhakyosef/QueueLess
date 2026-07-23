# LOVABLE_FINAL_PROMPT - QueueLess v6

השתמש בפרומפט הזה אם רוצים לבנות או לשחזר את QueueLess ב-Lovable.

## Context

Build a simple MVP web app called QueueLess for a single barber shop named “מספרת רמת גן”.

The product solves appointment chaos caused by WhatsApp, phone calls and manual calendars.

Keep the app simple. Do not add features beyond this prompt.

Current live deployment:

- Project domain: `https://queue-less-rho.vercel.app/`
- Presentation URL: `https://queue-less-rho.vercel.app/QueueLess.html`
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
- Do not create `index.html` in the current project structure. The main app file is `QueueLess.html`.

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

- Do not save the user yet.
- Keep registration details temporarily in memory.
- Show the OTP screen.
- Only after correct OTP, save user to `queueless-users`.
- Log the customer in and show the customer home screen.
- Wrong OTP must not create a user.

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
- Use the same OTP screen for registration and login.
- Correct OTP during registration creates the user and logs in.
- Correct OTP during login completes login.
- Do not duplicate OTP logic.
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
- If an owner cancels an appointment, keep it in storage with status `cancelled`.
- My Appointments must show cancelled appointments with a clear “בוטל” badge.
- Add a browser `storage` event listener so an open customer tab updates when `queueless_ux_appointments` changes.
- Code comment: in a real system this update would also trigger an external customer notification.

Business explanation for code comments:

The owner reviews appointment requests once per day around 22:00, so a pending request must lock the slot immediately.

## My Appointments

Show only appointments where `appointment.customer_id` equals the logged-in customer ID.

Display:

- Service.
- Date.
- Time.
- Status.

Cancelled appointments must remain visible. Do not remove them from the list.

## Owner Dashboard

Owner dashboard includes:

- Pending appointments.
- Today appointments.
- Future appointments.
- Cancelled appointments.
- Customers tab.
- Blocked dates and hours tab.

Owner can approve and cancel appointments.

Do not show an “all” tab.

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

## Blocked Dates and Hours

Add a normal owner dashboard tab named “חסימת תאריכים”.

This tab must be part of the regular dashboard tabs. When selected, show only the blocked dates content. Do not show appointment list content, customer list content, or content from another tab.

Owner can:

- Choose between “יום או טווח מלא” and “שעות ביום”.
- For full-day mode, select “מתאריך” and “עד תאריך”.
- Click “חסום יום או טווח”.
- Block one day by selecting the same date in both fields.
- For hours mode, select a date, start time and end time.
- Click “חסום שעות”.
- See blocked items with clear labels: full day or hours.
- Remove each blocked item separately.

Validation:

- Empty start or end date shows a Hebrew field error.
- End date before start date shows: “תאריך הסיום חייב להיות אחרי תאריך ההתחלה”.
- Empty hour fields show Hebrew field errors.
- End time before start time shows: “שעת הסיום חייבת להיות אחרי שעת ההתחלה”.

Storage:

- Use the existing key `queueless-blocked-dates`.
- Save full-day blocks as `{ id, type: "full-day", start_date, end_date, created_at }`.
- Save hour blocks as `{ id, type: "hours", date, start_time, end_time, created_at }`.
- Keep backward compatibility with old single-date string values by treating them as one-day ranges.

Customer side:

- If selected date is inside a blocked range, including start and end dates, show “המספרה סגורה בתאריך זה”.
- Do not show time slots.
- Block submission.
- If there is an hour block on a date, hide only the slots inside the blocked range.
- Enforce hour blocks again during form submission.

Conflicting appointments:

- Before saving a block, check for `pending` or `confirmed` appointments inside the blocked time.
- If conflicts exist, show a Hebrew warning with customer name, appointment date and time.
- Show actions: “המשך וחסום” and “ביטול”.
- Continue saves the block and changes conflicting appointments to `cancelled`.
- Cancel does not save a block and does not change appointments.
- The automatic cancellation must update the same appointments storage key so customer tabs update through `storage`.

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

## Blocked Item Models

### Full Day

```js
{
  id,
  type: "full-day",
  start_date,
  end_date,
  created_at
}
```

### Hours

```js
{
  id,
  type: "hours",
  date,
  start_time,
  end_time,
  created_at
}
```

## Final Checks

- Valid registration leads to OTP before user creation.
- Wrong OTP during registration does not create a user.
- OTP `123456` completes registration and login.
- Invalid fields show Hebrew errors.
- Login has phone and password only.
- Correct login leads to OTP.
- OTP `123456` logs in.
- Pending slot is locked immediately.
- Cancelled slot becomes available.
- Blocked date blocks booking.
- Blocked hours hide only relevant slots.
- Blocking time with active appointments shows warning.
- Continue block cancels conflicting appointments.
- Cancel warning changes nothing.
- Owner dashboard has no “all” tab.
- Owner customer list works.
- Data survives refresh.
- Opening `QueueLess.html` opens the app.
