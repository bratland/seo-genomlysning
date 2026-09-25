# seo-genomlysning

En komplett SEO- och GEO-genomlysning av en webbplats, på svenska, levererad som ett dokument med en prioriterad åtgärdslista.

GEO-delen är den som skiljer: utöver vanlig teknisk SEO kontrollerar genomlysningen om sajten går att läsa och citera av språkmodeller. Konkurrensen där är tunnare än i Google, och en sajt som bygger sitt innehåll i JavaScript är osynlig för GPTBot, ClaudeBot och PerplexityBot, som kör noll JavaScript.

## Installation

```
/plugin marketplace add bratland/plugins
/plugin install seo-genomlysning@raion
```

## Användning

```
/seo-genomlysning dinsajt.se
```

Skillen ställer fyra frågor innan arbetet börjar — syfte, omfång, vilka datakällor som får läsas och vilket format leveransen ska ha — och kör sedan genomlysningen i sju steg.

## Vad den täcker

| Steg | Innehåll |
| --- | --- |
| Inventering | Vad som redan finns i verktyg, credentials och dokumentation |
| Teknisk crawl | Statuskod, TTFB, titlar, canonical, JSON-LD, rendering utan JavaScript |
| Mätdata | GA4 och Search Console, om åtkomst getts |
| Konkurrenter | Vilka URL-mönster som rankar på målgruppens huvudfras |
| GEO | `llms.txt`, citerbara fakta, omnämnanden i redaktionella översikter |
| Åtgärdslista | Sorterad efter effekt per nedlagd timme |
| Genomförande | Arbetet i sajtens repo, om åtgärderna ska byggas och inte bara beskrivas |

## Vad som behövs

Skillen kommer igång utan någon koppling. Search Console och GA4 gör
genomlysningen säkrare men är valfria — se [CONNECTORS.md](CONNECTORS.md).

## Licens

Fri att använda i eget arbete och i uppdrag åt kunder, och fri att ändra och
sprida vidare. Att sälja själva skillen kräver godkännande. Hela texten i
[LICENSE](LICENSE).

## Principer

- Mätt och uppskattat hålls isär, i varsin kolumn.
- Varje siffra kommer från en sida som faktiskt öppnats. Ett sökresultatsutdrag räknas inte som källa.
- Positioner utlovas aldrig. Mätpunkter att följa anges i stället.
- Organisationsnummer, adresser, betyg och kundnamn hittas aldrig på för att fylla ett schemafält. Saknas uppgiften frågar skillen efter den.
