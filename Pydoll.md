# Pydoll: Modernus Naršyklių Automatizavimas be WebDriver filtrų

Šis straipsnis apžvelgia **Pydoll** – atvirojo kodo Python biblioteką, skirtą efektyviam duomenų rinkimui ir testavimui, naudojant tiesioginį ryšį su naršykle.

---

## 1. Kas yra Pydoll?

**Pydoll** yra naujos kartos automatizavimo įrankis, kuris atsisako tradicinių „WebDriver“ (tokių kaip ChromeDriver) ir naudoja **Chrome DevTools Protocol (CDP)**. Tai leidžia valdyti naršyklę „iš vidaus“, išvengiant daugelio anti-bot sistemų aptinkamų pėdsakų.

### Pagrindiniai privalumai:
* **Asinchroniškumas:** Sukurta naudojant `asyncio`, todėl vienu metu galima valdyti daugybę skirtukų.
* **Nematomumas:** Kadangi nenaudojamas WebDriver, svetainėms sunkiau atpažinti automatizuotą srautą.
* **Žmogiška elgsena:** Imituojami natūralūs pelės judesiai ir klaviatūros paspaudimai.

---

## 2. Praktinis pritaikymas: Prisijungimas ir CSV eksportas

Žemiau pateikiamas pavyzdys, kaip automatiškai prisijungti prie sistemos ir surinkti duomenis į failą.

```python
import asyncio
import csv
import random
from pydoll import Page

async def scrape_to_csv():
    async with Page() as page:
        await page.goto("[https://pavyzdys.lt/login](https://pavyzdys.lt/login)")
        
        # Žmogiškas duomenų įvedimas
        await page.type("#username", "vartotojas")
        await page.type("#password", "slaptazodis123")
        await page.click("#login-button")
        
        await page.wait_for_selector(".data-table")
        
        # Duomenų ištraukimas per JS vykdymą
        rows = await page.evaluate("""() => {
            const tableRows = document.querySelectorAll('.data-table tr');
            return Array.from(tableRows).map(row => {
                const cells = row.querySelectorAll('td');
                return Array.from(cells).map(cell => cell.innerText);
            });
        }""")
        
        # Įrašymas į failą
        with open('ataskaita.csv', 'w', newline='', encoding='utf-8') as f:
            writer = csv.writer(f)
            writer.writerow(["ID", "Pavadinimas", "Data", "Statusas"])
            writer.writerows(rows)

asyncio.run(scrape_to_csv())

---
## 3. Pažangūs nustatymai: Proksi ir User-Agent
Norint užtikrinti maksimalų anonimiškumą, rekomenduojama naudoti proksi serverius ir keisti naršyklės identifikatorių.

Proksi konfigūracija
Python

proxy_settings = {
    "server": "[http://123.456.78.9:8080](http://123.456.78.9:8080)",
    "username": "user",
    "password": "pass"
}
# Naudojama paleidžiant Browser(proxy=proxy_settings)
## Mobilių įrenginių emuliacija
Python

iphone_ua = "Mozilla/5.0 (iPhone; CPU iPhone OS 17_0 like Mac OS X) AppleWebKit/605.1.15..."
await page.set_user_agent(iphone_ua)
await page.set_viewport(width=390, height=844)

---
## 4. Diegimas serveryje naudojant Docker
Kad projektas veiktų 24/7, rekomenduojama naudoti konteinerizaciją.

Dockerfile pavyzdys:
Dockerfile
FROM python:3.11-slim

# Įdiegiame Chrome priklausomybes
RUN apt-get update && apt-get install -y \
    wget gnupg ca-certificates fonts-liberation \
    libnss3 libatk-bridge2.0-0 libgtk-3-0 --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .

CMD ["python", "skriptas.py"]
---
## 5. Išvados
„Pydoll“ yra galingas įrankis profesionalams, kuriems reikia apeiti sudėtingas apsaugos sistemas ir pasiekti aukštą duomenų rinkimo našumą. Dėl tiesioginio CDP protokolo naudojimo ši biblioteka užpildo spragą tarp paprasto testavimo ir sudėtingo „anti-detect“ automatizavimo.

Daugiau informacijos rasite oficialiame [Pydoll GitHub](https://github.com/autoscrape-labs/pydoll/) puslapyje.
