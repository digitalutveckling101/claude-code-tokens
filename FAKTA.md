# Faktaunderlag: Få tokens att räcka i Claude Code

Kontrollerat mot Anthropics dokumentation 2026-11-14. Bara det som står här får användas som fakta i kursen. Markerat [osäker] ska kontrolleras igen eller formuleras försiktigt.

Källor:
- https://code.claude.com/docs/en/costs.md
- https://code.claude.com/docs/en/commands.md
- https://code.claude.com/docs/en/model-config.md
- https://code.claude.com/docs/en/context-window.md
- https://code.claude.com/docs/en/memory.md
- https://code.claude.com/docs/en/sub-agents.md
- https://code.claude.com/docs/en/permission-modes.md
- https://support.claude.com/en/articles/11145838-using-claude-code-with-your-pro-or-max-plan

## Grundmekanik
- Claude Code skickar med hela konversationen i varje anrop. En kort fråga i en session som varit öppen hela dagen drar användning för hela konversationen.
- Gammal kontext kostar tokens i varje nytt meddelande.

## Gränser (Pro och Max)
- Femtimmars sessionsgräns och veckogräns.
- Gränserna delas mellan Claude (chatten) och Claude Code.
- Max har dessutom en separat veckogräns för Fable.
- Att byta modell med /model återställer inte en nådd gräns.
- Vid gränsen: vänta på återställning, köp användningskrediter eller uppgradera. /rate-limit-options visar alternativen.
- Siffror (timmar, antal meddelanden): står inte i dokumentationen. Nämn inga.

## Kommandon
- /clear: ny konversation med tom kontext (alias /reset, /new). Kostar ingenting. Använd mellan orelaterade uppgifter. /rename före, /resume för att gå tillbaka.
- /compact [instruktioner]: sammanfattar konversationen och frigör kontext, kan ta fokusinstruktioner. Läser själv hela konversationen när den sammanfattar.
- /context: visar hur full kontexten är som ett färgat rutnät, med förslag.
- /usage (alias /cost): sessionens användning, plangränser och vad som drar mest (skills, subagenter, plugins, MCP-servrar), med rekommendationer.
- /model: byt modell, sparas som standard. Utan argument öppnas en väljare.
- /effort: low till xhigh, max eller auto.
- /plan eller Shift+Tab: plan-läge.
- /init: skapar en CLAUDE.md.
- /memory: redigera CLAUDE.md och automatiskt minne.
- /mcp: stäng av MCP-servrar som inte används.
- /rewind eller dubbeltryck på Esc: gå tillbaka till en tidigare punkt. Esc avbryter direkt.
- /btw: sidofråga som inte läggs till i konversationen.
- /autocompact: ställ in när komprimering sker automatiskt.
- /status: [osäker] använd /usage för gränser.

## Komprimering
- Sker automatiskt när kontexten närmar sig gränsen, så sessionen tar inte slut.
- Efter komprimering laddas CLAUDE.md om från disk, och upp till fem nyligen ändrade filer läses om.

## Modeller
- Alias: sonnet (Sonnet 5.5, vardagskodning), opus (Opus 5.5, komplext resonemang), haiku (Haiku 5.5, enkla uppgifter), fable (Fable 5.2, de svåraste och längsta uppgifterna), opusplan (Opus i plan-läget, sedan Sonnet för utförandet).
- Dokumentationen: Sonnet klarar de flesta kodjobb bra och kostar mindre än Opus. Spara Opus till komplexa arkitekturbeslut och resonemang i flera steg.
- Byter du modell följer subagenter som ärver modellen med.

## Tänkande och effort
- Utökat tänkande är på som standard. Tänkandetokens räknas som utdata och kan bli tiotusentals per anrop.
- Sänk effort med /effort för enklare uppgifter. Standard är medium. max kan ge avtagande nytta och överanalys.
- Tänkandet går inte att stänga av helt på 5.5-modellerna och Fable.

## Plan-läge
- Shift+Tab (växlar default, acceptEdits, plan) eller /plan före en prompt.
- Claude läser, utforskar och skriver en plan men ändrar inga filer förrän du godkänner.

## Subagenter
- Egen kontext. Bara en sammanfattning kommer tillbaka till huvudkonversationen.
- Drar ändå från samma användningsgränser. Varje subagent spenderar egna tokens.
- Enkla subagentuppgifter kan köras på haiku (model: haiku).
- Agentteam: cirka 7 gånger fler tokens när teammedlemmar kör i plan-läge.

## CLAUDE.md, skills och MCP
- CLAUDE.md laddas i varje session och kostar tokens även när den inte behövs.
- Håll varje CLAUDE.md under 200 rader. Längre filer tar mer kontext och följs sämre.
- Skills laddas bara när de används. Flytta specialinstruktioner dit.
- MCP-servrar kostar kontext. Stäng av de som inte används, föredra CLI-verktyg.

## Ultra (tillagt 2026-11-14)
- Verifierade Ultra-konton har ingen femtimmarsgräns, bara en veckogräns. (support.claude.com, artikel 11145838, och claude.com/pricing)
- Pro och Max har fortfarande både femtimmarsgräns och veckogräns.

## Rådgivaren /advisor (tillagt 2026-11-14)
Källa: https://code.claude.com/docs/en/advisor.md och commands.md
- `/advisor [model|off]` slår på en rådgivare, en andra och oftast starkare modell som Claude själv frågar vid viktiga lägen (före ett vägval, vid återkommande fel, innan något förklaras klart). Accepterar fable, opus, sonnet eller ett fullt modell-ID.
- Rådgivaren måste vara minst lika stark som huvudmodellen.
- Rådgivaren läser hela samtalet vid varje anrop. Förbrukningen räknas mot plangränserna. En Fable-rådgivare debiteras som användningskrediter på abonnemang där Fable gör det.
- Dokumentationen: en snabbare huvudmodell med en starkare rådgivare kostar oftast mindre än att köra den starkare modellen hela tiden.
- Experimentell. Kräver Anthropic API eller Claude Platform on AWS. Stäng av med `/advisor off`.
