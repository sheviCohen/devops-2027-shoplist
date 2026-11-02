# תשובות - שיעור 4

1. ה-stats מגיע ל-backend לפי שם ה-service (http://backend:5000) דרך הרשת ש-Compose יוצר. הדפדפן מגיע אליו דרך Nginx: בקשה ל-localhost:8080/api/ מועברת ל-backend.

2. אחרי down ו-up המוצרים נשארו, כי postgres-data הוא named volume והוא לא נמחק ב-down. עם down -v ה-volume נמחק והמוצרים נעלמים.

3. זה bind mount: הצד השמאלי הוא נתיב בתיקייה במחשב (../sql/init.sql). שינוי ב-init.sql אחרי שהמסד נוצר לא ישפיע, כי סקריפטי האתחול רצים רק בפעם הראשונה, כשהמסד ריק. צריך down -v.

4. הרשת נקראת docker_default וה-volume נקרא docker_postgres-data. השמות מורכבים משם הפרויקט (שם התיקייה docker) ועוד שם הרשת או ה-volume.

5. ב-image הסופי יש Nginx, קובצי האתר מ-dist ו-nginx.conf. אין בו Node, build.js וקבצי המקור. אין CMD כי הוא עובר בירושה מה-image של Nginx. ל-stats לא צריך multi-stage כי אין לו שלב בנייה: הוא רק מריץ node server.js.

6. single: 92.7MB, multi: 92.7MB, kitchen: 244MB. multi כמעט בגודל של single כי בשניהם יש Nginx וקבצים סטטיים קטנים. הכלים של Node נשארים בשלב הבנייה ולא נכנסים ל-image הסופי.

7. ב-Nginx ה-location עם ההתאמה הארוכה והספציפית ביותר מנצח. /api/stats ארוך מ-/api/, ולכן מגיע ל-stats. הלוכסן בסוף proxy_pass מחליף את /api/ ב-/, כך ש-/api/products מגיע ל-backend כ-/products.