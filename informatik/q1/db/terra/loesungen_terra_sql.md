# SQL Übungen — Terra Datenbank: Lösungen

## Niveau 0 — Einstieg (SELECT & WHERE)

### 0-1: Alle Berge anzeigen
```sql
SELECT * FROM BERG;
```

### 0-2: Alle Länder
```sql
SELECT L_NAME, HAUPTSTADT FROM LAND;
```

### 0-3: Wo liegt Berlin?
```sql
SELECT ST_NAME, BREITE, LAENGE FROM STADT WHERE ST_NAME = 'Berlin';
```

### 0-4: Top 5 höchste Berge
```sql
SELECT B_NAME, HOEHE FROM BERG ORDER BY HOEHE DESC LIMIT 5;
```

### 0-5: Achttausender
```sql
SELECT B_NAME, HOEHE FROM BERG WHERE HOEHE > 8000 ORDER BY HOEHE DESC;
```

### 0-6: Berge im Himalaya
```sql
SELECT * FROM BERG WHERE GEBIRGE = 'Himalaya';
```

### 0-7: Mount-Berge
```sql
SELECT B_NAME, HOEHE FROM BERG WHERE B_NAME LIKE 'Mount%';
```

### 0-8: Große Länder
```sql
SELECT L_NAME, EINWOHNER FROM LAND WHERE EINWOHNER > 100000000 ORDER BY EINWOHNER DESC;
```

### 0-9: Lange Flüsse
```sql
SELECT F_NAME, LAENGE FROM FLUSS WHERE LAENGE > 5000 ORDER BY LAENGE DESC;
```

### 0-10: Städte in Deutschland
```sql
SELECT ST_NAME, EINWOHNER FROM STADT WHERE L_ID = 'D' ORDER BY EINWOHNER DESC;
```

### 0-11: Millionenstädte
```sql
SELECT ST_NAME, EINWOHNER FROM STADT WHERE EINWOHNER > 5000000 ORDER BY EINWOHNER DESC;
```

### 0-12: Sandwüsten
```sql
SELECT W_NAME, FLAECHE, WUESTENART FROM WUESTE WHERE WUESTENART = 'Sand';
```

### 0-13: Große Inseln
```sql
SELECT I_NAME, FLAECHE FROM INSEL WHERE FLAECHE > 100000 ORDER BY FLAECHE DESC;
```

### 0-14: Tiefe Meere
```sql
SELECT M_NAME, TIEFE FROM MEER ORDER BY TIEFE DESC;
```

### 0-15: Hauptstadt mit A
```sql
SELECT L_NAME, HAUPTSTADT FROM LAND WHERE HAUPTSTADT LIKE 'A%';
```

### 0-16: Berge zwischen 4000 und 5000m
```sql
SELECT B_NAME, HOEHE FROM BERG WHERE HOEHE BETWEEN 4000 AND 5000;
```

### 0-17: Alpen oder Anden
```sql
SELECT B_NAME, GEBIRGE, HOEHE FROM BERG WHERE GEBIRGE IN ('Alpen', 'Anden');
```

### 0-18: Seen mit großer Fläche
```sql
SELECT S_NAME, FLAECHE FROM SEE WHERE FLAECHE > 10000 ORDER BY FLAECHE DESC;
```

---

## Niveau 1 — Aggregation

### 1-1: Anzahl Länder
```sql
SELECT COUNT(*) FROM LAND;
```

### 1-2: Größtes Land
```sql
SELECT L_NAME, FLAECHE FROM LAND ORDER BY FLAECHE DESC LIMIT 1;
```

### 1-3: Bevölkerungsreichstes Land
```sql
SELECT L_NAME, EINWOHNER FROM LAND ORDER BY EINWOHNER DESC LIMIT 1;
```

### 1-4: Längster Fluss
```sql
SELECT F_NAME, LAENGE FROM FLUSS ORDER BY LAENGE DESC LIMIT 1;
```

### 1-5: Anzahl Gebirge
```sql
SELECT COUNT(DISTINCT GEBIRGE) FROM BERG;
```

### 1-6: Durchschnittliche Berghöhe
```sql
SELECT AVG(HOEHE) FROM BERG;
```

### 1-7: Anzahl Städte
```sql
SELECT COUNT(*) FROM STADT;
```

### 1-8: Tiefster See
```sql
SELECT S_NAME, TIEFE FROM SEE ORDER BY TIEFE DESC LIMIT 1;
```

### 1-9: Größte Insel
```sql
SELECT I_NAME, FLAECHE FROM INSEL ORDER BY FLAECHE DESC LIMIT 1;
```

