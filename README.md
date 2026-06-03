# AI portfelio eksperimentas

## Apie eksperimentą

Šis projektas yra kontroliuojamas eksperimentas, lyginantis dirbtinio intelekto sugeneruotą kapitalo alokacijos strategiją su Radoslav investuotojo portfoliu realiu laiku.

**Tikslas:** atsakyti į klausimą - ar AI tikrai gali konkuruoti su patyrusiu investuotoju finansų rinkose, ar tai dar tik mokslo fantastika.

**Naudojama mokomajam turiniui** (Discord, Instagram, Facebook) - NE realūs pinigai.

## Pradinės sąlygos

- **Pradinis kapitalas:** $100,000 USD (hipotetinis)
- **Pradžios data:** 2026-06-03 (close)
- **Tikslinė metinė grąža:** 25%
- **Trukmė:** atviras (perskaičiuojama metiniam atskaitymui)
- **Tax assumption:** Section 1256 60/40 SPX opcionams, kitur ignoruojama

## Benchmark'ai

| Benchmark | Pradinė reikšmė (2026-06-03) | Tikslas |
|---|---|---|
| SPY (S&P 500 ETF) | TBD @ open | Beat |
| QQQ (Nasdaq-100) | TBD @ open | Beat |
| BTC (Bitcoin) | TBD @ open | Compare (jei AI portfelis bullish crypto) |
| **Radoslav portfelis** | $100,000 hipotetinis | **Pagrindinis lyginimo objektas** |

## Taisyklės

Žiūrėk [RULES.md](RULES.md) detaliai. Pagrindinės:

1. **Decisions = AI tik.** Joks žmogiškas interventionas pozicijų sprendimuose.
2. **Visi pakeitimai loguojami** su laiku, kaina, priežastimi į [trades.json](trades.json).
3. **Daily NAV** atnaujinama po US market close.
4. **Pozicijos vykdomos** hipotetiškai @ next-day-open kainos po sprendimo.
5. **Slippage prielaida:** 0.1% kiekvienam vykdomam orderiui.
6. **Komisiniai:** $0 stock'ams, $0.65 per opcionų kontraktą.
7. **Margin leverage:** maksimaliai 1.3x (broker maintenance margin model).

## Failai

| Failas | Paskirtis |
|---|---|
| [README.md](README.md) | Šis failas |
| [RULES.md](RULES.md) | Detalios eksperimento taisyklės |
| [THESIS.md](THESIS.md) | Kiekvienos pozicijos investavimo tezė |
| [positions.json](positions.json) | Dabartinės atviros pozicijos (machine-readable) |
| [trades.json](trades.json) | Trade log su laiku ir kainomis |
| [daily-values.json](daily-values.json) | NAV istorija kasdien |
| [portfolio.html](portfolio.html) | Interaktyvus dashboard |

## Kaip naudoti dashboard'ą

1. Atidaryk [portfolio.html](portfolio.html) naršyklėje (`open portfolio.html` arba dukart spustelti)
2. Dashboard krauna duomenis iš JSON failų automatiškai
3. Atnaujinta po kiekvieno trade arba kasdien po close

## Pradinis snapshot - 2026-06-03

**Layer 1: Yield engine (40.2%):**
- STRC (Strategy 11.5% preferred): $8,000
- PDI (PIMCO Dynamic Income CEF): $5,000
- GME July'26 $19 short puts (cash-secured): $5,700 collateral
- T-bills 3M ladder: $20,000
- Cash buffer: $1,500

**Layer 2: Long equity (34%):**
- BMNR (Bitmine ETH treasury, 12% discount to NAV): $7,000
- SOFI (PEG 0.58, post-correction growth): $8,000
- GME (SOTP $35 vs $20.92 stock): $5,000
- LMT (defense contrarian, P/E 16): $5,130
- XOM (energy defensive, Iran hedge): $4,917
- URA (uranium structural ETF): $3,975

**Layer 3: Asymmetric / Options (13.7%):**
- LMT LEAPS Jan'27 $500C ×1: $5,500
- GEV Sep'26 $1000/$1100 bull call spread ×2: $5,000
- SOFI Jan'27 $20C ×4: $2,000
- BMNR Sep'26 $20C ×2: $1,200

**Layer 4: Hedge (0.5%):**
- SPX Sep'26 $7000P ×1: $500 (tail hedge)

**Layer 5: Dry powder (11.6%):**
- T-bills extended ladder: $11,600 (deploy on VIX > 25 trigger)

## Pradinė tezė vienam sakiniui

Rinka @ ATH ir VIX 16, todėl baseline yra premium-harvest mode (40% yield engine), tačiau yra trys specifinės asimetriškos galimybės (BMNR 12% discount ETH NAV, SOFI PEG 0.58, GME SOTP 41% discount), kurios verta agresyvesnio kapitalo įsipareigojimo nei tipinis institucinis defensyvinis allocator pasirinktų.
