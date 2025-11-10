---
Po vypracování, odzkoušení všeho, vypracováno Chatem GPT toto shrnutí, jen pro mě, myslím, že netřeba kontrolovat koučem.  
---

## 💻 Praktická část

### Úkol 1: Vytvoření skupin a uživatelů

1. Vytvoř skupiny:
   - `studenti`
   - `ucitele`
   ```bash
   sudo groupadd studenti
   sudo groupadd ucitele
   ```

2. Vytvoř uživatele:
   - `eva` (člen skupiny `studenti`)
   - `adam` (člen skupiny `studenti`)
   - `petr` (člen skupiny `ucitele`)
   ```bash
   sudo useradd -m -G studenti eva
   sudo useradd -m -G studenti adam
   sudo useradd -m -G ucitele petr
   ```

3. Nastav každému uživateli heslo (např. `test123`).
   ```bash
   sudo passwd eva
   sudo passwd adam
   sudo passwd petr
   ```

4. Ověř, do jakých skupin uživatelé patří.
   ```bash
   groups eva
   groups adam
   groups petr
   ```

5. Přidej uživatele `eva` také do skupiny `ucitele`.
   ```bash
   sudo usermod -aG ucitele eva
   groups eva
   ```

6. Smaž uživatele `adam` a skupinu `ucitele`.
   ```bash
   sudo userdel -r adam
   sudo groupdel ucitele
   ```

---

### Úkol 2: Práva a přístup k souborům

1. Přepni se na uživatele `petr`.
   ```bash
   su - petr
   ```

2. V jeho domovské složce vytvoř soubor `poznamka.txt`.
   ```bash
   touch poznamka.txt
   ```

3. Zobraz výpis oprávnění.
   ```bash
   ls -l poznamka.txt
   ```

4. Nastav oprávnění tak, aby:
   - vlastník mohl číst i zapisovat,
   - skupina mohla pouze číst,
   - ostatní neměli žádný přístup.
   ```bash
   chmod 640 poznamka.txt
   ```

5. Ověř, že změna proběhla správně.
   ```bash
   ls -l poznamka.txt
   ```

6. Změň vlastníka souboru na uživatele `eva` a skupinu na `studenti`.
   ```bash
   sudo chown eva:studenti poznamka.txt
   ```

7. Ověř, že soubor má nového vlastníka i skupinu.
   ```bash
   ls -l poznamka.txt
   ```

---

### Úkol 3: Sdílená složka pro skupinu

1. Jako `root` vytvoř složku `/home/spolecne`.
   ```bash
   sudo mkdir /home/spolecne
   ```

2. Nastav vlastníka `root` a skupinu `studenti`.
   ```bash
   sudo chown root:studenti /home/spolecne
   ```

3. Nastav práva tak, aby:
   - vlastník měl práva `rw`,
   - skupina měla práva `rw`,
   - ostatní neměli žádná.
   ```bash
   sudo chmod 660 /home/spolecne
   ```

4. Nastav **setgid bit**, aby nově vytvořené soubory ve složce zdědily skupinu `studenti`:
   ```bash
   chmod g+s /home/spolecne
   ```

5. Ve složce `/home/spolecne` vytvoř soubor `zkouska.txt` jako uživatel `eva`.
   ```bash
   su - eva
   touch /home/spolecne/zkouska.txt
   ```

6. Ověř, že soubor má skupinu `studenti`.
   ```bash
   ls -l /home/spolecne
   ```

7. Přepni se na uživatele `adam` a zkontroluj, že může tento soubor číst i upravovat.
   ```bash
   su - adam
   cat /home/spolecne/zkouska.txt
   echo "test" >> /home/spolecne/zkouska.txt
   ```

---

### Úkol 4: Struktura „Školní tým“

1. Znovu vytvoř skupinu `ucitele`.
   ```bash
   sudo groupadd ucitele
   ```

2. Vytvoř uživatele:
   - `reditel`
   - `ucitelka`
   - `student1`
   - `student2`
   ```bash
   sudo useradd -m reditel
   sudo useradd -m ucitelka
   sudo useradd -m student1
   sudo useradd -m student2
   ```

3. Přiřaď uživatele do skupin:
   - `reditel`, `ucitelka` → `ucitele`
   - `student1`, `student2` → `studenti`
   ```bash
   sudo usermod -aG ucitele reditel
   sudo usermod -aG ucitele ucitelka
   sudo usermod -aG studenti student1
   sudo usermod -aG studenti student2
   ```

