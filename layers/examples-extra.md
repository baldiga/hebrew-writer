# Additional Before/After Examples

## Example 2: English Brief → Generated Hebrew Article

**English input:**

> Write about why Israeli startups succeed disproportionately. Mention military background, culture of directness, failure tolerance.

**Generated Hebrew:**

> יש שאלה שחוזרת בכל כנס הייטק שהייתי בו: למה ישראל? מדינה של תשעה מיליון איש, בלי שוק פנים אמיתי, עם בירוקרטיה שמסוגלת לרסק רוח יזמית — ובכל זאת. ובכל זאת יש פה יותר חברות נסחרות בנאסד"ק לנפש מאשר בכל מדינה אחרת בעולם.
>
> ניסיתי להסביר את זה לחבר גרמני שבא לבקר. הוא שאל: "אבל איך אתם יודעים לקחת סיכונים?" ואני הסתכלתי עליו ולא הבנתי את השאלה. אחר כך הבנתי שהוא לא מבין שבישראל, כישלון הוא לא קצה קריירה. הוא פרק.
>
> הצבא מלמד דבר שאי אפשר ללמד בכיתה: שאתה יכול לטעות בגדול, לקבל פידבק ישיר מאוד על הטעות שלך — לעיתים מאוד ישיר — ולהמשיך. מי שיצא משלוש שנות שירות מחוזק, לא שבור, הוא בן אדם שיודע שהוא שורד.
>
> וגם הדוגריות. זה לא סתם מילה — זו שיטת עבודה. ישיבה שבה כולם מנומסים ואיש לא אומר מה הוא חושב? זה לא ישיבה ישראלית. אנחנו נגיד לך שהרעיון שלך גרוע. ויש לזה ערך: כי אם אנחנו לא נגיד לך, השוק יגיד לך, ואתה תגלה את זה מאוחר מדי.

*(Generated from nothing but the brief. Grounded detail: חבר גרמני, נאסד"ק. Opening: question. Discourse markers: absent — content type is semi-formal blog, 1-2% target appropriate. Self-correction visible: "לא הבנתי את השאלה. אחר כך הבנתי." Dugri closing.)*

---



## Example 3: Formal AI Hebrew → Casual Israeli Rewrite

**Before (formal AI Hebrew):**

> בהתאם לממצאי המחקר, על מנת לייעל את תהליך קבלת ההחלטות בארגון, מומלץ לאמץ גישה שיטתית המשלבת ניתוח כמותי ואיכותי. יתר על כן, חשוב להקפיד על שיתוף הגורמים הרלוונטיים בתהליך, כפי שמציינים מומחים בתחום, על מנת להבטיח יישום מיטבי של ההחלטות.

*(Patterns: בהתאם ל, על מנת, מומלץ, יתר על כן, הגורמים הרלוונטיים, מומחים בתחום, מיטבי — classic AI formal Hebrew)*

**After (casual Israeli):**

> בואו נדבר דוגרי: רוב הישיבות בארגונים נגמרות בלי החלטה כי אף אחד לא בא עם נתונים, ואלה שבאו עם נתונים לא שמעו אחד את השני.
>
> הדבר שעובד בפועל: לפני כל ישיבת החלטה, מישהו אחד אחראי להכין שלושה מצגים בני שלושה שקפים — מה אנחנו יודעים, מה אנחנו לא יודעים, ומה אנחנו צריכים להחליט היום. לא יותר. אם המצב יותר מורכב מזה, כנראה שעדיין לא הגעתם לשאלה הנכונה.
>
> ועוד דבר: מי שיושב בחדר ולא מבין על מה מחליטים — חייב לשאול. לא "כדי לא להיות בורה" אלא כי הוא כנראה לא לבד.

*(Fresh composition. No shared sentence structure with the original. Same core meaning — structured decision-making process. Register: 3/10 casual. Discourse markers: בואו נדבר דוגרי as opener. Cultural texture: ישיבות, practical Israeli directness.)*

---



## Example 4: Detection Report on AI Text

**Input text being scanned:**

> "הטכנולוגיה החדשנית והמתקדמת מהווה גורם מכריע בהתפתחות הכלכלה הדיגיטלית. מחקרים מראים כי חברות המאמצות פתרונות מקיפים וייחודיים מצליחות להשיג יתרון תחרותי משמעותי. יתר על כן, חשוב לציין כי ניהול נכון של המשאבים הדיגיטליים מהווה חלק בלתי נפרד מהצלחה ארגונית בעולם המשתנה של ימינו."

**Detection report:**

```
## דוח זיהוי AI / AI Detection Report

**ציון כולל:** 18/100 (18% human-like)
This text hits nearly every item on the Hebrew AI vocabulary blacklist and uses five structural
AI patterns in three sentences. It will score near 100% AI on any commercial detector.

---

### תבניות שזוהו / Detected Patterns

| # | תבנית | מיקום | חומרה |
|---|-------|--------|-------|
| 1 | AI vocabulary: חדשנית | "הטכנולוגיה החדשנית" | גבוה |
| 2 | AI vocabulary: מקיפים | "פתרונות מקיפים" | גבוה |
| 3 | AI vocabulary: ייחודיים | "ייחודיים" | גבוה |
| 4 | AI vocabulary: משמעותי | "יתרון תחרותי משמעותי" | גבוה |
| 5 | Copula stuffing: מהווה | "מהווה גורם מכריע" | גבוה |
| 6 | Copula stuffing: מהווה | "מהווה חלק בלתי נפרד" | גבוה |
| 7 | Vague attribution | "מחקרים מראים כי" | גבוה |
| 8 | Banned transition: יתר על כן | "יתר על כן, חשוב לציין" | בינוני |
| 9 | Significance inflation | "בעולם המשתנה של ימינו" | בינוני |
| 10 | Uniform sentence length | All 3 sentences: 18-22 words | בינוני |
| 11 | Missing discourse markers | Zero informal markers in otherwise casual framing | נמוך |
| 12 | Over-consistent formality | בשל כך register throughout — no variation | נמוך |

---

### ציוני מימדים / Dimension Scores

| מימד | ציון | הערה |
|------|------|------|
| דוגריות | 2/10 | No position taken; describes without claiming anything |
| קצב | 3/10 | Three sentences of nearly identical length |
| אמינות | 1/10 | No Israeli could be imagined behind this text |
| טקסטורה | 2/10 | No slang, no variation, no personality on the surface |
| נשמה | 1/10 | No emotion, no opinion, no human presence |
| צפיפות | 4/10 | Ideas present but buried in filler phrases |
| רישום | 4/10 | Register is internally consistent, just consistently wrong |
| אנטי-זיהוי | 1/10 | 12 patterns detected; will fail every commercial detector |

---

### המלצות / Recommendations

**עדיפות גבוהה:**
1. Replace all 4 blacklist vocabulary words with specific descriptions or remove them
2. Eliminate both מהווה constructions — use nominal sentences or direct verbs instead
3. Replace "מחקרים מראים כי" with either a specific source or a first-person claim

**עדיפות בינונית:**
4. Replace יתר על כן with גם or ועוד
5. Break sentence length uniformity — add a short fragment somewhere

**נמוך:**
6. Add one grounded detail — something specific, not abstract

To automatically fix: run `/hebrew-writer "[paste text]" --mode rewrite`
```

---
