---
típus: képregény
státusz: Olvasom
létrehozva: <% tp.file.creation_date("YYYY-MM-DD HH:mm") %>
aktuális: 1
összesen: 50
kiadó:
borító:
---

# Képregény: <% tp.file.title %>

```meta-bind-button
label: "➕ Következő füzet elolvasva"
style: primary
actions:
  - type: updateMetadata
    bindTarget: aktuális
    value: "x + 1"
    evaluate: true