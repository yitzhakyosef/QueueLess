# CLAUDE_BUILD_PROMPT - QueueLess v6

השתמש בפרומפט הזה כדי לבנות או לעדכן את מערכת QueueLess דרך Claude.

## תפקיד Claude

אתה Lead Builder לפרויקט קורס Vibe Coding.

בנה מערכת MVP פשוטה, יציבה וברורה עבור QueueLess - מספרת רמת גן.

אל תוסיף פיצ'רים מעבר למה שמוגדר כאן.

## Product Goal

QueueLess היא מערכת לקביעת תורים במספרת רמת גן, כדי למנוע בלאגן בתורים, כפילויות, ביטולים לא מתועדים ושיחות טלפון מיותרות.

הגרסה הנוכחית מפורסמת כאן:

- דומיין הפרויקט: `https://queue-less-rho.vercel.app/`
- קישור להצגה: `https://queue-less-rho.vercel.app/QueueLess.html`
- GitHub repo: `https://github.com/yitzhakyosef/QueueLess`

## Tech Constraints

- קובץ HTML אחד.
- Vanilla JavaScript.
- CSS פנימי.
- עברית בלבד ב-UI.
- RTL.
- התאמה רספונסיבית בסיסית.
- לא להצהיר על mobile-first מלא.
- `localStorage` בלבד.
- אין Supabase.
- אין Backend.
- אין SMS אמיתי.
- אין תשלומים.
- אין multi-business.
- אין מלאי.

## Main File

עדכן או צור את הקובץ:

`QueueLess.html`

הקובץ צריך לשקף את לוגיקת v6.

אל תיצור `index.html` בשלב הנוכחי. קובץ המערכת הראשי הוא `QueueLess.html`.

## Demo Business

- שם העסק: מספרת רמת גן.
- בעל העסק נכנס עם הקוד `RG2026`.
- לבעל העסק אין OTP.

## Services

| Service | Duration | Price |
|---|---:|---:|
| תספורת גבר | 30 דקות | 70 ₪ |
| עיצוב זקן | 20 דקות | 40 ₪ |
| תספורת + זקן | 45 דקות | 100 ₪ |
| תספורת ילד | 25 דקות | 50 ₪ |

## Customer Registration

צור מסך הרשמה נפרד מהתחברות.

שדות חובה:

- `full_name` ← שם מלא בעברית בלבד.
- `phone` ← נייד ישראלי בן 10 ספרות שמתחיל ב-05.
- `national_id` ← תעודת זהות בת 9 ספרות.
- `email` ← מייל תקין.
- `password` ← סיסמה אישית בת 4 ספרות.

הערות ושגיאות:

- מתחת לשדה שם הצג: “יש להזין שם בעברית בלבד”.
- מתחת לשדה סיסמה הצג: “הסיסמה חייבת להכיל 4 ספרות”.
- סיסמה לא תקינה ← “יש להזין סיסמה בת 4 ספרות”.
- נייד קיים ← “מספר הנייד הזה כבר רשום במערכת. יש לעבור להתחברות”.
- תעודת זהות קיימת אצל לקוח אחר ← “תעודת הזהות הזו רשומה על לקוח אחר”.

כל שגיאה צריכה להופיע בעברית מתחת לשדה הרלוונטי.

הרשמה תקינה:

- אינה יוצרת עדיין לקוח.
- שומרת את פרטי ההרשמה בזיכרון זמני בלבד.
- מעבירה למסך OTP.
- רק אחרי OTP תקין יוצרת לקוח חדש ב-`queueless-users`.
- מחברת את הלקוח ומעבירה לדף הבית.
- אם הקוד שגוי, המשתמש לא נשמר.

## Customer Login

מסך התחברות לקוח כולל בדיוק שני שדות:

- נייד.
- סיסמה אישית.

כללים:

- נייד לא קיים ← “לא נמצא לקוח עם מספר הנייד הזה. יש להירשם תחילה”.
- הצג קישור ישיר להרשמה במקרה של נייד לא קיים.
- סיסמה שגויה ← “סיסמה שגויה. נסה שוב”.
- נייד וסיסמה תקינים ← מעבר למסך OTP.

אין להשתמש בסיסמה אחידה ללקוחות.

## OTP

צור מסך OTP נפרד.

כללים:

- הצג שהקוד נשלח לנייד של הלקוח.
- שדה אחד בן 6 ספרות.
- קוד דמו קבוע: `123456`.
- הגדר קבוע אחד בשם `DEMO_OTP_CODE`.
- ליד הקבוע בקוד הוסף תגובה:

```js
// במערכת אמיתית הקוד ייווצר אקראית ויישלח דרך שירות SMS חיצוני, ולא יישמר בצד הלקוח.
```

