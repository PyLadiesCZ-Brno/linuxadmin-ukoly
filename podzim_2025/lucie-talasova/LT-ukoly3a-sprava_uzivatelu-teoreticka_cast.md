# Úkoly – Správa uživatelů, Sudo

## 🧠 Teoretická část

# Domácí úkol – Linux: práce se soubory, uživateli a právy

## A. Práce se soubory a výstupem

1. **Co udělá `cat` a kdy je vhodné ho použít?**  
   `cat` vypíše obsah souboru do terminálu, případně spojí víc souborů za sebe. Výstup lze přesměrovat i do souboru. Ideální pro kratší soubory nebo spojování. 

2. **Rozdíl mezi `less` a `cat`**  
   `cat` jen vypíše celý obsah a hotovo. `less` vypíše obsah souboru tak, že se dá v něm posunovat po obrazovkách pomocí šipek /`PgUp`/`PgDn` a hledat (`/text`). Lepší pro delší soubory.

3. **K čemu slouží `head` a jak určit počet řádků?**  
   `head` vypíše začátek souboru (automaticky 10 řádků). Počet řádků lze nastavit parametrem `-n`, např. `head -n 25 soubor.txt`.

4. **Jak zjistit počet řádků nebo slov v souboru?**  
   Pomocí `wc` (word count):  
   - řádky: `wc -l soubor.txt`  
   - slova: `wc -w soubor.txt`  
   - znaky: `wc -m soubor.txt`  
   - vše najednou: `wc soubor.txt`

5. **Jak zobrazit posledních 10 řádků souboru?**  
   `tail soubor.txt` (defaultně 10 řádků). Pro jiný počet: `tail -n 50 soubor.txt`. 

---

## B. Uživatelé, skupiny a práva

1. **Rozdíl mezi uživatelem a skupinou v Linuxu**  
   Uživatel = účet (UID) s domovským adresářem a shell konfigurací. Skupina = logické seskupení uživatelů (GID) pro sdílení oprávnění. Uživatel může být členem více skupin; každá entita má primární skupinu.

2. **Význam `-rw-r--r--`**  
   První znak typ: `-` soubor (`d` složka, `l` symlink).
   Pak práva uživatele - číst a zapisovat, práva skupiny - číst, práva všech ostatních - číst. 

3. **Rozdíl mezi vlastníkem souboru a skupinou souboru**  
   Vlastník (user) je jeden konkrétní účet, který soubor vytvořil (nebo mu byl přiřazen); skupina je jedna vybraná skupina, jejíž členové mohou mít další práva k souboru (klidně i stejná, či dokonce vyšší než uživatel).

4. **Co obsahují `/etc/passwd` a `/etc/shadow`**  
    Etc je složka, ve které se nachází konfigurace. Není ve domovském adresáři; je to top-level vedle /home, /bin, /var, …
   - `/etc/passwd`: základní údaje o účtech (login, UID, GID, domovský adresář, shell). Dnes neobsahuje hesla, ale placeholder `x`.  
   - `/etc/shadow`: hashovaná hesla a politiky (expirace apod.); přístup má jen root (kvůli bezpečnosti).

5. **K čemu slouží `chmod` a jak se používá**  
   Mění přístupová práva. Dva způsoby:  
   - Symbolicky: `chmod u+rwx,g+rx,o-r soubor`                   
   - Osmiciferně (čísly): `chmod 754 soubor` 

6. **K čemu slouží `useradd`, `groupadd`, `usermod`, `passwd`, `userdel`, `groupdel`**  
   - `useradd`: vytvoří nový účet (např. `sudo useradd -m -s /bin/bash jenda`).  
   - `groupadd`: vytvoří novou skupinu (např. `sudo groupadd editors`).  
   - `usermod`: změní vlastnosti účtu (např. přidání do skupin `sudo usermod -aG editors jenda`, změna shellu `-s`, domova `-d` …).  
   - `passwd`: nastaví/změní heslo uživatele (`sudo passwd jenda`).  
   - `userdel`: smaže účet (s domovem `-r`: `sudo userdel -r jenda`).  
   - `groupdel`: smaže skupinu (pokud už není primární pro nějaký účet).