4. Vytvoř strukturu složek:
   ```console
   /opt/skola/
     ├── ucitele/
     └── studenti/
   ```
   ```bash
   sudo mkdir -p /opt/skola/ucitele
   sudo mkdir -p /opt/skola/studenti
   ```

5. Nastav:
   - `/opt/skola/ucitele` → vlastník `root`, skupina `ucitele`, práva `770`
   - `/opt/skola/studenti` → vlastník `root`, skupina `studenti`, práva `770`
   ```bash
   sudo chown root:ucitele /opt/skola/ucitele
   sudo chown root:studenti /opt/skola/studenti
   sudo chmod 770 /opt/skola/ucitele
   sudo chmod 770 /opt/skola/studenti
   ```

6. Do obou složek přidej soubor `info.txt` s krátkým textem *(např. „Vítejte ve složce učitelů“)*.
   ```bash
   echo "Vítejte ve složce učitelů" | sudo tee /opt/skola/ucitele/info.txt
   echo "Vítejte ve složce studentů" | sudo tee /opt/skola/studenti/info.txt
   ```

7. Ověř, že:
   - Učitelé mají přístup pouze do složky `ucitele`.
   - Studenti pouze do `studenti`.
   - Ostatní uživatelé nemají přístup do žádné z nich.
   ```bash
   su - reditel
   cd /opt/skola/ucitele  # funguje
   cd /opt/skola/studenti # Permission denied
   ```

---

## Bonus: Sudo zapomíná

Co by měl dělat tento příkaz?

```console
ll /var/db/sudo/lectured/
```

👉 Zobrazí soubory, které si sudo ukládá pro každého uživatele – informaci o tom, že už viděl varování („sudo lecture“).

Příkaz ale pod běžným uživatelským účtem nefunguje; potřebuješ `sudo ll /var/db/sudo/lectured/`.  
To ale taky nefunguje, protože `ll` není skutečný příkaz, ale alias, který není dostupný pro root.  

✅ Použij místo toho:
```bash
sudo ls -l /var/db/sudo/lectured/
```

* Jaké soubory jsou v `/var/db/sudo/lectured/`? Komu patří? Jak jsou velké?  
  → Každý soubor odpovídá jednomu uživateli (název podle UID). Patří rootovi, mají pár bajtů, práva `-rw-------`.

* Jaké soubory jsou v `/var/run/sudo/ts`? Komu patří? Jak jsou velké? Kdo k nim má jaká práva?  
  → Jsou to timestampy – sudo si tu pamatuje, že jsi zadala heslo.  
    Každý soubor patří rootovi, pojmenovaný podle UID, malý soubor, přístup jen root.

* Jak by vypadal příkaz, který smaže „tvůj“ soubor v `/var/run/sudo/ts`?
  ```bash
  sudo rm /var/run/sudo/ts/$(id -u)
  ```

Když spustíš příkaz `sudo`, zeptá se tě na heslo. Když ho spustíš podruhé (ve stejném terminálu), už se neptá – pamatuje si, že jsi heslo před chvilkou zadala.

Vyzkoušej si ale, že tohle nefunguje s příkazem pro smazání „tvého“ souboru v `/var/run/sudo/ts`. Když ho pustíš několikrát za sebou, `sudo` se vždycky znovu zeptá na heslo.

* Proč?  
  → Protože mazáním souboru timestamp (`/var/run/sudo/ts`) mažeš právě tu informaci, že jsi heslo už zadala.  
    Sudo tedy pokaždé zapomene, že ses ověřila.

* Zadej tyhle příkazy:
  * Smaž „svůj“ soubor z `/var/db/sudo/lectured/`.
    ```bash
    sudo rm /var/db/sudo/lectured/$(id -u)
    ```
  * Smaž „svůj“ soubor z `/var/run/sudo/ts`.
    ```bash
    sudo rm /var/run/sudo/ts/$(id -u)
    ```
  * Spusť `sudo echo`.
    ```bash
    sudo echo
    ```

  A odpověz:
  * Co je u `sudo echo` jinak?  
    → Znovu se zobrazí výstraha („lecture“) a sudo se ptá na heslo. Lucie-edit: Tady jsem to pokazila. Nestalo se vůbec nic. Protože lecture se zobrazí až při dalším zadávání suda, což jsem hned dělat nemusela, protože jsem nesmazala ts. A možná by bylo fajn tady použít sudo true (místo sudo echo) a seznámit tak na ideálním místě nováčka s tímhle příkazem. 

  * K čemu slouží soubory ve `/var/db/sudo/lectured/`?  
    → Ukládají informaci, že uživatel už jednou viděl výstražnou hlášku sudo – aby se znovu nezobrazovala.
