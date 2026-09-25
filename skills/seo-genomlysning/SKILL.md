---
name: seo-genomlysning
description: "Kör en komplett SEO- och GEO-genomlysning av en domän på svenska — teknisk crawl, on-page, AI-synlighet i språkmodeller, sökord, lokal SEO och mätning — och levererar den som ett dokument med prioriterad åtgärdslista. Används både för genomlysningen och för att bygga åtgärderna i sajtens repo."
version: "1.2.0"
author: Anders Bratland
user-invocable: true
argument-hint: "<domän>"
---

# SEO-genomlysning

Används när någon vill ha en SEO-rapport, SEO-analys, sökgenomlysning eller AI-synlighetsanalys för en webbplats — egen eller en kunds — och när åtgärderna från en sådan rapport ska genomföras i sajtens repo.

Språk: svenska, om inget annat sagts. Ton: rak, konkret, inga floskler. Varje påstående ska gå att belastas med en siffra eller en källa.

## Steg 0 — Fråga först

Använd AskUserQuestion innan något arbete börjar. Flagga aldrig en lucka i en leveransnot när den kan frågas bort i förväg. Fyra frågor:

1. **Syfte** — eget beslutsunderlag, återanvändbar mall, eller säljunderlag mot kund?
2. **Omfång** (flerval) — teknisk SEO + on-page, GEO/AI-synlighet, sökord + konkurrenter, lokal SEO.
3. **Datakällor** — får GA4/Search Console läsas, eller bara publik data?
4. **Format** — Claude Doc, HTML-artefakt, eller markdown i repot.

## Steg 1 — Inventera vad som redan finns

**Börja med att avgöra vad du kan nå.** Har du ett skal och en ansluten mapp
gäller listan nedan. Har du varken eller, vilket är normalfallet i
skrivbordsappen utan ansluten mapp, hoppa rakt till steg 3: genomlysningen körs
på publik data och blir fullgod ändå. Säg i så fall en rad om vad som utelämnas,
och fortsätt. Stanna aldrig upp och be om ett skal.

Med tillgång: innan nya verktyg föreslås, ta reda på vad som redan är på plats.
Fråga efter det du inte kan se, och leta efter resten:

- Befintliga skills: `ls ~/.claude/skills`
- Analysverktyg på maskinen: `which gcloud lighthouse`, och sparade OAuth-credentials under `~/.config/`
- Sajtens repo, om det finns lokalt: läs `README*.md`, `ARCHITECTURE.md` och `docs/` — mätningen är ofta redan dokumenterad någonstans, och stämmer den inte med koden är det i sig ett fynd
- Crawlverktyg: Screaming Frog, lighthouse

SearchSkills och SearchMcpRegistry för SEO-konnektorer. Semrush, Ahrefs och OpenRush finns i registret men kräver licens.

Rapportera vad som fanns och vad som saknades. En befintlig GA4-åtkomst är värd mer än en ny konnektor.

## Steg 2 — Skapa dokumentskelettet

Välj format efter vad som finns: ett dokument om verktyget för det är
tillgängligt, annars en artefakt eller en markdownfil. Frågan ställdes i steg 0,
men svaret kan peka på något som saknas: välj då närmaste tillgängliga och säg
vilket det blev.

Om leveransen är ett Claude Doc: skapa skelettet som turens första verktygsanrop, öppna det, och fyll sedan en sektion per anrop. Sektioner:

Sammanfattning · Metod och datakällor · Teknisk SEO · On-page och innehåll · GEO och AI-synlighet · Sökord och konkurrenter · Lokal SEO · Mätning · Prioriterad åtgärdslista · Källor

## Steg 3 — Teknisk crawl

Hämta `robots.txt` och `sitemap.xml`, crawla sedan varje URL i sitemap. Per sida: statuskod, TTFB, titel, meta description, canonical, antal H1/H2, JSON-LD, `lang`, ordantal, unika interna länkar, externa länkar, bilder.

En one-liner per sida via `curl` räcker och är snabbare än att installera en crawler.

Röda flaggor att alltid kolla: noll JSON-LD, saknad canonical, fler än en H1, sidor under 300 ord, färre än 8 unika interna länkar (= bara navigationen länkar), saknad Search Console-verifiering.

**Rendering.** Hämta sidan som en crawler ser den, inte som webbläsaren visar den. En SPA som skickar 800 byte och bygger resten i JavaScript är osynlig för GPTBot, ClaudeBot och PerplexityBot, som kör noll JavaScript. Är `curl`-svaret under ~2 kB brödtext är rendering rapportens dyraste fynd, oavsett hur bra sidan ser ut i en webbläsare.

## Steg 4 — Mätdata

Om GA4-åtkomst godkänts: kör `runReport` mot Data API för kanaler, landningssidor, händelser, månadsserie, källa/medium och land. Ange alltid hur lång historiken faktiskt är — en property som är två veckor gammal ger en nollpunkt, inte en baslinje.

Kontrollera om Search Console finns. Saknas den är det nästan alltid rapportens dyraste lucka: utan GSC går det inte att svara på om sajten är indexerad.

