# Playspot AI hívóügynök (ElevenLabs) — v1 csomag

Cél: napi 10–12 hívás squash-, tenisz- és egyéb létesítményeknek. Siker = személyes időpont Ádámmal (vagy online bemutató, vagy döntéshozó neve + elérhetősége). Nem ad el.

## Fájlok
| Fájl | Mi ez | Hova kerül az ElevenLabsben |
|---|---|---|
| system_prompt.md | Ügynök viselkedése, cél, szabályok | Agent → System prompt |
| elso_uzenet.md | 3 nyitómondat A/B teszthez | Agent → First message (+ batch felülírás) |
| tudasbazis.md | Playspot tények + kifogáskezelés | Agent → Knowledge base (dokumentumként) |
| ertekeles.json | Sikerkritériumok + kinyerendő adatok | Agent → Analysis (success evaluation + data collection) |
| hivolista_sablon.csv | Batch hívás sablon, dinamikus változókkal | Batch calling feltöltés |
| tanulas/napi_tanulas_prompt.md | Esti tanulási kör | Ütemezett Claude-feladat |

## Felépítés
1. **Hívás:** ElevenLabs Agent + magyar telefonszám (Twilio vagy SIP). Batch calling a hívólistából CSV-vel, napi 10–12 sor.
2. **Hívás után:** az ElevenLabs post-call webhook elküldi az átiratot, a sikerértékelést és a kinyert mezőket → egy kis fogadó (Cloud Run, a h-t-email-projekt mintájára, aláírás-ellenőrzéssel) → „Playspot AI hívások” Google Sheet, „Hívások” lap. (Apps Script nem jó fogadónak: nem látja a fejlécet, így az aláírást sem tudja ellenőrizni.)
3. **Tanulás (este):** ütemezett Claude-feladat a tanulas/napi_tanulas_prompt.md alapján → „Javaslatok” lap.
4. **Jóváhagyás (Ádám, 5 perc):** a jó javaslatokat jóváhagyod → bekerülnek a tudasbazis.md-be → frissül a tudásbázis az ElevenLabsben. Így minden nap élesebb, de csak az kerül be, amit te jóváhagytál.

## Teendők sorrendben
- [ ] ElevenLabs fiók (Creator vagy afölötti csomag — a percdíjat és a kereteket a díjlapon ellenőrizni)
- [ ] Magyar telefonszám: Twilio (magyar számhoz cím- és cégigazolás kell, pár nap) vagy magyar SIP-szolgáltató
- [ ] Hang kiválasztása: 3–4 magyar hang próbája ugyanazzal a nyitómondattal, Dáviddal együtt döntsétek el
- [ ] Agent beállítása a fenti fájlokból; nyelv: magyar
- [ ] 10 próbahívás saját magadnak és ismerősöknek (kapuőr, elutasító, érdeklődő szerepben), felvétellel
- [ ] Webhook-fogadó + Sheet (cloud session-feladat)
- [ ] Esti tanulási kör ütemezése
- [ ] Pilot: 50 hívás, fele AI, fele te, ugyanabból a listából

## Mérés a pilotban
Felvett hívás · döntéshozó elérve · személyes időpont · online időpont · nem érdekelt · átlagos hívásidő · AI-ra rákérdeztek-e és mi lett utána.

## Szabály, ami nem lazítható
Az ügynök az első mondatban mondja meg, hogy AI-asszisztens. Az EU AI Act 50. cikke 2026. augusztus 2. óta ezt az első interakcióra írja elő.