### 1-10: Tiefstes Meer
```sql
SELECT M_NAME, TIEFE FROM MEER ORDER BY TIEFE DESC LIMIT 1;
```

### 1-11: Anzahl Wüsten
```sql
SELECT COUNT(*) FROM WUESTE;
```

---

## Niveau 2 — GROUP BY & HAVING

### 2-1: Berge pro Gebirge
```sql
SELECT GEBIRGE, COUNT(*) AS Anzahl
FROM BERG
GROUP BY GEBIRGE
ORDER BY Anzahl DESC;
```

### 2-2: Gebirge mit >10 Bergen
```sql
SELECT GEBIRGE, COUNT(*) AS Anzahl
FROM BERG
GROUP BY GEBIRGE
HAVING COUNT(*) > 10;
```

### 2-3: Durchschnitt pro Gebirge
```sql
SELECT GEBIRGE, ROUND(AVG(HOEHE), 0) AS Durchschnitt
FROM BERG
GROUP BY GEBIRGE
ORDER BY Durchschnitt DESC;
```

### 2-4: Max/Min pro Gebirge
```sql
SELECT GEBIRGE, MAX(HOEHE) AS Hoechster, MIN(HOEHE) AS Niedrigster
FROM BERG
GROUP BY GEBIRGE;
```

### 2-5: Wüstenarten
```sql
SELECT WUESTENART, COUNT(*) AS Anzahl
FROM WUESTE
GROUP BY WUESTENART
ORDER BY Anzahl DESC;
```

### 2-6: Länder pro Kontinent
```sql
SELECT K_NAME, COUNT(*) AS Anzahl
FROM UMFASST
GROUP BY K_NAME
ORDER BY Anzahl DESC;
```

### 2-7: Inseln pro Inselgruppe
```sql
SELECT INSELGRUPPE, COUNT(*) AS Anzahl
FROM INSEL
GROUP BY INSELGRUPPE
HAVING COUNT(*) > 1
ORDER BY Anzahl DESC;
```

---

## Niveau 3a — Einfache JOINs (2 Tabellen: STADT ↔ LAND)

> STADT und LAND teilen sich die Spalte `L_ID` — hier braucht man keine Verbindungstabelle.

### 3a-1: Städte mit Ländername
```sql
SELECT s.ST_NAME, l.L_NAME
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID;
```

### 3a-2: Deutsche Städte mit Land
```sql
SELECT s.ST_NAME, s.EINWOHNER, l.L_NAME
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
WHERE s.L_ID = 'D'
ORDER BY s.EINWOHNER DESC;
```

### 3a-3: Top 10 größte Städte + Land
```sql
SELECT s.ST_NAME, s.EINWOHNER, l.L_NAME
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
ORDER BY s.EINWOHNER DESC
LIMIT 10;
```

### 3a-4: Millionenstädte mit Land
```sql
SELECT s.ST_NAME, s.EINWOHNER, l.L_NAME
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
WHERE s.EINWOHNER > 5000000
ORDER BY s.EINWOHNER DESC;
```

### 3a-5: Städte in großen Ländern
```sql
SELECT s.ST_NAME, l.L_NAME, l.EINWOHNER
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
WHERE l.EINWOHNER > 100000000
ORDER BY l.EINWOHNER DESC;
```

### 3a-6: Städte in Riesenländern
```sql
SELECT s.ST_NAME, l.L_NAME, l.FLAECHE
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
WHERE l.FLAECHE > 5000000;
```

### 3a-7: Anzahl Städte pro Land
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
GROUP BY l.L_NAME
ORDER BY Anzahl DESC;
```

### 3a-8: Länder mit >5 Städten
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
GROUP BY l.L_NAME
HAVING COUNT(*) > 5
ORDER BY Anzahl DESC;
```

### 3a-9: Durchschnittliche Stadtgröße pro Land
```sql
SELECT l.L_NAME, ROUND(AVG(s.EINWOHNER), 0) AS Durchschnitt
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
GROUP BY l.L_NAME
ORDER BY Durchschnitt DESC;
```

