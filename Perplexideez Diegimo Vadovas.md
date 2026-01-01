# Perplexideez Diegimo Vadovas

## 🚀 Greitas paleidimas
1. Klonuokite saugyklą:
   `git clone https://github.com/brunostjohn/perplexideez.git`
2. Nukopijuokite konfigūraciją:
   `cp .env.example .env`
3. Paleiskite paslaugas:
   `docker-compose up -d`

## 🛠 Konfigūracijos parametrai
| Kintamasis | Aprašymas |
| :--- | :--- |
| `DATABASE_URL` | PostgreSQL prisijungimo nuoroda |
| `NEXTAUTH_SECRET` | Saugumo raktas sesijoms |
| `OLLAMA_BASE_URL` | Vietinio AI modelio adresas |
| `SEARXNG_URL` | Paieškos variklio API adresas |

## 💡 Rekomendacija
Geriausia Perplexideez naudoti kartu su **Authentik** arba kitu OIDC teikėju, jei planuojate suteikti prieigą keliems vartotojams. Vietiniam naudojimui rekomenduojama rinktis **Llama 3** arba **Mistral** modelius per Ollama.

# 🔍 Perplexideez Sistemos Statusas

## 📋 Reikalavimai prieš paleidimą
- [ ] SearXNG: Įjungtas `formats: [ json ]` konfigūracijoje.
- [ ] PostgreSQL: Paruošta tuščia DB.
- [ ] Ollama: Atsisiųsti modeliai (Llama3.1, Gemma2, Nomic-Embed).

## 🚀 Diegimo komandos
1. **DB Paruošimas:**
   ```bash
   docker run --env DATABASE_URL=... ghcr.io/brunostjohn/perplexideez/migrate
```
   Paleidimas:

```Bash
docker-compose up -d
```
🔐 SSO / Authentik Integracija
Jei naudojate Authentik, nukreipimo URL (Redirect URL): https://[JŪSŲ_DOMENAS]/auth/callback/generic-oauth

---

### 💡 Svarbios pastabos:
* **Bare Metal:** Autorius griežtai nepalaiko diegimo tiesiai į operacinę sistemą (be Docker) – tokios problemos nebus sprendžiamos.
* **Kubernetes:** Helm lentelės dar ruošiamos, todėl rekomenduojama remtis autoriaus „homelab“ pavyzdžiais, jei diegiate į K8s klasterį.
* **Atmintis:** Jei naudojate `qwen2.5:32b`, įsitikinkite, kad jūsų serveris turi bent 24-32GB operatyviosios atminties (RAM/VRAM).

**Ar norėtumėte, kad padėčiau sugeneruoti konkretų `docker-compose.yml` failą su visais jūsų nurodyt