- קוד שגוי ← “קוד האימות שגוי. נסה שוב”.
- OTP משמש גם להרשמה וגם להתחברות.
- בהרשמה: קוד תקין יוצר את המשתמש ורק אז מחבר אותו.
- בהתחברות: קוד תקין מחבר את הלקוח.
- השתמש באותו רכיב OTP ובאותו קבוע, בלי לשכפל קוד.
- הוסף כפתור חזרה למסך התחברות.
- הצג הערת דמו ברורה שאין SMS אמיתי.

## Data Models

### User

```js
{
  id: string,
  full_name: string,
  phone: string,
  national_id: string,
  email: string,
  password: string,
  created_at: string
}
```

### Appointment

```js
{
  id: string,
  customer_id: string,
  customer_name: string,
  customer_phone: string,
  service_id: string,
  service_name: string,
  appointment_date: string,
  appointment_time: string,
  status: "pending" | "confirmed" | "cancelled",
  created_at: string,
  updated_at: string
}
```

### Blocked Full Day

```js
{
  id: string,
  type: "full-day",
  start_date: string,
  end_date: string,
  created_at: string
}
```

### Blocked Hours

```js
{
  id: string,
  type: "hours",
  date: string,
  start_time: string,
  end_time: string,
  created_at: string
}
```

## localStorage Keys

- `queueless-users`
- `queueless_ux_appointments`
- `queueless-blocked-dates`
- `queueless_ux_session`

## Booking Logic

- כל תור חדש מתחיל ב-`pending`.
- שעה תפוסה אם יש עליה תור `pending` או `confirmed`.
- `cancelled` משחרר את השעה.
- אין לאפשר ללקוח לבחור שעה תפוסה.
- אם בעל העסק מבטל תור, שמור אותו כ-`cancelled` ואל תמחק אותו.
- במסך “התורים שלי” הצג גם תורים שבוטלו עם תג סטטוס “בוטל”.
- הוסף listener לאירוע `storage` כדי שטאב לקוח פתוח יתרענן כאשר `queueless_ux_appointments` משתנה.
- הערת קוד: במערכת אמיתית ביטול כזה היה שולח גם התראה יזומה ללקוח.
- אין צורך בהודעת התנגשות באישור בעל עסק, כי לא אמורות להיווצר שתי בקשות על אותה שעה.
- הסבר עסקי בקוד: בעל העסק עובר על היומן פעם ביום סביב 22:00, ולכן שעה ממתינה חייבת להינעל.

## Blocked Dates and Hours

בדשבורד בעל העסק הוסף טאב רגיל בשם “חסימת תאריכים”.

הטאב חייב להיות חלק ממנגנון הטאבים הקיים, כך שבבחירה בו יוצג רק תוכן חסימת התאריכים ולא יוצג תוכן של יומן, לקוחות או טאב אחר.

בעל העסק יכול:

- לבחור בין מצב “יום או טווח מלא” לבין מצב “שעות ביום”.
- במצב יום מלא: לבחור “מתאריך” ו-“עד תאריך”.
- ללחוץ “חסום יום או טווח”.
- לחסום יום בודד על ידי בחירת אותו תאריך בשני השדות.
- במצב שעות: לבחור תאריך, שעת התחלה ושעת סיום.
- ללחוץ “חסום שעות”.
- לראות רשימת חסימות שמבדילה בין יום שלם לבין שעות.
- להסיר כל חסימה בנפרד.

ולידציות:

- שדה ריק מציג שגיאה בעברית מתאימה.
- תאריך סיום מוקדם מתאריך התחלה מציג “תאריך הסיום חייב להיות אחרי תאריך ההתחלה”.
- שעת סיום מוקדמת משעת התחלה מציגה “שעת הסיום חייבת להיות אחרי שעת ההתחלה”.

שמירה:

- השתמש במפתח הקיים `queueless-blocked-dates`.
- שמור יום מלא במבנה `{ id, type: "full-day", start_date, end_date, created_at }`.
- שמור שעות במבנה `{ id, type: "hours", date, start_time, end_time, created_at }`.
- תמוך בפורמט הישן של תאריך בודד כמחרוזת על ידי המרה לטווח שבו ההתחלה והסיום זהים.

בצד הלקוח:

- אם נבחר תאריך שנמצא בתוך טווח חסום, כולל תאריכי הקצה, הצג “המספרה סגורה בתאריך זה”.
- אל תציג שעות.
- חסום שליחת טופס.
- אם קיימת חסימת שעות, הסתר רק את הסלוטים שנמצאים בטווח השעות והשאר את שאר השעות זמינות.
- חסום שליחת תור גם אם השעה נבחרה לפני שנוצרה חסימה.

