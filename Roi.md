# רועי - Lead Builder

## אחריות שלך

- להסביר איך המערכת בנויה בפועל.
- להבין את הקובץ `QueueLess_v5.html`, למרות שהוא כבר מכיל את עדכוני v6.
- לשמור על Vanilla JS, RTL, עברית ו-`localStorage`.
- לא להוסיף פיצ'רים מעבר למה שאושר.

## מבנה טכני

- הקובץ הראשי: `QueueLess_v5.html`.
- אין Backend אמיתי.
- שכבת הנתונים בדמו היא `localStorage`.
- הלוגיקה העסקית נמצאת בפונקציות JavaScript בתוך הקובץ.

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

- `pending` ו-`confirmed` נועלים שעה.
- `cancelled` משחרר שעה.
- תאריך שנמצא בתוך טווח חסום לא מציג שעות ולא מאפשר שליחה.
- מבנה טווח חסום: `{ id, start_date, end_date, created_at }`.
- בדיקת חסימה מתבצעת על ידי השוואה: `date >= start_date && date <= end_date`.
- הפורמט הישן של תאריך בודד נתמך על ידי המרה לטווח שבו ההתחלה והסיום זהים.
- לקוח רואה רק תורים עם `customer_id` שלו.
- סיסמאות לקוחות לא מוצגות לבעל העסק.
- בעל עסק נכנס עם `RG2026`.
- לקוח חוזר עובר OTP עם הקוד `123456`.

## שאלות שהמרצה יכולה לשאול

### איפה ה-Backend?

בדמו אין שרת. `localStorage` משמש שכבת Data, וה-JavaScript משמש שכבת Business Logic. ב-Production זה היה עובר לשרת או ל-Supabase.

### איך נמנע double booking?

הפונקציה שבודקת שעה תפוסה מחשיבה גם `pending` וגם `confirmed` כתפוסים.

### איך הנתונים שורדים רענון?

הם נשמרים ב-`localStorage`, ולכן נשארים בדפדפן גם אחרי refresh.
