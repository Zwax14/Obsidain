# 📖 Olvasási lista 
---
```dataview
TABLE WITHOUT ID 
    ("![](" + borító + ")") AS "Borító", 
    file.link AS "Cím", 
    státusz AS "Státusz", 
    "<progress value='" + aktuális + "' max='" + összesen + "'></progress> " + round((aktuális / összesen) * 100) + "%" AS "Haladás"
FROM ""
WHERE típus = "képregény" 
    AND file.name != "Képregény Sablon"