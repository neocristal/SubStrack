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
