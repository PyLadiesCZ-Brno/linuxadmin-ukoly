# Úkoly - Git

## Teoretické otázky

## 1. Proč je vhodné nastavit uživatelské jméno a e-mail hned po instalaci?
Když se spolupracuje na projektu, je nutné, aby bylo vidět, kdo změny udělal. Když to nenastavím hned, mohlo by se stát, že na to zapomenu, a nebude pak zřejmé, kdo je autorem změn. 

## 2. Jaký je rozdíl mezi pracovním adresářem, indexem (staging area) a repozitářem?
- **Pracovní adresář** je místo, kde fyzicky vytvářím a upravuju svoje soubory.
- **Index (staging area)** je mezikrok, kam se soubory přidávají pomocí `git add`, než se uloží do historie GITu.
- **Repozitář** obsahuje historii všech commitů, tedy všech uložených verzí projektu.

## 3. Co se děje při příkazu `git add` a co při `git commit`?
- `git add` přidá změny z mého pracovního adresáře do indexu, tj. připraví je ke commitu.
- `git commit`  všechny změny z indexu commitne do historie repozitáře GITU jako jeden commit.

## 4. Vysvětli, co je to commit hash a proč je důležitý.
Je to unikátní identifikátor každého commitu - **hash** (např. `a4f6b2d...`). Lze podle něj commit dohledávat a upravovat. 

## 5. Jak Git uchovává historii změn? Uveď rozdíl oproti klasickému ukládání souborů.
Git neukládá celé soubory při každé změně, ale ukládá jn **rozdíly (dify)** mezi verzemi. Díky tomu je efektivní a rychlý. Klasické zálohování by při každé změně vytvářelo celou kopii souboru.

## 6. Co znamená, že Git je „distribuovaný systém pro správu verzí“?
Každý uživatel má kompletní kopii celého repozitáře, včetně historie. To znamená, že může pracovat offline, dělat commity a prohlížet historii i bez připojení k internetu. Centrální server (většinou GitHub) slouží k synchronizaci změn, které jednotliví spolupracovníci udělali. 

## 7. Proč je doporučeno používat větve místo práce přímo v hlavní větvi (main/master)?
Větve umožňují pracovat na projektu bez ovlivnění stabilní hlavní verze. Teprve až když je práce hotová a zkontrolovaná, sloučí se do hlavní větve. Díky tomu se minimalizují chyby a chaos.

## 8. Jaký je rozdíl mezi `git merge` a `git rebase`? Uveď příklad, kdy bys použil/a který.
- **Merge** spojí dvě větve dohromady a zachová historii tak, jak byla – vytvoří nový „merge commit“.
- **Rebase** přepíše historii tak, že změny z jedné větve „přehraje“ na konec jiné větve.

**Kdy použít:**
- `merge` – když chci zachovat úplnou historii (např. při sloučení feature větve do mainu).
- `rebase` – když chci historii zpřehlednit (např. před odesláním na GitHub).

**Rozdíl v historii:**
- Merge přidá nový commit, který spojuje obě větve.
- Rebase vytvoří nové commity s přepsanou historií, takže působí, jako by změny vznikly přímo na mainu.

## 9. Jaký je účel pull requestu a proč se používá?
Pull request slouží k návrhu sloučení změn z jedné (pracovní) větve do jiné (main). Umožňuje ostatním členům týmu zkontrolovat kód, diskutovat o něm a schválit ho před začleněním do hlavní větve.

## 10. Co znamená code review a jaký je jeho přínos?
Code review je proces, kdy někdo jiný než autor projde změny v kódu a zhodnotí je. Pomáhá odhalit chyby, zlepšuje kvalitu kódu a zároveň slouží jako forma učení se mezi vývojáři.

## 11. K čemu je soubor `.gitignore`?
Slouží k určení, které soubory nebo adresáře má Git ignorovat, tj. nezahrnovat do verzovacího systému. Typicky logy, dočasné soubory, konfigurace prostředí apod.

## 12. Co se stane, pokud přidáš do `.gitignore` soubor, který už je ve verzovací historii?
Git ho ignorovat nezačne automaticky, už je totiž sledovaný. Musím ho nejdřív odstranit z repozitáře pomocí `git rm --cached <soubor>` a teprve pak se začne ignorovat.

## 13. Proč je vhodné ignorovat logy, dočasné soubory editorů nebo sestavení?
Tyto soubory se mění často, ale nejsou pro projekt důležité. Zabíraly by místo, dělaly nepořádek v historii a mohly by obsahovat citlivé nebo zbytečné informace.

### 14. Jak se zapisují vzory do `.gitignore`? Uveď příklady pro:
- **Ignorování všech `.log` souborů:**
  ```
  *.log
  ```
- **Ignorování adresáře `build`:**
  ```
  /build/
  ```