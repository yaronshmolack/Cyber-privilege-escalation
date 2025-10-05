# Linux Capabilities — איתור, הבנה והמלצות

> **הערה אתית:** כל הפקודות וההסברים מיועדים לשימוש חוקי בלבד — מעבדה, CTFים מורשים ואודיטים. אין להשתמש בהן כנגד מערכות ללא הרשאה מפורשת.

---

## מה הן "Capabilities" בלינוקס?

במקום להעניק ל־process את כל הרשאות ה‑root באמצעות SUID, ישנן "יכולות" (capabilities) שמאפשרות להעניק להרצה חלק מההרשאות המנהלתיות (למשל יכולת לפתוח raw sockets, לשנות UID, לקשור פורטים נמוכים וכו'). זה מאפשר להקטין את שטח התקיפה לעומת מתן root מלא.

---

## איך למצוא קבצים עם capabilities מופעלות

כלי שימושי: `getcap` (חלק מ־`libcap`)

### חיפוש רקורסיבי (מניעת רעש של שגיאות)

```bash
# חיפוש רקורסיבי ברמת שורש והסתרת שגיאות גישה
getcap -r / 2>/dev/null
```

* `-r /` — חפש ברקורסיה החל משורש המערכת.
* `2>/dev/null` — מנתב שגיאות (permission denied, וכו') ל־/dev/null כדי לשמור על פלט קריא.

### חיפוש על קובץ מסוים

```bash
getcap /usr/bin/ping
```

פלט לדוגמה אפשרי:

```
/usr/bin/ping = cap_net_raw+ep
```

פירוש: `ping` מחזיק את היכולת `cap_net_raw` עם flags `e` (effective) ו`p` (permitted).

---

## פירוט יכולות נפוצות (דוגמאות)

* `cap_net_raw` — גישה לרשת ברמה נמוכה (raw sockets). שימושי ל־ping, sniffing וכו'.
* `cap_net_bind_service` — אפשרות לקשור פורטים נמוכים (<1024) בלי root.
* `cap_sys_admin` — יכולת רחבה ומסוכנת מאוד (כמעט כמו root בהרבה מקרים).
* `cap_setuid` / `cap_setgid` — שינוי UID/GID של תהליכים.

למידע מלא: `man capabilities`.

---

## מה זה flags כמו `+ep`?

* `e` — effective: היכולת משמשת בזמן הריצה.
* `p` — permitted: היכולת מותרת במסגרת ה־capability set של התהליך.
* `i` — inheritable: היכולת תעבור לצאצאים בתנאים מסוימים.

---

## חיפוש קבצים עם יכולות ועריכה (שימו לב לבטיחות)

להסרת capability מקובץ:

```bash
sudo setcap -r /path/to/binary
```

להוספת capability (לדוגמה):

```bash
sudo setcap cap_net_bind_service+ep /path/to/binary
```

**אזהרה:** אל תשנה capabilities בקבצים מערכתיים ללא בדיקה — זה עלול לשבור פונקציונליות.

---

## חשיבות בבדיקות אבטחה

* קבצים עם capabilities יכולים להיות נקודות כניסה לפריבילגיה אם הם ניתנים להרצה ע"י משתמש לא מוגן או אם הם מאפשרים פעולה אשר יכולה להוביל ל־privilege escalation.
* בדומה ל־SUID — יש להשוות את הרשימת הקבצים עם capabilities אל GTFOBins וכתבי פירוט אחרים.

---

## פקודות עזר שימושיות

* רשימת קבצים עם capability (רק שמות):

```bash
getcap -r / 2>/dev/null | cut -d' ' -f1
```

* בדיקת capabilities של תהליך רץ (בשימוש ב־/proc):

```bash
cat /proc/<pid>/status | grep Cap
```

* בדיקת ה־bounding set של הליבה (מה יכול להיות פעיל):

```bash
cat /proc/sys/kernel/cap_last_cap
# לצפייה ב־capabilities המותרים/מוגבלים יתכן שתצטרך כלים מתקדמים יותר
```

---

## המלצות תיקון והגנה

* הסר capabilities מקבצים במידת האפשר (בעיקר `cap_sys_admin` ויכולות רחבות אחרות).
* אם כלי זקוק ליכולת מסוימת — שקול להריץ אותו בתוך container או sandbox מוגן.
* בצע סריקות תקופתיות: `getcap -r /` ו־audit על ריצות חשודות.
* הדגם למפתחים כיצד להשתמש ביכולות בצורה בטוחה (הסבר על inheritable/effective/permitted).

---

## המשך למידה

* `man capabilities` / `man getcap` / `man setcap`
* מאמרי אבטחה על Linux capabilities וגבולותיהם.

---

**סוף — CAPABILITIES.md**
