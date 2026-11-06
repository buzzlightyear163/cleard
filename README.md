# CLEARED · Only buys launches that clear.

Landningssida och fungerande verktyg för $CLEARED. En enda fil (`index.html`) med egen WebGL-motor och inga bibliotek, plus `og.jpg` som delningsbild.

## Vad som fungerar på riktigt
- **Skannern** (sektion 03): klistra in en pump.fun-CA. Webbläsaren läser coinens första trades från publik Solana RPC och räknar ut de fyra gates:
  - **G1 Bundle**: andel av supply som andra wallets köpte i samma block som coinen skapades (gräns 3 %)
  - **G2 Snipers**: köp i de nästa 8 slotsen, cirka 3 s (gräns 12 %)
  - **G3 Dev**: vad deployer-walleten håller nu (gräns 5 %)
  - **G4 Fresh**: köp av tidiga wallets med färre än 10 transaktioner totalt (gräns 10 %)
- **Clearance board** (sektion 04): live-ström av nya pump.fun-launches via PumpPortal (gratis, ingen nyckel). G3 läses direkt från create-eventet. Var 20:e sekund får en launch full skanning i besökarens webbläsare.
- **Clearance card**: delningsbild (1200×675) med klustret och gate-resultaten, plus "Copy text".
- Om live-strömmen inte går att nå visar tavlan en tydligt märkt simulering (SIM / Preview). Simulerad data visas aldrig som riktig.
- Motorns köp är **paper mode** tills motorns wallet går live (ligger under "Next" på roadmapen), och det står på sidan.

## Fyll i vid launch (överst i scriptet, `CONFIG`)
| Fält | Vad |
|---|---|
| `ca` | $CLEARED mint-adressen. Tom = "Posted here and on X at launch". Köpknapparna går då till `pump.fun/coin/<ca>`. |
| `x` | Länk till X-kontot |
| `dexscreener` | Valfritt |
| `site` | Domänen, skrivs på clearance-korten |
| `engineWallet` | Motorns publika wallet när den är live |
| `rpc` | Lista med RPC-noder. Lägg gärna en gratis Helius/QuickNode-URL först, så slipper skanningarna rate limits. |
| `gates` | Gränserna för de fyra gates. Texterna på sidan uppdateras automatiskt. |
| `fees` | Fördelningen av creator fees |

## Deploy
1. Lägg `index.html`, `og.jpg` och `README.md` i ett nytt GitHub-repo.
2. Vercel → New Project → importera repot → Framework: **Other**, ingen build-kommando, output = roten.
3. Kolla efter deploy:
   - Intron och 3D-klustret i heron (det ska rulla mellan exempel-launches och sveppa med ringen)
   - Att typsnitten laddar: Schibsted Grotesk, Cormorant Garamond (kursiva ord) och Chivo Mono
   - Att lampan uppe till höger går till **Live** och tavlan fylls med riktiga launches
   - Skanna en färsk launch från tavlan (klicka en rad)
   - X-förhandsvisningen med og.jpg (klistra in länken i en tweet-draft)

## Ärliga begränsningar
- En launch måste ha minst 3 köpare innan den bedöms (`CONFIG.scan.minBuyers`). Annars visas "Too early" i skannern och "Quiet" på tavlan, så att döda launches aldrig får CLEARED.
- Coins med fler än cirka 4 000 trades visas som "Out of window": skannern kan inte bläddra tillbaka till block 0 från en webbläsare.
- Publik RPC kan rate-limita vid hög trafik. Lös det med en egen RPC-URL i `CONFIG.rpc`.
- G4 fångar inte en dev som köper med gamla wallets. Det står i FAQ:n, och "funding source" som femte gate finns på roadmappen.