## Steg 5 — Konkurrenter och sökord

Sök på målgruppens huvudfras ("<tjänst> <ort>") och notera vilka URL:er som rankar. Mönstret i URL:erna är själva insikten — om alla i toppen har en dedikerad landningssida och kunden inte har det, är det den viktigaste åtgärden.

Bygg en tabell: konkurrent, URL-mönster, vad de gör rätt. Lägg kunden sist i samma tabell.

Sökordstabell med kolumnerna: sökord, köpläge, konkurrens, målsida. Märk tydligt att volym och svårighetsgrad är bedömda när ingen Semrush/Ahrefs-licens finns.

## Steg 6 — GEO och AI-synlighet

Den här delen är ofta där största hävstången finns, eftersom konkurrensen är tunnare.

- Finns `llms.txt`? Bedöm om den namnger ort, tjänster, priser, kontaktväg och citerbara fakta.
- Finns citerbart material på sajten — siffror, årtal, namngivna metoder, konkreta resultat? Språkmodeller citerar påståenden de kan attribuera.
- Hämta de redaktionella översikterna för branschen och orten och kontrollera om kunden nämns. Listicles är det språkmodeller läser.
- Sök på varumärket ensamt. Krockar namnet med något annat är `Organization`-schema med `sameAs` den billigaste motmedicinen.

## Steg 7 — Åtgärdslista

Tabell sorterad efter effekt per nedlagd timme: nummer, åtgärd, insats i timmar, effekt, när. Allt som låser upp mätbarhet (Search Console, key events) ligger i vecka 1 oavsett storlek.

Avsluta med rimlig förväntan i veckor eller månader, och vilka mätpunkter som ska följas. Lova aldrig positioner.

## Genomförande i repot

Gäller när sajtens kodbas är åtkomlig och åtgärderna ska byggas, inte bara
beskrivas. Saknas den är åtgärdslistan i steg 7 hela leveransen, och den är
skriven för att kunna lämnas vidare till den som förvaltar sajten. Avsnittet
nedan hoppas då över utan kommentar.

**Redigera generatorn, aldrig resultatet.** Har sajten ett byggskript skrivs varje `.html` över vid nästa körning. Hitta generatorn först (`scripts/build*.mjs`, `package.json`, en SSG-konfig) och gör ändringen där. En rättning i en genererad fil är borta innan någon hinner deploya den.

**En ny sida ska äga sin fras ensam.** Skapas en ortsida eller landningssida för en fras måste den fras samtidigt bort ur titeln på varje annan sida som bär den — oftast startsidan. Annars konkurrerar sajten mot sig själv och den nya sidan får noll visningar trots att den är bättre. Bestäm vilken sida som äger frasen, skriv om de andras titlar, och säg vilket bytet var.

**Spåra sidan hela vägen till produktion.** En sida kan genereras, committas, deployas grönt och ändå svara 404. Följ kedjan innan arbetet rapporteras klart:

- `Dockerfile` eller motsvarande: kopieras filen till imagen? En uppräknad `COPY a.html b.html`-lista är en fälla. Byt den mot ett mönster och låt `.dockerignore` filtrera.
- Webbserverns routing: fångar en catch-all upp `robots.txt`, `sitemap.xml` och `llms.txt` och svarar med app-skalet i stället för filen?
- Analysskriptet: många mätuppsättningar har en sökvägs-allowlist. En sida som saknas där mäts aldrig, och luckan syns först om en månad.
- Sitemap och `llms.txt`: genereras de ur samma lista som sidorna, eller underhålls de för hand?

Avsluta med `curl` mot den publicerade URL:en och läs statuskod, `<title>` och JSON-LD ur svaret. Ett grönt bygge är inte ett kvitto.

**Bygg schemat som en `@graph`.** En `Organization`-nod med `@id`, som varje annan nod refererar, i stället för en kopia per sida. Utelämna `aggregateRating` på `Organization`: egenpublicerade betyg ignoreras av Google och kan dra en manuell åtgärd. Låt betyget stå synligt på sidan i stället.

**FAQ-text och FAQ-schema ur samma källa.** Lägg frågorna i en array som renderas både till HTML och till `FAQPage`. Två kopior glider isär, och då beskriver strukturerad data något som inte står på sidan.

**Uppdatera dokumentationen i samma commit.** Stämmer inte README med koden är det ett fynd att rapportera, inte något att gå förbi.

## Regler

- Skilj alltid mätt från uppskattat, i en egen kolumn eller rad i metodavsnittet.
- Öppna sidorna du tar siffror ifrån. Ett sökresultatsutdrag är inte en källa.
- Länka varje namngiven konkurrent till den sida du faktiskt läste.
- Sätt as-of-datum. SERP-positioner åldras på dagar.
- Hittar du något som motbevisar befintlig dokumentation i repot — säg det, och föreslå att doken uppdateras.
- Hitta aldrig på organisationsnummer, adresser, betyg eller kundnamn för att fylla ett schemafält. Saknas uppgiften: fråga, eller utelämna fältet.
