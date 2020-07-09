# Szorgalmi házi feladat: szerveroldali programozás T-SQL nyelven platformon

A házi feladat opcionális. A teljesítéssel **2 pluszpont és 2 iMsc pont** szerezhető.

GitHub Classroom segítségével a <TBD> linken keresztül hozz létre egy repository-t. Klónozd le a repository-t. Ez tartalmazni fogja a megoldás elvárt szerkezetét. A feladatok elkészítése után kommitold és pushold a megoldásod.

## Szükséges eszközök

- Microsoft SQL Server
  - Express változat ingyenesen használható, avagy Visual Studio telepítésével a gyakorlatokon is használt "localdb" változat elérhető
- Microsoft SQL Server Management Studio
- Gyakorlatokon is használt minta adatbázis kódja (elérhető a gyakorlatok anyagai között).
  - Előkészületként hozz létre egy új adatbázist, és futtasd le a táblákat létrehozó szkriptet.

## Feladat 0: Neptun kód

Első lépésként a gyökérben található `neptun.txt` fájlba írd bele a Neptun kódodat!

## Feladat 1: Jelszó lejárat és karbantartása (2 pluszpont)

Biztonsági megfontolásból szeretnénk kötelezővé tenni a jelszó időnkénti megújítását. Ehhez rögzíteni fogjuk, és karbantartjuk a jelszó utolsó módosításának idejét.

1. Adj hozzá egy új oszlopot a `Vevo` táblához `JelszoLejarat` néven, ami egy dátumot tartalmaz: `alter table [Vevo] add [JelszoLejarat] datetime`.

1. Készíts egy triggert, amellyel jelszó változtatás esetén automatikusan kitöltésre kerül a `JelszoLejarat` mező értéke. Az új értéke a mai dátum plusz egy év legyen. Az értéket a szerver számítsa ki. Ügyelj arra, hogy új vevő regisztrálásakor (_insert_) mindig kitöltésre kerüljön a mező, viszont a vevő adatainak szerkesztésekor (_update_) csak akkor változzon a lejárat dátuma, ha változott a jelszó. (Tehát pl. ha az email címet változtatták csak, akkor a lejárat ne változzon.)

   > A megoldást az `f1.sql` fájlban add be. Az sql fájl egyetlen utasítást tartalmazzon csak (egyetlen `create trigger`), ne legyen benne se `use` se `go` parancs!

1. Készíts egy képernyőképet (screenshot), amin látszódik

   - a trigger fejlesztéséhez használt eszköz (pl. SQL Server Management Studio),
   - a gép és a felhasználó neve, amin a fejlesztést végezted (pl. SQL Server Management Studio-ban az Object Explorer-ben a megnyitott kapcsolat nevében szerepel, vagy konzolban add ki a `whoami` parancsot és ezt a konzolt is rakd a képernyőképre),
   - az aktuális dátum (pl. az óra a tálcán)
   - valamint a trigger kódja.

   [Itt egy példa](img/img-screenshot-pl-sql.png), körülbelül ilyesmit várunk.

   > A képet `f1.png` néven mentsd el és add be a megoldásod részeként!

## Feladat 2: Termék ajánlott korhatára (2 iMsc pont)

> Az iMsc pont megszerzésére a első feladat megoldásával együtt van lehetőség.

A minta adatbázisban a termékek rekordjaiban van egy xml típusú `leiras` nevű oszlop. Ez néhány terméknél van csak kitöltve.

Egy példa a tartalmára:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<termek>
    <termek_meret>
        <mertekegyseg>cm</mertekegyseg>
        <szelesseg>150</szelesseg>
        <magassag>50</magassag>
        <melyseg>150</melyseg>
    </termek_meret>
    <leiras>Elemmel mukodik, a csomag nem tartalmaz elemet.</leiras>
    <ajanlott_kor>0-18 hónap</ajanlott_kor>
</termek>
```

Szeretnénk az **ajánlott kort** tartalmazó adatot könnyebb elérhetőség végett egy saját oszlopba helyezni.

1. Adj hozzá egy új oszlopot a `Termek` táblához `AjanlottKor` néven, ami egy szöveget tartalmaz tartalmaz: `alter table [Termek] add [AjanlottKor] nvarchar(200)`.

1. Írj T-SQL szkriptet, amely minden termék esetén az xml leírásból az `<ajanlott_kor>` elemet kiemelve feltölti a az előbb létrehozott `AjanlottKor` oszlopot. Ha az xml leírás üres, vagy nincs benne a keresett elem, akkor maradjon `NULL` az új oszlop tartalma. Ellenkező esetben az xml tag szöveges tartalma kerüljön átmásolásra, és az xml dokumentumból töröld ezt az elemet. Feltételezheti, hogy csak egyetlen `<ajanlott_kor>` elem van az xml-ben.

   > A megoldást az `f2.sql` fájlban add be. Ne használj se tárolt eljárást, se triggert, csak egy T-SQL kód blokkot készíts. Az sql fájl önmagában futtatható legyen, ne legyen benne se `use` se `go` parancs!

1. Készíts egy képernyőképet a fent leírtak szerint a szkript kódjával.

   > A képet `f2.png` néven mentsd el és add be a megoldásod részeként!