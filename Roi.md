# רועי - Lead Builder

## אחריות שלך

- להסביר איך המערכת בנויה בפועל.
- להבין את הקובץ `QueueLess_v5.html`, למרות שהוא כבר מכיל את עדכוני v6.
- לשמור על Vanilla JS, RTL, עברית ו-`localStorage`.
- לא להוסיף פיצ'רים מעבר למה שאושר.

## מבנה טכני

- הקובץ הראשי: `QueueLess_v5.html`.
- קובץ הכניסה לפרסום: `index.html`.
- הדומיין הראשי: `https://queue-less-rho.vercel.app/`.
- GitHub repo: `https://github.com/yitzhakyosef/QueueLess`.
- אין Backend אמיתי.
- שכבת הנתונים בדמו היא `localStorage`.
- הלוגיקה העסקית נמצאת בפונקציות JavaScript בתוך הקובץ.

## מה לקרוא ב-PRD

- `Data Model` ← להבין את מבנה users, appointments, services, blocked_dates ו-session.
- `כללי Business Logic` ← נעילת slots, חסימת טווחים והרשאות דמו.
- `מסכים` ← להבין איזה state כל מסך צריך להציג.
- `Acceptance Criteria` ← לדעת מה הקוד חייב לעבור.
- `מחוץ ל-Scope` ← כדי לא להוסיף Supabase, SMS, תשלומים או multi-business.
- `blocked_dates` ← להבין את ההבדל בין `full-day` לבין `hours`.

## מפתחות localStorage

| מפתח | תפקיד |
|---|---|
| `queueless-users` | רשימת לקוחות רשומים |
| `queueless_ux_appointments` | רשימת תורים |
| `queueless-blocked-dates` | טווחי תאריכים חסומים |
| `queueless_ux_session` | שמירת המשתמש המחובר |

## מודל לקוח

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

## מודל תור

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

## לוגיקות שחייבים לשמור

- בהרשמה יש `pendingRegistrationData`, והמשתמש נשמר רק אחרי OTP תקין.
- אותו מסך OTP משמש גם להרשמה וגם להתחברות באמצעות `pendingOtpMode`.
- `pending` ו-`confirmed` נועלים שעה.
- `cancelled` משחרר שעה.
- תאריך שנמצא בתוך טווח חסום לא מציג שעות ולא מאפשר שליחה.
- מבנה חסימת יום מלא: `{ id, type: "full-day", start_date, end_date, created_at }`.
- מבנה חסימת שעות: `{ id, type: "hours", date, start_time, end_time, created_at }`.
- בדיקת חסימת יום מתבצעת על ידי השוואה: `date >= start_date && date <= end_date`.
- בדיקת חסימת שעות מתבצעת על ידי השוואה: `appointment_time >= start_time && appointment_time <= end_time`.
- הפורמט הישן של תאריך בודד נתמך על ידי המרה לטווח שבו ההתחלה והסיום זהים.
- חסימה שמתנגשת עם תורים פעילים שומרת קודם `pendingBlockRequest` ומחכה לאישור בעל העסק.
- אחרי אישור החסימה, תורים מתנגשים עוברים ל-`cancelled` דרך אותו מערך תורים.
- אירוע `storage` מרענן טאב לקוח פתוח כאשר `queueless_ux_appointments` משתנה.
- לקוח רואה רק תורים עם `customer_id` שלו.
- סיסמאות לקוחות לא מוצגות לבעל העסק.
- בעל עסק נכנס עם `RG2026`.
- לקוח חוזר עובר OTP עם הקוד `123456`.
- הטאב “הכל” הוסר מהדשבורד.

## שאלות שהמרצה יכולה לשאול

### איפה ה-Backend?

בדמו אין שרת. `localStorage` משמש שכבת Data, וה-JavaScript משמש שכבת Business Logic. ב-Production זה היה עובר לשרת או ל-Supabase.

### איך נמנע double booking?

הפונקציה שבודקת שעה תפוסה מחשיבה גם `pending` וגם `confirmed` כתפוסים.

### איך הנתונים שורדים רענון?

הם נשמרים ב-`localStorage`, ולכן נשארים בדפדפן גם אחרי refresh.

### איך טאב לקוח מתעדכן כשבעל העסק מבטל?

הביטול נשמר ב-`queueless_ux_appointments`. טאב אחר מקבל אירוע `storage` ומרנדר מחדש את “התורים שלי”.
