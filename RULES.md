# Eksperimento taisyklės

## Pagrindinės taisyklės

### 1. Sprendimų autonomija
- Visi pozicijų sprendimai daromi AI (Claude) - autonomiškai
- Radoslav GALI klausti, ginčytis, prašyti pateisinimo, BET negali atlikti veiksmų portfeliuose
- Jei Radoslav prašo "pirk X" - AI vertina ar tai atitinka portfelio strategiją ir gali atmesti
- Eksperimento sąžiningumui - AI turi būti FREE iš human interventionų

### 2. Trade vykdymas
- Sprendimai dokumentuojami `trades.json` su laiku (UTC)
- Hipotetiška entry kaina = next US market open (po sprendimo)
- Stock'ams: midpoint of bid/ask @ open, jei nežinoma - daily open
- Opcionams: midpoint of bid/ask + 5% premium (real-world slippage approximation)
- Slippage: 0.1% kiekvienam stock orderiui

### 3. Komisiniai
- Stocks/ETFs: $0
- Options: $0.65 per kontraktą (IBKR pricing)
- Margin interest: 6.5% APR jei naudojama (assume daily compound)

### 4. NAV apskaita
- Atnaujinama kasdien po US market close (16:00 ET ~ 23:00 Vilnius vasarą)
- NAV = stock pozicijų vertė + options market value + cash + collateral
- T-bills accruja 4.2% APR linijiškai
- STRC dividend 11.5%/12 = ~0.96% mėnesį akumuluojama
- Options market value = midpoint bid/ask (jei nežinoma, naudojama Black-Scholes su naujausiu IV)

### 5. Rebalansavimas
- AI gali rebalance bet kada
- Jokio "draugavimo" su pozicijomis - jei tezė sulaužyta, exitas be sentimento
- Kiekvienas rebalance dokumentuojamas su rationale

### 6. Triggers
Pre-defined deployment trigeriai dry powder kapitalui:

| Trigger | Action | Capital deploy |
|---|---|---|
| VIX 20-25 | Sell more aggressive CSPs, add iron condors | 20% of dry powder |
| VIX 25-35 + SPX -8% | Convert yield → LEAPS on quality | 40% of dry powder |
| VIX 35+ + SPX -15% | Full deploy + box spread margin if needed | 100% + margin |

### 7. Maximum risk caps
- Single position: max 15% kapitalo
- Single sector exposure: max 30% kapitalo (excluding cash)
- Options total premium at risk: max 20% kapitalo
- Margin used: max 30% kapitalo

### 8. Reporting cadence
- **Daily:** NAV snapshot, position values updated
- **Weekly:** Performance vs benchmarks, drawdown stats
- **Monthly:** Full review, thesis updates, lessons learned
- **Quarterly:** Major rebalance review, attribution analysis

## Eksperimento sąžiningumo apsauga

### Ką AI MOKA daryti:
- Naudoti realių rinkų duomenis (finviz, X.com, news) per WebFetch/WebSearch
- Skaičiuoti, modeliuoti, simulate'inti
- Skaityti earnings reports, 10-K, 10-Q
- Atlikti technical, fundamental, sentiment analizes

### Ko AI NEDARO:
- Look-ahead bias: nepasirenka pozicijų pagal "po fact" informaciją
- Cherry-pick'a entry kainas - vykdymas @ market open
- Survivorship bias: jei pozicija blogai veikia - lieka logge, ne ištrinama
- Cheat'a su laiku: jokių "ai pakeičiau prieš tris dienas" retroaktyvių pakeitimų

## Galimi nesėkmės signalai (kada eksperimentas baigtas)

- AI portfelis -25% nuo pradinės sumos ($75K) → AI failed
- AI portfelis underperformuoja SPY by >15% per metus → AI failed
- Margin call → AI failed
- Radoslav portfelis +50% AI portfelio per metus → human winner clear

## Galimi sėkmės signalai

- AI portfelis +25%+ per metus → tikslas pasiektas
- AI portfelis beats SPY by >5% per metus → alpha generation
- AI portfelis beats Radoslav portfelį → AI competitive

## Žurnalo įrašai - kada updaitinami

| Įvykis | Veiksmas |
|---|---|
| Naujas trade | Pridėti į `trades.json` su timestamp |
| Pozicijos exit | Atnaujinti `positions.json` (remove) + `trades.json` (close) |
| Daily close | Atnaujinti `daily-values.json` su NAV |
| Major tezės pokytis | Atnaujinti `THESIS.md` su data ir reason |
| Rebalance | Į `trades.json` su "REBALANCE" tag |

## Eksperimento pabaigos kriterijai

Eksperimentas oficialiai baigiamas vienu iš sąlygų:

1. **Time-based:** 12 mėnesių po start (2027-06-03) - pirmasis pilnas annual review
2. **Performance-based:** Vienas portfelis +100% per kitą (rare)
3. **Capital-based:** AI portfelis < $50K (50% drawdown - akivaizdus failure)
4. **Decision-based:** Radoslav nusprendžia užbaigti dėl turinio cikllo

Po pabaigos - publikuojamas pilnas attribution analysis Discord/Instagram/Facebook.
