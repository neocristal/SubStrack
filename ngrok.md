# Kas yra ngrok ir kodėl jis tapo standartu development pasaulyje

## Įvadas
Šiuolaikinė programinės įrangos kūrimo realybė reikalauja greito grįžtamojo ryšio, integracijų su trečiųjų šalių sistemomis ir galimybės parodyti veikiančius sprendimus dar prieš juos išleidžiant į produkciją. Čia atsiranda **ngrok** ir jo alternatyvos – įrankiai, leidžiantys **lokalią aplikaciją saugiai padaryti pasiekiamą iš interneto**.

Šiame straipsnyje paaiškinsime:
- kas yra ngrok;
- kodėl profesionalai jį naudoja;
- kokios yra alternatyvos (ypač **Cloudflare Tunnel**);
- kada ir kodėl tai yra **saugiau** nei tiesiog „atidaryti portą“;
- praktinius pavyzdžius ir patarimus.

---

## Kas yra ngrok?

**ngrok** – tai „secure tunneling“ įrankis, kuris sukuria **užšifruotą HTTPS tunelį** tarp viešo interneto ir tavo lokalaus kompiuterio ar serverio.

Paprastai tariant:

```
Internet (HTTPS)
   ↓
ngrok serveriai
   ↓ (encrypted tunnel)
localhost:8501 (Streamlit / API)
```

Be jokių firewall, router ar SSL konfigūracijų tu gali:
- pasiekti lokalią aplikaciją iš bet kur;
- turėti galiojantį HTTPS sertifikatą;
- priimti webhook’us iš Jira, GitHub, Stripe ir pan.

### Kodėl tai veikia taip gerai?
- Tunelis inicijuojamas **iš vidaus į išorę** (outbound connection).
- Nereikia atidaryti inbound portų.
- SSL valdomas ngrok infrastruktūros.

---

## Kodėl profesionalai naudoja ngrok?

### 1. Greitis
Per 30 sekundžių gali parodyti veikiančią sistemą klientui ar komandai:
```bash
streamlit run app.py
ngrok http 8501
```

### 2. Integracijos
Daugelis SaaS sistemų (Jira, Azure DevOps, Stripe, Microsoft Graph) reikalauja **viešo HTTPS endpointo**. ngrok leidžia tai turėti development aplinkoje.

### 3. Saugumas
- Jokio atviro porto (80/443) tavo kompiuteryje.
- Tunelis yra laikinas.
- Gali būti papildomai apsaugotas API keys, OAuth, IP allowlist.

### 4. Realistiška testavimo aplinka
Testuoji beveik identiškai kaip produkcijoje, bet be rizikos.

---

## Saugumo aspektas: kodėl tai geriau nei „atidaryti portą“?

**Bloga praktika**:
- Atidaryti 8501 ar 8000 portą routeryje
- Be HTTPS
- Be WAF
- Be audit log

**Tunneling įrankiai (ngrok / Cloudflare Tunnel)**:
- Naudoja outbound TLS jungtį
- Nėra tiesioginio inbound srauto
- Sertifikatai valdomi profesionaliai
- Galima greitai išjungti

Tai ypač svarbu, kai dirbi su:
- API raktai
- PII duomenys
- Enterprise klientais

---

## Ngrok minusai (kodėl ieškoma alternatyvų)

- Nemokama versija turi **laikiną URL**
- URL keičiasi po kiekvieno restarto
- Nemokama versija riboja srautą
- Ne visada tinkama ilgalaikiam demo

Dėl to profesionalai dažnai pereina prie alternatyvų.

---

## Cloudflare Tunnel (cloudflared) – rekomenduojama alternatyva

**Cloudflare Tunnel** yra modernus ngrok pakaitalas, kuris naudoja tą pačią tunelio idėją, bet su **enterprise lygio infrastruktūra**.

### Kodėl Cloudflare Tunnel yra stiprus pasirinkimas?
- Nemokamas
- Stabilus domenas (tavo domenas)
- Automatinis HTTPS
- Integruotas WAF
- Galimybė naudoti Cloudflare Access (SSO, MFA)

### Architektūra
```
Internet (HTTPS)
   ↓
Cloudflare Edge
   ↓ (secure tunnel)
cloudflared agent
   ↓
localhost:8501
```

### Kada rinktis Cloudflare Tunnel?
- Ilgalaikis demo
- Pilotinė aplinka
- Vidiniai enterprise įrankiai
- OAuth integracijos (nereikia keisti redirect kas kartą)

---

## Kitos alternatyvos ir kada jas naudoti

### LocalTunnel
- Greita
- Be registracijos
- Tinka trumpiems testams
- Mažiau patikima

### Expose (BeyondCode)
- Patogus UI
- HTTPS
- Ribota nemokama versija

### Serveo (SSH tunnel)
- Minimalus setup
- Nestabilus
- Labiau „quick hack“

### FRP (Fast Reverse Proxy)
- Reikalauja savo VPS
- Maksimali kontrolė
- Didesnė administravimo kaina

### Tailscale Funnel
- Puiku komandoms
- Zero-trust tinklas
- Reikalauja Tailscale

---

## Kada ką naudoti? (praktiniai patarimai)

- **Greitas demo / webhook testas** → ngrok
- **Ilgalaikis demo su OAuth** → Cloudflare Tunnel
- **Vidinė komanda** → Tailscale Funnel
- **Savas serveris / maksimalus valdymas** → FRP

Svarbiausia: **nėra vieno teisingo sprendimo visiems atvejams**.

---

## Išvada

ngrok ir jo alternatyvos išsprendžia realią, kasdienę problemą – kaip saugiai parodyti ir testuoti sistemą, kuri dar nėra produkcijoje.

Profesionalai juos naudoja ne todėl, kad „taip patogiau“, o todėl, kad:
- tai saugiau;
- tai greičiau;
- tai artimiau realiai produkcijai;
- tai leidžia fokusuotis į produktą, o ne infrastruktūrą.

Jei reikėtų vieno patarimo:
> **Development ir demo aplinkose niekada neatidarinėk portų – naudok tunelius.**

---

---

# Techninis priedas (Markdown)

## ngrok
```bash
ngrok http 8501
```

## Cloudflare Tunnel
```bash
cloudflared tunnel login
cloudflared tunnel create my-app
cloudflared tunnel route dns my-app app.example.com
cloudflared tunnel run my-app
```

## LocalTunnel
```bash
npm install -g localtunnel
lt --port 8501
```

## Expose
```bash
expose share 8501
```

## Serveo
```bash
ssh -R 80:localhost:8501 serveo.net
```

## FRP (santrauka)
- frps – VPS
- frpc – lokalus agentas

## Tailscale Funnel
```bash
tailscale funnel 8501
```

