# Filmaften — opsætning

En web-app til jeres watchlist: film & serier fra jeres streamingtjenester. Tilføj via IMDB-id, søg på titel + skuespiller, filtrér på film/serie og "Til Mette", og kast terningen om aftenen.

Du skal udfylde **tre ting** øverst i `index.html` (blokken markeret `KONFIGURATION`). Så er den klar.

---

## 1. OMDb-nøgle (påkrævet — henter IMDB-rating, plakat, skuespillere)

1. Gå til **omdbapi.com/apikey.aspx**
2. Vælg **FREE (1.000 pr. dag)**, indtast din mail, og bekræft via linket i mailen du får.
3. Kopiér nøglen ind i `index.html`:
   ```js
   const OMDB_API_KEY = "din_nøgle_her";
   ```

## 2. Firebase / Firestore (databasen — samme værktøj som sidst)

1. **console.firebase.google.com** → opret projekt (eller genbrug et).
2. **Build → Firestore Database → Create database** → start i **production mode** (region: eur3 el.lign.).
3. **Project settings (tandhjul) → General → Your apps → Web (`</>`)** → registrér app → kopiér `firebaseConfig`-objektet ind i `index.html`, så det erstatter placeholder-værdierne.
4. **Firestore → Rules** — indsæt reglerne fra `firestore.rules` (se den fil) og tryk **Publish**.

## 3. TMDB-nøgle (valgfri — giver top 7 skuespillere i stedet for ~4)

Vil du have flere skuespillere (bedre skuespiller-søgning):

1. **themoviedb.org** → opret konto → **Settings → API** → bed om en gratis nøgle (v3).
2. Sæt den ind:
   ```js
   const TMDB_API_KEY = "din_tmdb_nøgle";
   ```
Lader du den stå tom, bruger appen bare OMDb's skuespillere. Alt andet virker uændret.

---

## Streamingtjenester

Ret listen så den matcher jeres abonnementer:
```js
const STREAMING_SERVICES = ["Netflix","Max","Disney+","Viaplay","TV 2 Play","SkyShowtime","Prime Video","Apple TV+","DRTV"];
```

## Læg den på GitHub Pages

1. Nyt repo → læg `index.html` ind (README og firestore.rules er kun til dig — de behøver ikke ligge på siden).
2. **Settings → Pages → Source: Deploy from a branch → main / root → Save.**
3. Efter et øjeblik ligger den på `https://ditbrugernavn.github.io/repo-navn/`.

Tilføj den URL under **Firebase → Authentication → Settings → Authorized domains** hvis du senere slår login til.

---

## Sådan bruges den

- **Tilføj:** indsæt IMDB-id (`tt0082971`) eller hele linket, vælg tjeneste, sæt evt. flueben ved **Til Mette**, tryk **Tilføj**. Titel, år, film/serie, rating, plakat og skuespillere hentes selv. Tilføjelsesdatoen gemmes.
- **Søg:** skriv en titel *eller* et skuespillernavn.
- **Filtrér:** Alle / Film / Serier, "Kun til Mette", og pr. tjeneste.
- **Terning:** 🎲 vælger en tilfældig titel blandt dem, filteret viser lige nu — så du kan fx sætte "Kun til Mette" + "Netflix" og lade terningen bestemme.

## Et ord om sikkerhed

Firestore-reglerne i `firestore.rules` gør basen **åben for læsning og skrivning** for enhver, der kender jeres projekt — fint til en privat, ikke-følsom hygge-liste. Vil du lukke den, kan du slå **Firebase Authentication** til og ændre reglerne til kun at tillade jeres egne konti. Sig til, så laver jeg den version.

## Hvis noget driller

- **Rødt banner om config:** du mangler at udfylde `firebaseConfig` eller `OMDB_API_KEY`.
- **"Kunne ikke læse databasen":** Firestore-reglerne er ikke publiceret — se trin 2.4.
- **Plakat mangler:** OMDb har ikke altid en plakat; appen viser så titlens initialer. Det er normalt.