### 3a-10: Land mit den meisten Städten
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM STADT s
JOIN LAND l ON s.L_ID = l.L_ID
GROUP BY l.L_NAME
ORDER BY Anzahl DESC
LIMIT 1;
```

---

## Niveau 3b — JOINs mit Verbindungstabellen (3 Tabellen)

> BERG, FLUSS, SEE usw. haben keine direkte `L_ID` — der Umweg geht über Verbindungstabellen wie GEO_BERG, GEO_FLUSS, GEO_SEE.

### 3b-1: Top 10 Berge + Länder
```sql
SELECT b.B_NAME, b.HOEHE, l.L_NAME
FROM BERG b
JOIN GEO_BERG gb ON b.B_NAME = gb.B_NAME
JOIN LAND l ON gb.L_ID = l.L_ID
ORDER BY b.HOEHE DESC
LIMIT 10;
```

### 3b-2: Länder mit >6000m Bergen
```sql
SELECT DISTINCT l.L_NAME
FROM BERG b
JOIN GEO_BERG gb ON b.B_NAME = gb.B_NAME
JOIN LAND l ON gb.L_ID = l.L_ID
WHERE b.HOEHE > 6000;
```

### 3b-3: Berge in Deutschland
```sql
SELECT b.B_NAME, b.HOEHE, b.GEBIRGE
FROM BERG b
JOIN GEO_BERG gb ON b.B_NAME = gb.B_NAME
WHERE gb.L_ID = 'D';
```

### 3b-4: Flüsse in Frankreich
```sql
SELECT f.F_NAME, f.LAENGE
FROM FLUSS f
JOIN GEO_FLUSS gf ON f.F_NAME = gf.F_NAME
WHERE gf.L_ID = 'F'
ORDER BY f.LAENGE DESC;
```

### 3b-5: Seen in Russland
```sql
SELECT s.S_NAME, s.TIEFE
FROM SEE s
JOIN GEO_SEE gs ON s.S_NAME = gs.S_NAME
WHERE gs.L_ID = 'RUS'
ORDER BY s.TIEFE DESC;
```

### 3b-6: Land mit den meisten Bergen
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
GROUP BY l.L_NAME
ORDER BY Anzahl DESC
LIMIT 1;
```

### 3b-7: Städte am Rhein
```sql
SELECT DISTINCT la.ST_NAME
FROM LIEGT_AN la
WHERE la.F_NAME = 'Rhein';
```

### 3b-8: Länder + Berganzahl
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
GROUP BY l.L_NAME
HAVING COUNT(*) > 0
ORDER BY Anzahl DESC;
```

---

## Niveau 4 — Kombination

### 4-1: Anzahl Länder >5000m
```sql
SELECT COUNT(DISTINCT l.L_ID)
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
JOIN BERG b ON gb.B_NAME = b.B_NAME
WHERE b.HOEHE > 5000;
```

### 4-2: Höchster Durchschnitt
```sql
SELECT l.L_NAME, ROUND(AVG(b.HOEHE), 0) AS Durchschnitt
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
JOIN BERG b ON gb.B_NAME = b.B_NAME
GROUP BY l.L_NAME
ORDER BY Durchschnitt DESC
LIMIT 1;
```

### 4-3: Länderstatistik
```sql
SELECT l.L_NAME,
       COUNT(b.B_NAME) AS Anzahl,
       ROUND(AVG(b.HOEHE), 0) AS Durchschnitt
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
JOIN BERG b ON gb.B_NAME = b.B_NAME
GROUP BY l.L_NAME
ORDER BY Durchschnitt DESC;
```

### 4-4: Bergstarke Länder
```sql
SELECT l.L_NAME,
       COUNT(*) AS Anzahl,
       ROUND(AVG(b.HOEHE), 0) AS Durchschnitt
FROM LAND l
JOIN GEO_BERG gb ON l.L_ID = gb.L_ID
JOIN BERG b ON gb.B_NAME = b.B_NAME
GROUP BY l.L_NAME
HAVING COUNT(*) > 5 AND AVG(b.HOEHE) > 3000;
```

### 4-5: Flüsse pro Land
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM LAND l
JOIN GEO_FLUSS gf ON l.L_ID = gf.L_ID
GROUP BY l.L_NAME
ORDER BY Anzahl DESC
LIMIT 10;
```

### 4-6: Europäische Berge
```sql
SELECT b.B_NAME, b.HOEHE, l.L_NAME
FROM BERG b
JOIN GEO_BERG gb ON b.B_NAME = gb.B_NAME
JOIN LAND l ON gb.L_ID = l.L_ID
JOIN UMFASST u ON l.L_ID = u.L_ID
WHERE u.K_NAME = 'Europa'
ORDER BY b.HOEHE DESC;
```

### 4-7: Seen pro Land
```sql
SELECT l.L_NAME, COUNT(*) AS Anzahl
FROM LAND l
JOIN GEO_SEE gs ON l.L_ID = gs.L_ID
GROUP BY l.L_NAME
ORDER BY Anzahl DESC
LIMIT 5;
```