תורים מתנגשים:

- לפני שמירה של חסימה, בדוק האם קיימים תורים `pending` או `confirmed` בתוך החסימה.
- אם קיימים, הצג אזהרה עם שם לקוח, תאריך ושעה.
- הצג שתי פעולות: “המשך וחסום” ו-“ביטול”.
- “המשך וחסום” שומר את החסימה ומעביר תורים מתנגשים ל-`cancelled`.
- “ביטול” לא שומר חסימה ולא משנה תורים.
- ביטול אוטומטי משתמש באותו מנגנון `localStorage` כך שלקוח פתוח מתעדכן דרך `storage`.

## Owner Dashboard

הדשבורד כולל:

- בקשות ממתינות.
- תורים להיום.
- תורים עתידיים.
- תורים שבוטלו.
- לקוחות.
- חסימת תאריכים ושעות כטאב עצמאי.

בעל העסק יכול לאשר או לבטל תורים.

אל תציג טאב “הכל”.

## Customers Management

בטאב “לקוחות”:

- הצג את כל הלקוחות מ-`queueless-users`.
- לכל לקוח הצג שם ונייד.
- הצג כמות תורים.
- הצג תאריך תור אחרון.
- הוסף חיפוש לפי שם או נייד.
- אם אין לקוחות, הצג “עדיין אין לקוחות רשומים”.
- אל תציג סיסמאות.
- תעודת זהות מוצגת רק בפתיחת כרטיס לקוח בודד.

## Required Screens

- Login and Registration.
- OTP.
- Customer Home.
- Booking Page.
- Appointment Confirmation.
- My Appointments.
- Owner Dashboard.
- Customers Management.
- Blocked Dates and Hours.

## Acceptance Criteria

- הרשמה תקינה מובילה ל-OTP.
- OTP שגוי בהרשמה לא יוצר משתמש.
- OTP `123456` בהרשמה יוצר לקוח ומחבר אותו.
- כל שדות ההרשמה הם חובה.
- סיסמה של 3 ספרות, 5 ספרות או אותיות נחסמת.
- נייד כפול נחסם.
- תעודת זהות כפולה נחסמת.
- התחברות לקוח כוללת נייד וסיסמה בלבד.
- נייד לא קיים מציג קישור להרשמה.
- סיסמה שגויה נחסמת.
- התחברות תקינה מובילה ל-OTP.
- OTP שגוי נחסם.
- `123456` מחבר את הלקוח.
- שני לקוחות עם סיסמאות שונות יכולים להתחבר רק לחשבון שלהם.
- תור `pending` נועל שעה.
- ביטול תור משחרר שעה.
- ביטול תור נשאר מוצג ללקוח כ-“בוטל”.
- טאב לקוח פתוח מתעדכן דרך `storage`.
- תאריך חסום לא מאפשר קביעת תור.
- חסימת שעות מסתירה רק שעות חסומות.
- חסימה עם תורים פעילים מציגה אזהרה.
- “המשך וחסום” מבטל תורים מתנגשים.
- טאב “הכל” לא קיים בדשבורד.
- בעל העסק רואה לקוחות ומחפש לפי שם או נייד.
- נתונים שורדים רענון.
- פתיחת `QueueLess.html` מובילה למערכת.

## Do Not Add

- Supabase.
- Database חיצוני.
- SMS אמיתי.
- Email login.
- Password reset.
- תשלומים.
- כמה עסקים.
- מלאי.
- פיצ'רים שלא אושרו.


## Update: Owner Data View Tab

Add an owner-only dashboard tab named `מבט נתונים`.

Requirements:

- The tab is visible only after owner login with `RG2026`.
- It is part of the same tab mechanism as the other owner dashboard tabs.
- It displays live data from the existing sources, without duplicating data.
- Tables:
  - `users` from `queueless-users`: `id`, `full_name`, `phone`, `national_id`, `email`, `created_at`.
  - `appointments` from `queueless_ux_appointments`: `id`, `customer_id`, `customer_name`, `service_id`, `service_name`, `date`, `time`, `status`, `updated_at`.
  - `blocked_time_ranges` from `queueless-blocked-dates`: `id`, `type`, `start_date`, `end_date`, `start_time`, `end_time`, business meaning, `created_at`.
  - `services` from the static JavaScript array, with a note that it is not stored in `localStorage`.
  - `active_session` from `queueless_ux_session`.
- Do not show customer passwords. If the field exists in storage, omit it from the table.
- Show empty state text: `אין רשומות להצגה`.
- Add four explanatory cards: customer ← appointment relation, slot locking, owner blocking, and why this is not Production.