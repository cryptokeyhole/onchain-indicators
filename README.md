# On-chain indicators

A plain-English reference to on-chain and market indicators for Bitcoin, Ethereum and a growing list of coins: what each one measures, how it is calculated, how it is usually read and where it falls short.

Every entry links to a live chart on [CryptoKeyhole](https://cryptokeyhole.com), an independent analytics site. The charts are computed from CryptoKeyhole's own Bitcoin and Ethereum nodes (and its Litecoin, Dogecoin and Bitcoin Cash nodes for those chains) plus market and fund data it collects, and they refresh every hour. Most entries also link to a longer guide with the indicator's full history, zone tables and common questions.

## Contents

- [Conventions](#conventions)
- [Cycle and valuation](#cycle-and-valuation)
  - [Bitcoin Cycle Composite Score](#bitcoin-cycle-composite-score) · [MVRV Ratio](#mvrv-ratio) · [MVRV Z-Score](#mvrv-z-score) · [NUPL](#net-unrealized-profitloss-nupl) · [Reserve Risk](#reserve-risk) · [RHODL Ratio](#rhodl-ratio) · [CVDD](#cvdd-cumulative-value-days-destroyed) · [Balanced Price](#bitcoin-balanced-price) · [Mayer Multiple](#mayer-multiple) · [Pi Cycle Top](#pi-cycle-top-indicator) · [Pi Cycle Bottom](#pi-cycle-bottom-indicator) · [Drawdown From All-Time High](#bitcoin-drawdown-from-all-time-high) · [NVT Signal](#nvt-signal-raw-output-90-day)
- [Price models](#price-models)
  - [Market Cap](#bitcoin-market-cap) · [200-Week Moving Average](#bitcoin-200-week-moving-average) · [2-Year Moving Average Multiplier](#bitcoin-2-year-moving-average-multiplier) · [Golden Ratio Multiplier](#golden-ratio-multiplier) · [Bull Market Support Band](#bull-market-support-band) · [Power Law](#bitcoin-power-law) · [Logarithmic Regression Bands](#bitcoin-logarithmic-regression-bands) · [Monthly Returns](#bitcoin-monthly-returns)
- [Holder behaviour](#holder-behaviour)
  - [Realized Price](#bitcoin-realized-price) · [SOPR](#spent-output-profit-ratio-sopr) · [Long-Term Holder Supply](#long-term-holder-supply) · [Dormant Supply](#bitcoin-dormant-supply) · [Coin Days Destroyed](#coin-days-destroyed-cdd) · [Average Coin Dormancy](#average-coin-dormancy)
- [Mining and network](#mining-and-network)
  - [Hash Rate](#bitcoin-hash-rate) · [Hash Ribbons](#hash-ribbons) · [Difficulty Adjustment](#bitcoin-difficulty-adjustment) · [Block Time](#bitcoin-block-time) · [Puell Multiple](#puell-multiple) · [Transaction Fees](#bitcoin-transaction-fees) · [Mining Pool Concentration](#bitcoin-mining-pool-concentration) · [Active Addresses](#bitcoin-active-addresses) · [UTXO Count](#bitcoin-utxo-count) · [Dogecoin Merged Mining](#dogecoin-merged-mining) · [Bitcoin Cash Hash Rate](#bitcoin-cash-hash-rate)
- [Supply](#supply)
  - [Bitcoin Supply](#bitcoin-supply) · [Stock-to-Flow Ratio](#bitcoin-stock-to-flow-ratio) · [Litecoin Supply](#litecoin-supply) · [Dogecoin Supply and Inflation](#dogecoin-supply-and-inflation)
- [Ethereum](#ethereum)
  - [Ethereum Supply](#ethereum-supply) · [Ethereum Staking](#ethereum-staking) · [Gas Price](#ethereum-gas-price) · [Transactions per Second](#ethereum-transactions-per-second) · [Active Addresses](#ethereum-active-addresses) · [Stablecoin Supply](#stablecoin-supply-on-ethereum)
- [Exchanges, ETFs and derivatives](#exchanges-etfs-and-derivatives)
  - [Exchange Balance and Netflow](#exchange-balance-and-netflow) · [US Spot Bitcoin ETF Net Flows](#us-spot-bitcoin-etf-net-flows) · [Perpetual Funding Rate](#perpetual-funding-rate) · [Futures Open Interest](#futures-open-interest)
- [Using the data](#using-the-data)
- [Contributing](#contributing)
- [Licence](#licence)

## Conventions

- **Days are UTC.** A day's close is the BTC/USD price at the following midnight UTC. Moving averages use daily closes and need every day in their window; a missing day leaves the value undefined rather than filled in.
- **Realized value** prices every unspent output at the price of the day it was created, that is, when its coins last moved. It is the chain's best estimate of what holders collectively paid. Outputs created before CryptoKeyhole's price history begins (17 July 2010) have no known cost and are left out of cost-basis measures.
- **Coin age** is measured from block header timestamps, which miners set and which can step backwards slightly.
- **Issued supply** is counted from the chain itself: each block's coinbase payout minus the fees it collected. Rewards that miners under-claimed were never created and are not counted.
- **Plus chart** marks an indicator whose current reading is on CryptoKeyhole's Plus plan; free visitors see a fixed 2019–2022 sample covering a full cycle. Unmarked charts are free, current and have full history.

---

## Cycle and valuation

### Bitcoin Cycle Composite Score

A daily score from 0 to 100 that rolls several well-known Bitcoin cycle indicators into one number. Each input is first scored against its own previous four years, which keeps the scale comparable as each cycle's extremes get smaller.

```
z_i   = (x_i - mean of x_i over the last 1,461 days) / std dev of x_i over the same days
s_i   = min(100, max(0, 50 + 25 * z_i))
score = average of s_i across the inputs (equal weights)
```

Inputs: MVRV Z-Score, NUPL, Reserve Risk, Puell Multiple, Mayer Multiple, price against its 2-year average, and the Pi Cycle Top ratio. Reserve Risk, the Puell Multiple and the three price ratios enter as natural logarithms. Each day uses only data available up to that day.

**How to read it**
- Below 20, cold: inputs on average at least 1.2 standard deviations below their four-year norms.
- 20 to 40 cool, 40 to 60 neutral, 60 to 80 warm.
- 80 and above, hot: inputs on average at least 1.2 standard deviations above their norms.

**Limitations**
- Most inputs compare price with its own past or with holders' cost, so they tend to move together.
- It is relative: the same score corresponds to a lower absolute valuation in a calmer cycle.
- The four-year window, the ±2 range and the equal weights were set with the whole history in view.
- Warm and hot readings have also appeared in rallies that did not end a cycle.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.CycleCompositeScore) · [Guide](https://cryptokeyhole.com/learn/bitcoin-cycle-indicator/)

### MVRV Ratio

Bitcoin's market value divided by its realized value: how richly the market prices the coins compared with what their holders paid on average. For the same coins this equals price divided by the [realized price](#bitcoin-realized-price), so an MVRV of 1.5 means an aggregate unrealised profit of 50%.

```
MVRV = market value / realized value
```

Both sides use the spendable unspent outputs at the last block of each UTC day: at the day's close for market value, at each output's creation-day price for realized value.

**How to read it**
- Below 1: price below realized value; the average holder is at a loss.
- 1 to 2: moderate profit.
- 2 to 3: elevated.
- 3 to 3.7: high.
- 3.7 and above: extreme.

The bands are descriptive conventions, not levels the market obeys.

**Limitations**
- A coin's last move is not necessarily a purchase; shifting coins between your own addresses resets their recorded cost.
- Lost coins keep a very old, low cost basis and pull realized value down.
- Not a timing tool: MVRV can sit high or low for many months.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MVRVPriced) · [Guide](https://cryptokeyhole.com/learn/mvrv-ratio/) · Plus chart: free visitors see a 2019–2022 sample.

### MVRV Z-Score

The gap between Bitcoin's market value and realized value, expressed in standard deviations of market value. Scaling the gap this way makes readings from cycles of very different size easier to compare than the plain [MVRV ratio](#mvrv-ratio).

```
MVRV Z-Score = (market value - realized value) / std dev of market value
```

The standard deviation is the population standard deviation of every daily market value from 17 July 2010 up to that day. Market value covers all issued BTC; realized value covers spendable BTC with a known creation price.

**How to read it**
- Below 0: market value under realized value; the average coin is held at a loss.
- 0 to 2: moderate.
- 2 to 5: elevated.
- 5 to 7: high.
- 7 and above: extreme and rare.

Very high readings have come close to past cycle peaks and readings below zero close to past cycle lows.

**Limitations**
- Realized value only approximates cost: self-transfers reset a coin's recorded price.
- Peaks have shrunk cycle by cycle, so a fixed threshold such as 7 may not be reached again.
- The score can stay in one zone for months; it describes valuation, not timing.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MVRVZScoreKnownBasisPriced) · [Guide](https://cryptokeyhole.com/learn/mvrv-z-score/) · Plus chart: free visitors see a 2019–2022 sample.

### Net Unrealized Profit/Loss (NUPL)

The share of Bitcoin's market value that is paper profit (or, below zero, paper loss) for the market as a whole. It carries the same information as MVRV in a bounded form: it can never exceed 1, which compresses the top of the range and stretches the bottom.

```
NUPL = (market value - realized value) / market value
     = 1 - 1 / MVRV
```

**How to read it**
- Below 0, capitulation: the market as a whole at a loss.
- 0 to 0.25, hope and fear.
- 0.25 to 0.5, optimism and anxiety.
- 0.5 to 0.75, belief and denial.
- 0.75 and above, euphoria and greed.

Each zone name pairs the mood of a rising market with that of a falling one in the same range.

**Limitations**
- Recorded cost is an approximation; coins moved between a holder's own addresses take a new cost.
- Long-lost coins keep a very low cost and lift NUPL. An adjusted chart that leaves out coins unmoved for seven years or more addresses this.
- The zone names describe moods common in past cycles; they are not forecasts.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.NUPLPriced) · [Guide](https://cryptokeyhole.com/learn/nupl/) · Plus chart: free visitors see a 2019–2022 sample.

### Reserve Risk

Price set against the conviction long-term holders show by not selling. Each day that old coins stay still adds to a running "HODL bank" of forgone selling opportunity; Reserve Risk divides the price by that bank. It is low when the price is modest and holders sit tight, and high when the price is high and old coins are being spent.

```
daily component = price - 30-day moving median of (price * CDD / BTC issued so far)
HODL bank       = cumulative sum of daily components since 20 August 2010
Reserve Risk    = price / HODL bank
```

CDD is [coin days destroyed](#coin-days-destroyed-cdd). The day units cancel, leaving a plain ratio; a zero or negative bank leaves it undefined.

**How to read it**
- Read it on a log scale, because values span orders of magnitude.
- Lower is cheaper relative to holder conviction; higher is dearer.
- Compare readings within a cycle: the bank keeps growing, so the scale drifts over time.

**Limitations**
- Spending is not selling; exchange and custodian wallet housekeeping also destroys coin days.
- The bank accumulates from 2010, so it changes slowly.
- Despite the name it is not a probability, and it has no fixed thresholds.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DailyNativeReserveRisk) · [Guide](https://cryptokeyhole.com/learn/reserve-risk/) · Plus chart: free visitors see a 2019–2022 sample.

### RHODL Ratio

Compares the realized value held in coins that moved within the past week with the realized value in coins last moved one to two years ago. When prices rise fast, coins changing hands are priced at the new highs, the young slice swells against the seasoned one, and the ratio climbs: a gauge of how hot new-money flows are.

```
RHODL = realized value (outputs under 7 days old)
        / realized value (outputs 365 to under 730 days old)
        * network age in days (3 January 2009 counted as day 1)
```

The age multiplier offsets the drift that would otherwise pull the raw ratio down as more of the supply grows old.

**How to read it**
- Use a log scale; the ratio ranges over orders of magnitude.
- Higher: far more capital in just-moved coins than in seasoned ones; high readings have come in late bull markets.
- Lower: recent activity is small next to seasoned holders' capital, typical after a long decline.
- Compare peaks and troughs across cycles rather than reading a fixed threshold.

**Limitations**
- Coins moved between a holder's own addresses join the young group at that day's price.
- The age multiplier is a convention, not a law.
- Peaks have differed in size from cycle to cycle.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.CreationDayRHODLRatioPriced) · [Guide](https://cryptokeyhole.com/learn/rhodl-ratio/) · Plus chart: free visitors see a 2019–2022 sample.

### CVDD (Cumulative Value-Days Destroyed)

A slowly rising model price built from the dollar value of every coin day ever destroyed, spread over the network's age. In past cycles the BTC price has rarely closed below it, and cycle lows have come near it.

```
CVDD = cumulative sum of (CDD on the day * price on the day)
       / (network age in days * 6,000,000)
```

CDD is [coin days destroyed](#coin-days-destroyed-cdd), valued at the day's close and accumulated from 17 July 2010, when the price history begins. The 6,000,000 is a scaling constant chosen so the line sat near earlier cycle lows; it is not a count of coins.

**How to read it**
- Price far above CVDD: typical of bull markets.
- Price approaching CVDD: near levels that have been close to the bottom of the range in past cycles.
- Price below CVDD: rare.

**Limitations**
- Designed on the past: the constant was fitted to earlier lows, so treat the line as a reference level, not a guaranteed floor.
- Every spend counts, including transfers between a holder's own addresses.
- Coin days destroyed before July 2010 have no price and are excluded.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.CVDDPricedPeriod) · [Guide](https://cryptokeyhole.com/learn/cvdd/) · Plus chart: free visitors see a 2019–2022 sample.

### Bitcoin Balanced Price

The [realized price](#bitcoin-realized-price) minus a "transferred price" derived from the dollar value of all coin days destroyed. The result is a lower, slower line that the BTC price has dipped beneath only in the deepest parts of past bear markets, which puts it in the same family of on-chain floor levels as [CVDD](#cvdd-cumulative-value-days-destroyed).

```
transferred price = cumulative sum of (CDD on the day * price on the day)
                    / network age in days / BTC issued
balanced price    = realized price - transferred price
```

**How to read it**
- Price well above the balanced price: the normal state.
- Price near or below it: historically rare, seen in deep bear markets.

The line can fall as well as rise, because the transferred price can outpace the realized price when old coins move at high prices. It can even go negative, and it is never clamped.

**Limitations**
- A model level, not a floor: the price has closed below it.
- Self-transfers destroy coin days and feed the transferred price.
- Its record as a bottom level rests on only a few bear markets.
- The two terms cover slightly different coins (priced spendable BTC and all issued BTC), so it is not a complete valuation.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.BalancedPricePricedPeriod) · [Guide](https://cryptokeyhole.com/learn/balanced-price/) · Plus chart: free visitors see a 2019–2022 sample.

### Mayer Multiple

Bitcoin's daily close divided by its 200-day simple moving average. Expressing price as a multiple of a slow trend line, rather than a dollar gap, makes readings comparable across years when Bitcoin traded at very different prices.

```
Mayer Multiple = daily close / mean of the last 200 daily closes
```

**How to read it**
- Below 0.8, deep discount: more than 20% under the 200-day average.
- 0.8 to 1.0: below the average.
- 1.0 to 1.5: above the average, the most common zone in an uptrend.
- 1.5 to 2.4: stretched.
- 2.4 and above: overheated; rare and historically brief.

What share of past days had a lower multiple is often a more useful framing than the zone alone.

**Limitations**
- It places price relative to its trend; it does not say where price is going. Long spells above and below 1 have both happened.
- The 200-day window is arbitrary, and a reading near a boundary can change zone on a small move.
- Bitcoin's earliest years produced far higher and lower multiples than recent cycles, so compare with recent history.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MayerMultiple) · [Guide](https://cryptokeyhole.com/learn/mayer-multiple/)

### Pi Cycle Top Indicator

Two moving averages of Bitcoin's daily close: a fast 111-day average and twice a slow 350-day average. The signal is the rare day when the fast line closes above the slow one, which only happens after prices over a few months have averaged more than double their yearly average. In earlier cycles that came close to the cycle high. The name comes from 350 / 111 being about 3.15, close to π.

```
fast  = SMA(close, 111)
slow  = 2 * SMA(close, 350)
cross = fast closes above slow after closing at or below it the day before
```

**How to read it**
- Far apart: the fast line more than 25% below the slow line, where it spends most of its time.
- Approaching: a gap of 10% to 25%.
- Within 10%: the lines are close, and further gains move the fast line quickly.
- Crossed: the 111-day average closes above twice the 350-day average.

**Limitations**
- The windows were chosen with hindsight on a small number of cycles.
- A peak without a steep run-up may never trigger it.
- A cross says nothing about how far or when the price falls afterwards.
- It uses price alone.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DailyPriceCycle111350) · [Guide](https://cryptokeyhole.com/learn/pi-cycle-top/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.PiCycleTop), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.PiCycleTop), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.PiCycleTop), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.PiCycleTop)

### Pi Cycle Bottom Indicator

The mirror image of the Pi Cycle Top: a 150-day exponential average of the daily close against 0.745 times the 471-day simple average. After a long, deep decline the faster average drops quickly while the slow one still holds more than a year of higher prices; the day the fast line closes below the scaled slow line is the signal. 471 / 150 is also about 3.14.

```
fast  = EMA(close, 150)   smoothing 2/151, seeded with the simple mean of its first 150 closes
slow  = 0.745 * SMA(close, 471)
cross = fast closes below slow after closing at or above it the day before
```

**How to read it**
- Far above (more than 25%): the usual state through bull markets and recoveries.
- Within 25%: the medium-term average has fallen back towards the scaled long-term one.
- Crossed below: past crosses came during deep bear markets.

**Limitations**
- Very few signals, so the track record is thin.
- The 471-day window and the 0.745 factor were fitted to earlier lows.
- Both averages lag by months; the cross confirms a deep decline rather than anticipating it, and the lowest close can come before or after it.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.PiCycleBottom) · [Guide](https://cryptokeyhole.com/learn/pi-cycle-bottom/)

### Bitcoin Drawdown From All-Time High

How far the latest daily close sits below the highest close so far. It is the most direct, model-free measure of how much pain a market has inflicted, and of how far it must climb to make the latest buyers at the top whole.

```
drawdown = (close / highest close to date - 1) * 100%
```

The running high never resets. The recovery arithmetic is uneven: a 50% fall needs a 100% rise to recover, and an 80% fall needs 400%.

**How to read it**
- 0%: a new (or equal) closing high.
- Within 20% of the high: ordinary pullbacks in a strong market.
- 20% to 50% below.
- 50% to 75% below.
- More than 75% below.

The zones are round numbers for describing depth; the guide's era table shows how deep each cycle actually went.

**Limitations**
- Uses daily closes, not intraday highs or lows, so a spike that reverses within a day does not set a new high.
- For younger coins, the high is measured over a short history.
- Depth is not timing: it says how far price has fallen, not whether the fall is over.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DailyCloseDrawdownAvailableHistory) · [Guide](https://cryptokeyhole.com/learn/bitcoin-drawdown/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.PriceDrawdown), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.PriceDrawdown), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.PriceDrawdown), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.PriceDrawdown), [Chainlink](https://cryptokeyhole.com/charts/link/?metric=link.PriceDrawdown)

### NVT Signal (raw output, 90-day)

Relates the size of the Bitcoin network to how much bitcoin moves on chain. It measures how many days of recent average output volume it would take to move the entire issued supply. Because both sides are counted in BTC, a common price cancels out and no price data is needed.

```
NVT Signal = cumulative BTC issued / mean daily raw output volume over the trailing 90 days
```

The result is in days.

**How to read it**
- Higher: on-chain transfer volume is small relative to the supply.
- Lower: on-chain volume is large relative to the supply.
- Watch the trend over months rather than a single level; the 90-day mean already smooths day-to-day noise.

**Limitations**
- Raw output volume includes change outputs and transfers between a holder's own addresses, which inflates volume and lowers the ratio.
- Issued supply includes lost and unspendable coins.
- On-chain volume misses trading that happens on exchanges' internal books.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.RawOutputNVTS) · Paid chart: free visitors see a 2019–2022 sample.

---

## Price models

### Bitcoin Market Cap

The value of every bitcoin issued, at the latest daily close. It is the standard way to put assets of very different unit prices on one scale, and the base of many valuation measures such as [MVRV](#mvrv-ratio).

```
market cap = BTC issued to date * daily close
```

Supply is counted block by block from the genesis block, including the genesis reward and coins later lost.

**How to read it**
- Use a log scale, where equal distances are equal percentage changes; Bitcoin's market cap has grown by many orders of magnitude.
- Milestones and yearly ranges show how far the value has travelled within and between cycles.
- Bitcoin's share against Ether is `100 * BTC market cap / (BTC market cap + ETH market cap)`, from hourly price indexes and on-chain supply counts.

**Limitations**
- Lost coins are counted, so market cap overstates the value of coins anyone can actually sell.
- It values every coin at the marginal price; selling a large amount would move that price.
- The Bitcoin-against-Ether share is a two-asset share, not dominance of the whole crypto market.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DailyIssuedSupplyMarketValueUSD) · [Guide](https://cryptokeyhole.com/learn/bitcoin-market-cap/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.MarketCapUSD), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.MarketCapUSD), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.MarketCapUSD), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.MarketCapUSD), [Chainlink](https://cryptokeyhole.com/charts/link/?metric=link.MarketCapUSD)

### Bitcoin 200-Week Moving Average

The mean of Bitcoin's last 1,400 daily closes (200 weeks, just under four years). The window spans roughly a whole halving cycle, so over a cycle the line has behaved more like a floor beneath the price than a centre line through it. A companion heatmap colours each day by how fast the average itself is rising.

```
200WMA         = mean of the last 1,400 daily closes
multiple       = close / 200WMA
heatmap colour = 100 * (200WMA today / 200WMA 28 days earlier - 1)   (% change over four weeks)
```

**How to read it**
- Multiple below 1: price under its four-year average, which has happened on only a small share of days.
- 1 to 1.5: within 50% above the average.
- 1.5 to 3: well above it.
- Above 3: far above it, seen in the strongest parts of past cycles.
- Heatmap: deep purple at 0% or below, rising to yellow at 16% or more per four weeks. The colour lags, because the average keeps rising for a while after the price peaks.

**Limitations**
- Slow by design: it reacts over years and cannot warn of a turn.
- Not a guaranteed floor; the price has closed below it, sometimes for months.
- The 200-week window fits Bitcoin's four-year cycles so far; if cycle length changes, so will its meaning.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.TwoHundredWeekMAChange4W) · [Guide](https://cryptokeyhole.com/learn/200-week-moving-average/)

### Bitcoin 2-Year Moving Average Multiplier

Two slow lines from Bitcoin's daily close, also published as the Investor Tool: the 2-year (730-day) average and five times that average. Between them they have contained most of Bitcoin's trading days, with the price dipping below the lower line in deep bear markets and above the upper line only in the strongest phases of early cycles.

```
lower line = mean of the last 730 daily closes   (730 fixed days, not two calendar years)
upper line = 5 * lower line
multiple   = close / lower line
```

**How to read it**
- Below 1x: under the 2-year average; historically the zone of bear-market lows.
- 1x to 2.5x: above the average but well short of the upper line, where Bitcoin has spent about half its days.
- 2.5x to 5x: extended.
- 5x and above: above the upper line, reached only in earlier cycles.

**Limitations**
- Slow by design; it cannot flag a turn in advance.
- As cycle peaks have shrunk relative to their averages, the 5x line has gone unreached for years.
- Spells below the lower line have lasted from days to well over a year, and being below it has never said when it would end.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.ClosingQuoteInvestorTool) · [Guide](https://cryptokeyhole.com/learn/bitcoin-investor-tool/)

### Golden Ratio Multiplier

A ladder of price levels built on Bitcoin's 350-day simple moving average: the average itself, 1.6 times it (close to the golden ratio, 1.618) and multiples by the Fibonacci numbers 2, 3, 5, 8, 13 and 21. On a log chart the lines are evenly spaced copies of one curve, and they make Bitcoin's diminishing cycle peaks easy to see.

```
lines    = SMA(close, 350) * {1, 1.6, 2, 3, 5, 8, 13, 21}
multiple = close / SMA(close, 350)
```

**How to read it**
- Below 1x: under the 350-day average, typical of bear phases.
- 1x to 1.6x: the most common band.
- 1.6x to 2x, 2x to 3x and 3x to 5x: progressively more extended.
- 5x and above: reached only in Bitcoin's stronger, earlier cycles.

Each cycle's highest multiple has generally been lower than the last, so a price pushing into a higher band than the previous cycle reached would be unusual.

**Limitations**
- There is no known mechanism that makes Fibonacci numbers matter to a market; they are a convenient, roughly geometric ladder.
- Diminishing peaks are a pattern, not a schedule.
- Moving averages lag: after a sharp fall the price can sit below the average for months.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.GoldenRatioMultiplier) · [Guide](https://cryptokeyhole.com/learn/golden-ratio-multiplier/)

### Bull Market Support Band

The narrow band between Bitcoin's 20-week simple moving average and 21-week exponential moving average. In Bitcoin's strongest phases the price has tended to hold above it, which is how it got its name; weeks below it have usually meant the trend has weakened. Both averages are computed on daily closes, so the band moves every day.

```
20-week SMA = mean of the last 140 daily closes
21-week EMA = EMA(close, 147)   smoothing 2/148, seeded with the simple mean of its first 147 closes
state       = above both lines / between them / below both lines
```

**How to read it**
- Above the band: above both averages.
- Inside the band: usually a short-lived state while the price crosses.
- Below the band: below both averages.
- Count consecutive days on one side: a single close means little, weeks mean more. In strong years the price has spent nearly every day above it, and in bear years nearly every day below.

**Limitations**
- Five-month averages lag; the band confirms a change of trend after it has begun.
- In sideways markets the price can cross it many times, so count days rather than crossings.
- It is not support in any mechanical sense; it holds only while buyers act near it.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.BullMarketSupportBand) · [Guide](https://cryptokeyhole.com/learn/bull-market-support-band/)

### Bitcoin Power Law

Plotted with both price and time (days since the genesis block) on log scales, Bitcoin's long-run path is close to a straight line: a power law, with an exponent a little under 6 in CryptoKeyhole's fits. Rather than one fitted line, the chart uses quantile regression to fit a separate line for every level from 1% to 99%, so the bands can narrow over time, and condenses them into a 0 to 100 position.

```
for each quantile q from 1% to 99%:
    log10(price) = a_q + b_q * log10(days since 3 January 2009, genesis day = day 1)
position = price's place between the two nearest quantile lines, interpolated, kept within 0..100
```

**How to read it**
- Below 5: near the bottom of the range Bitcoin has occupied.
- 5 to 25: below trend.
- 25 to 75: around trend, where the price has spent about half its days.
- 75 to 95: above trend.
- 95 and above: near the top of the range; brief and typical of cycle peaks.

The upper lines have flattened relative to the lower ones, so a position of 80 now may reflect as much excitement as 95 did in an earlier cycle.

**Limitations**
- An empirical regularity fitted to the past, not a law; it could break.
- Coefficients are refitted once a year and the whole history redrawn, so past positions are seen with hindsight.
- Price only; no on-chain data.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.PowerLawQuantileBands) · [Guide](https://cryptokeyhole.com/learn/bitcoin-power-law/)

### Bitcoin Logarithmic Regression Bands

A single straight line fitted by ordinary least squares to log price against log time since the genesis block, with bands one, two and three standard deviations of the residuals either side. The current position is published as one number: the distance from the line in standard deviations.

```
fit:       log10(price) = a + b * log10(days since 3 January 2009)   (ordinary least squares)
band k:    line * 10^(k * sigma),   k = -3 .. +3
deviation: (log10(price) - log10(line)) / sigma
```

With the current fit, one standard deviation is close to a factor of two in price.

**How to read it**
- 0σ: on the trend line.
- +1σ is about twice the trend, +2σ about four times and +3σ about eight times.
- −1σ is about half the trend, −2σ about a quarter.
- Zones run from more than 1σ below the trend to more than 2σ above it.

**Limitations**
- Constant-width bands assume the scatter around the trend has stayed the same; it has shrunk, so the upper bands look further away than recent cycles have been able to reach.
- The fit is redone yearly and includes prices that came after many of the days it describes.
- A straight line on log scales is an observed regularity, not a forecast.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.LogRegressionBands) · [Guide](https://cryptokeyhole.com/learn/bitcoin-logarithmic-regression/)

### Bitcoin Monthly Returns

Bitcoin's price change in every calendar month of its history, laid out as a year-by-month grid with each month's average and median across the years. It is the straightforward way to check claims about seasonality.

```
monthly return = (last daily close of the month / last daily close of the previous month - 1) * 100%
```

Months are UTC calendar months. The month in progress uses the latest hourly quote and is labelled as open; averages and medians use closed months only.

**How to read it**
- Read the median alongside the average: a few early months with gains of several hundred percent dominate the averages.
- Count how many years each month rose; consistency says more than any single figure.
- Keep the sample size in mind: each calendar month contributes only one observation per year of history.
- The open month's cell moves every hour until the month closes.

**Limitations**
- Too few observations per month to establish a reliable seasonal effect.
- The market has changed a great deal since the early years that produced the extremes.
- A month's history says nothing certain about its next occurrence.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MonthlyReturns) · [Guide](https://cryptokeyhole.com/learn/bitcoin-monthly-returns/)

---

## Holder behaviour

### Bitcoin Realized Price

The average price at which all spendable bitcoin last moved on chain: realized value divided by the number of coins. It is the closest on-chain estimate of the market's aggregate cost basis. Because a coin keeps its basis until it is spent, the line moves slowly, rising through bull markets and falling far less than price in bear markets.

```
realized price = sum over spendable outputs of (BTC * price on creation day) / BTC in those outputs
```

Outputs created before 17 July 2010 have no known basis and are left out entirely. Price divided by realized price is the [MVRV ratio](#mvrv-ratio).

**How to read it**
- Price well above the realized price: large aggregate unrealised profit.
- Price approaching it from above: paper profits shrinking, a test of holders' conviction.
- Price below it: the market as a whole at a loss, which has happened only in deep bear markets.

**Limitations**
- Moving coins to a new address resets their basis whether or not they were sold.
- Coins bought on an exchange and never withdrawn have no on-chain cost of their own.
- Long-lost coins keep their very old, low basis and hold the line down.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.RealizedPricePriced) · [Guide](https://cryptokeyhole.com/learn/realized-price/) · Plus chart: free visitors see a 2019–2022 sample.

### Spent Output Profit Ratio (SOPR)

Whether the coins moving on chain each day are, on balance, realising a profit or a loss. Every spent output is valued twice, at the price of the day it is spent and at the price of the day it was created, and the totals are compared.

```
SOPR = sum(BTC spent * price on spending day) / sum(BTC spent * price on creation day)
```

It is one value-weighted ratio across all spends (newly mined coins excluded), using day-start prices.

**How to read it**
- Above 1: coins moved at a profit on balance.
- Near 1: break-even (a 7-day mean between 0.99 and 1.01).
- Below 1: coins moved at a loss on balance.
- Use the 7-day mean. In strong markets dips towards 1 have tended to be brief; in weak markets rises towards 1 have tended to fail. A move that holds on the other side of 1 for weeks says more than a single crossing.

**Limitations**
- Not every spend is a sale; wallet moves, consolidations and exchange deposits count and pull the ratio towards 1. An entity-adjusted version removes transfers within the same entity.
- A single movement of very old coins can dominate a day.
- Daily prices miss intraday trading; coins created and spent on the same day score exactly 1.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.SOPRPriced) · [Guide](https://cryptokeyhole.com/learn/sopr/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.SOPR), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.SOPR), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.SOPR), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.SOPR), [Chainlink](https://cryptokeyhole.com/charts/link/?metric=link.SOPR)

### Long-Term Holder Supply

The bitcoin sitting in coins that have not moved for at least 155 days, the usual on-chain dividing line between long-term and short-term holders. Coins become much less likely to be spent once they are about five months old, so this group is the stock of coins whose owners have chosen to hold through months of price moves.

```
LTH supply = BTC in spendable outputs aged 155 days or more
STH supply = BTC in younger spendable outputs
```

Ages run from the timestamp of the block that created each output; no entity adjustment is made.

**How to read it**
- Watch the direction more than the level; turning points matter most.
- It tends to rise in bear markets and quiet periods (few old coins move while recent buys age past 155 days) and fall in strong rallies, as seasoned holders sell to new buyers.
- Remember the lag: coins bought today join the long-term group about five months later.

**Limitations**
- The groups describe coins, not people; one person can hold both.
- Consolidations and custody changes make old coins look young.
- Coins held for a long time in exchange wallets count as long-term.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.Old155dUtxoSupply) · [Guide](https://cryptokeyhole.com/learn/long-term-holder-supply/) · Plus chart: free visitors see a 2019–2022 sample.

### Bitcoin Dormant Supply

The share of all spendable bitcoin that has not moved for a year or more. It is one band of the supply-by-age view, singled out because it is easy to read and follows a recognisable cycle pattern.

```
dormant share = BTC in outputs aged 365 days or more / all spendable BTC * 100%
```

Measured at the last block of each UTC day; the genesis reward and provably unspendable outputs are excluded.

**How to read it**
- Rising: coins are ageing faster than old coins are being spent; holders are sitting tight. This is typical of bear markets.
- Falling: long-held coins are moving, often being distributed into a rising market.
- The lag: a coin bought today only enters the share a year later, so a rise reflects buying from a year before.

**Limitations**
- Lost coins and patiently held coins look identical on chain.
- An exchange or custodian moving coins between its own wallets makes old coins look new.
- Age belongs to outputs, not people. For the 155-day convention, see [Long-Term Holder Supply](#long-term-holder-supply).

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.UtxoAgeOver1Y) · [Guide](https://cryptokeyhole.com/learn/bitcoin-dormant-supply/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.SupplyLastActiveOver1YPercent), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.SupplyLastActiveOver1YPercent), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.SupplyLastActiveOver1YPercent), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.SupplyLastActiveOver1YPercent), [Chainlink](https://cryptokeyhole.com/charts/link/?metric=link.SupplyLastActiveOver1YPercent)

### Coin Days Destroyed (CDD)

Every bitcoin earns one coin day for each day it stays in the same output, and loses them all when it is spent. Coin days destroyed adds up those spent days across everything that moved, so old coins waking up show as spikes: moving 1 BTC that is a day old destroys 1 coin day, while moving 1 BTC untouched for ten years destroys about 3,650.

```
CDD = sum over spent outputs of (BTC spent * days since the output was created)
```

Ages come from block header times and are never negative. CDD feeds [Reserve Risk](#reserve-risk), [CVDD](#cvdd-cumulative-value-days-destroyed) and [dormancy](#average-coin-dormancy).

**How to read it**
- Use an average, such as the 30-day mean; a single old wallet can make one day spike enormously.
- Look at big spikes individually and check what is known about the coins that moved.
- Watch the baseline: a 30-day mean rising over months says older coins in general are moving more. Sustained high CDD in a rising market has been read as long-term holders distributing.

**Limitations**
- Not proof of selling; exchange and custodian wallet maintenance can destroy huge numbers of coin days.
- One entity can outweigh a month of ordinary activity.
- Age is tracked per output, so splitting or merging coins resets it even if the owner has not changed.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.CoinDaysDestroyed) · [Guide](https://cryptokeyhole.com/learn/coin-days-destroyed/) · Plus chart: free visitors see a 2019–2022 sample.

### Average Coin Dormancy

The value-weighted average age of the bitcoin spent each day: how long, on average, each spent coin had sat still before it moved. It is [coin days destroyed](#coin-days-destroyed-cdd) per bitcoin spent, so it separates "old coins moving" from "a lot of coins moving".

```
dormancy = sum(BTC spent * days since the output was created) / sum(BTC spent)
         = CDD / BTC spent
```

Ages are measured from block header times and are never negative.

**How to read it**
- Higher: the coins changing hands are older on average.
- Lower: activity is dominated by recently moved coins.
- Rising dormancy with steady volume means older coins are moving. Compare the trend with price and with CDD.

**Limitations**
- Every spent output counts, so wallet reshuffles by exchanges and custodians can lift a day; an entity-adjusted dormancy chart is also published.
- Value weighting lets a single large movement of old coins dominate a day.
- Spending is not necessarily selling.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.Dormancy)

---

## Mining and network

### Bitcoin Hash Rate

The number of attempts at the mining puzzle the whole network makes each second, now counted in exahashes per second (EH/s, 10^18 hashes a second). It is the best single measure of the work, and therefore the electricity and hardware, protecting the chain. Nobody can measure it directly; it is estimated from the blocks found.

```
hash rate = sum over the day's blocks of (difficulty * 2^32) / 86,400 seconds
```

`difficulty * 2^32` is the expected number of hashes needed to find a block at that difficulty.

**How to read it**
- Use the 30-day average. Block discovery is random: a day that should produce 144 blocks can easily produce anywhere from about 125 to 165.
- Read it on a log scale; growth has slowed as mining hardware has matured.
- A rising hash rate means miners expect mining to pay; a sustained fall usually means revenue per unit of work has dropped below many miners' costs.

**Limitations**
- Luck makes single days, and even single weeks, noisy.
- It is an estimate; sources using different windows or timestamp rules publish different numbers.
- It measures work done, not whether that work pays.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.HashRateEstimate) · [Guide](https://cryptokeyhole.com/learn/bitcoin-hash-rate/) · Also for [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.HashRate), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.HashRate), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.HashRate)

### Hash Ribbons

The 30-day and 60-day moving averages of Bitcoin's estimated hash rate, drawn together as a ribbon. Normally the network grows and the faster line stays on top. When miners switch machines off faster than others switch them on, the 30-day line sinks below the 60-day line; that inversion, often called miner capitulation, is what the indicator watches for.

```
fast = SMA(daily hash rate estimate, 30)
slow = SMA(daily hash rate estimate, 60)
```

**How to read it**
- 30-day above 60-day: hash rate growing, the normal state.
- 30-day below 60-day: hash rate falling; miners under stress.
- The depth of the gap shows severity: a few percent is common, gaps near 20% are rare.
- The cross back up marks the end of the stretch, a sign the weakest miners have gone and growth has resumed.

**Limitations**
- Hash rate is inferred from blocks; luck alone moves the 30-day average by a few percent.
- A fall can come from economics, regulation, weather or a pool outage, and the ribbons do not say which.
- The 60-day average reacts slowly, so the signal confirms rather than anticipates.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.HashRateRibbon) · [Guide](https://cryptokeyhole.com/learn/hash-ribbons/) · Plus chart: free visitors see a 2019–2022 sample.

### Bitcoin Difficulty Adjustment

How hard it is to find a block, and the estimate for the next reset. Every 2,016 blocks (an epoch of about two weeks) every node recalculates the target so that blocks keep arriving about every ten minutes whatever the hash rate. At difficulty D, a block takes about D × 2^32 hashes on average.

```
new difficulty = old difficulty * (2,016 * 600 s) / actual time the epoch took
                 (limited to a factor of four either way)
estimate       = 100 * (1,209,600 / (2,015 * tau) - 1) %
```

`tau` is the average block time so far in the current epoch, in seconds. The protocol times an epoch from its first to its last block, which spans 2,015 intervals, a quirk every node still follows.

**How to read it**
- Positive estimate: blocks are coming faster than every ten minutes because hash rate has grown since the last adjustment.
- Negative estimate: blocks are slower; hash rate has fallen.
- Early in an epoch the estimate rests on few blocks and can swing by several points a day; by the final block it matches the actual change.

**Limitations**
- The estimate assumes the epoch's pace so far continues.
- Adjustments respond to the past and lag hash rate by up to two weeks.
- Timestamps are set by miners and can be off by up to about two hours.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DifficultyAdjustmentEstimate) · [Difficulty history](https://cryptokeyhole.com/charts/btc/?metric=ckh.DifficultyEstimatedHashes) · [Guide](https://cryptokeyhole.com/learn/bitcoin-difficulty-adjustment/)

### Bitcoin Block Time

How long Bitcoin blocks are actually taking, against the ten-minute design target. Finding a block is a lottery, so individual gaps are random; difficulty only steers the average. Block time sets how fast transactions confirm and coins are issued, and its drift moves halving dates.

```
block time = mean over the day's blocks of (header timestamp - parent header timestamp)
```

Negative differences (header times can step back) are kept, so the figure is an honest average of the recorded times.

**How to read it**
- Use the 30-day average; a single day can easily average eight or twelve minutes.
- Below 600 seconds: blocks faster than target, usually because hash rate has grown since the last [difficulty adjustment](#bitcoin-difficulty-adjustment).
- Above 600 seconds: slower blocks, often after miners switch off.
- 144 blocks a day corresponds to exactly ten minutes per block.

**Limitations**
- Miners set timestamps, so single intervals can be wrong by minutes; averages over many blocks are reliable.
- Even with a perfectly steady hash rate, block times vary widely.
- It is not confirmation time, which also depends on a transaction's fee and the competition for block space.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.SignedBlockIntervalMean) · [Guide](https://cryptokeyhole.com/learn/bitcoin-block-time/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.BlockIntervalMean), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.BlockIntervalMean), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.BlockIntervalMean), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.BlockIntervalMean)

### Puell Multiple

The dollar value of the bitcoin miners create each day, divided by its average over the past year. New-coin issuance barely changes between halvings, so the multiple mostly tracks price against its yearly average, with a sharp drop at each halving. It is read as a gauge of miners' fortunes and of the selling pressure their new coins can bring.

```
Puell Multiple = USD value of today's issuance / mean daily issuance value over the 365 days ending today
```

Issuance is each block's coinbase payout minus its fees, so transaction fees are excluded.

**How to read it**
- Below 0.5: very low; dollar issuance under half its yearly norm.
- 0.5 to 1: below average.
- 1 to 2: above average.
- 2 to 4: high.
- 4 and above: extreme.
- After a halving the multiple drops regardless of the market and recovers over the following year, so a low reading just after a halving means something different from one mid-cycle.

**Limitations**
- Fees are left out; in high-fee periods miners earn far more than the multiple suggests.
- The halving dominates the series every four years.
- Large miners hedge, borrow and hold, so their selling does not track the multiple closely.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.DailyIssuanceValueMultiple365D) · [Guide](https://cryptokeyhole.com/learn/puell-multiple/) · Plus chart: free visitors see a 2019–2022 sample.

### Bitcoin Transaction Fees

What users paid miners to confirm their transactions each day: the total, the median per transaction, and fees as a share of miners' income. Fees show how much people will pay for scarce block space, and they matter for long-run security because the new-coin subsidy halves every four years.

```
fee per transaction = sum(inputs) - sum(outputs)       (non-coinbase transactions)
daily total         = sum of fees in the UTC day's blocks
fee share           = fees / (fees + newly issued coins)
```

**How to read it**
- Use the 30-day average for trends; the chart plots it on a log scale.
- Use the median for what a typical transaction paid; a few huge fees can dominate the total.
- Watch the fee share: it shows whether fees are a meaningful part of miners' income or still small next to new coins.

Fees spike when demand to transact outruns capacity: panics, rallies and waves of new kinds of transactions.

**Limitations**
- These are amounts paid per transaction, not fee rates per virtual byte; a large transaction pays more for the same priority.
- Batching means one transaction can pay hundreds of recipients.
- A single transaction with an accidental huge fee can lift a day's total.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=fees.VolumeSum) · [Guide](https://cryptokeyhole.com/learn/bitcoin-transaction-fees/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.FeesTotal), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.FeesTotal), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.FeesTotal), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.FeesTotal)

### Bitcoin Mining Pool Concentration

How concentrated Bitcoin's block production is. Pools coordinate many miners and decide what goes into each block, so the spread of blocks across pools is the clearest picture of who controls production. The chart shows the daily share of blocks mined by the largest one, three and five pools, plus the Nakamoto coefficient.

```
top-N share         = blocks mined by the N largest pools / all blocks that day   (unidentified blocks included)
Nakamoto coefficient = fewest pools whose blocks together exceed half of the day's blocks
```

Blocks are attributed by coinbase tag or payout address using an open, MIT-licensed set of pool definitions. The coefficient is shown with pools as tagged, and with pools found mining on one large pool's block templates counted as a single group.

**How to read it**
- Use 30-day averages; with about 144 blocks a day, daily shares move several points by chance.
- The five-largest share is the steadiest summary; the largest-pool line shows whether any one pool is nearing dangerous levels.
- A coefficient of 2 or 3 means two or three pools mined a majority.

**Limitations**
- Shares count blocks found, which follow hash rate only on average.
- Tags can be copied, and one operator can run several pools.
- Miners can switch pools, so shares can change quickly. Before 2013 most blocks carried no recognisable tag.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.PoolTopShares) · [Guide](https://cryptokeyhole.com/learn/bitcoin-mining-pools/)

### Bitcoin Active Addresses

The number of distinct addresses that sent or received bitcoin each day. An address counts once per UTC day however many transactions it took part in. It is the broadest simple gauge of how much the chain is being used.

```
active addresses = count of distinct addresses among the inputs and outputs of the day's transactions
```

Public keys paid directly and the addresses derived from the same keys count as one; outputs with no standard address, such as data-carrying outputs, are left out. Addresses are not linked to owners.

**How to read it**
- Use the 30-day average; weekends are quieter and one-off events, such as an exchange consolidating thousands of deposit addresses, can lift a single day.
- Compare like with like, for example this year's 30-day average with last year's.
- Expect structural shifts: batching, address reuse and payments moving to Lightning change the count without any change in users.

**Limitations**
- Addresses are not people; one user can have hundreds and one exchange millions.
- Spam and consolidations inflate the count without economic activity behind it.
- Layer 1 only: Lightning payments and exchanges' internal transfers do not appear.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=addresses.ActiveCount) · [Guide](https://cryptokeyhole.com/learn/bitcoin-active-addresses/) · Also for [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.ActiveAddresses), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.ActiveAddresses), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.ActiveAddresses)

### Bitcoin UTXO Count

How many unspent transaction outputs (UTXOs) Bitcoin's ledger holds. Bitcoin stores no balances, only outputs; the set of outputs not yet spent is the whole state every node keeps at hand to validate new transactions, so its size bears on the cost of running a node.

```
UTXO count = number of unspent outputs at the highest block of each UTC day
```

The genesis output, provably unspendable outputs (such as data carriers) and the two coinbase outputs overwritten by duplicate transaction IDs in 2010 are excluded, as Bitcoin Core does.

**How to read it**
- Steady growth reflects more users and payments over the years.
- Sudden jumps come from waves of outputs created for reasons other than payments, often very small ones (as with inscriptions and token experiments).
- Declines come from consolidation, usually when fees are low enough to make merging small outputs worthwhile.

**Limitations**
- It counts outputs, not value: one satoshi counts the same as a thousand bitcoin.
- Wallets, exchanges and token protocols decide how many outputs they create, so it is not a user count.
- Dust that is uneconomic to spend may never leave the set.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=blockchain.UtxoCount) · [Guide](https://cryptokeyhole.com/learn/bitcoin-utxo-count/) · Also for [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.UTXOCount), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.UTXOCount), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.UTXOCount)

### Dogecoin Merged Mining

Dogecoin and Litecoin share the Scrypt hash function, and since September 2014 Dogecoin has accepted auxiliary proof of work (AuxPoW): a block can be proven by work done for a parent chain, provided that work commits to the Dogecoin block and meets Dogecoin's own target. In practice Litecoin pools mine both chains with the same hashes, so almost every Dogecoin block is merge-mined.

```
merge-mined share = Dogecoin blocks carrying the AuxPoW flag and parent data / all Dogecoin blocks that day
hash rate (each)  = sum over the day's blocks of (that chain's difficulty * 2^32) / seconds in the day
```

**How to read it**
- The share has been close to 100% almost every day since the change; a dip means some blocks were mined for Dogecoin alone.
- Two nearly identical hash-rate lines show the same Scrypt work securing both chains.

**Limitations**
- Hash rates are estimates inferred from blocks found; small differences between the chains are noise and timing.
- Parent chains are not identified block by block; almost all the work comes from Litecoin, but other Scrypt chains can act as parents.
- Merged mining brought Dogecoin far more hash rate than it could attract alone, but the same few pools now produce most blocks on both chains.

[Live chart](https://cryptokeyhole.com/charts/doge/?metric=doge.MergeMinedBlockShare) · [Hash rate chart](https://cryptokeyhole.com/charts/doge/?metric=doge.HashRate) · [Guide](https://cryptokeyhole.com/learn/dogecoin-merged-mining/)

### Bitcoin Cash Hash Rate

Bitcoin Cash split from Bitcoin in August 2017 and kept its SHA-256 proof of work, so the same machines can mine either chain and switch in minutes. This measures Bitcoin Cash's share of the combined work on the two chains, which is the clearest gauge of how small a slice of Bitcoin's mining power could overpower it.

```
share = BCH work / (BCH work + BTC work) * 100%
work  = sum over the UTC day's blocks of (difficulty * 2^32)
```

Both chains are read from their own nodes on the same block clock.

**How to read it**
- Use the 30-day average; each chain's day rests on about 144 blocks.
- A share of 1% means roughly 1% of Bitcoin's mining power could match all of Bitcoin Cash's.
- Watch the trend on a log scale: a falling share means Bitcoin Cash is attracting less of the available SHA-256 work. Miners follow revenue per unit of work, so the share also moves with the two coins' prices and fees.

**Limitations**
- Two chains only; other SHA-256 coins compete for the same machines.
- Work is estimated from difficulty and blocks found.
- Bitcoin Cash adjusts difficulty every block, so miners move in and out quickly and daily values jump.

[Live chart](https://cryptokeyhole.com/charts/bch/?metric=bch.Sha256HashRateShare) · [Guide](https://cryptokeyhole.com/learn/bitcoin-cash-hash-rate/)

---

## Supply

### Bitcoin Supply

How many bitcoin have been issued so far, counted block by block, and how much of the 21 million cap that represents. New coins appear only as block rewards, which started at 50 BTC and halve every 210,000 blocks (about four years); the schedule sums to just under 21 million, reached around the year 2140.

```
issued supply = sum over every block since genesis of (coinbase payout - fees collected)
share of cap  = issued supply / 21,000,000
```

**How to read it**
- Issued supply counts every coin created, including the genesis block's 50 BTC, which can never be spent.
- Circulating supply is the same thing for Bitcoin; there are no locked or treasury allocations.
- Spendable supply is smaller by the unspendable coins and an unknowable amount of lost ones.
- The next halving falls at the next multiple of 210,000 blocks; its date is an estimate from the recent average block interval.

**Limitations**
- Lost coins cannot be identified, so the issued total overstates what can actually be spent.
- Halvings are set by block height, so their dates depend on hash rate.
- Because under-claimed rewards are not counted, the figure can differ slightly from one computed from the schedule alone.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.CumulativeIssued) · [Guide](https://cryptokeyhole.com/learn/bitcoin-supply/) · Also for [Ethereum](https://cryptokeyhole.com/charts/eth/?metric=eth.CirculatingSupply), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.CirculatingSupply), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.CirculatingSupply), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.CirculatingSupply)

### Bitcoin Stock-to-Flow Ratio

A scarcity measure borrowed from commodity markets: the existing stock divided by the past year's new production, giving the number of years current issuance would take to recreate the supply. Bitcoin's issuance is fixed in its rules, so the ratio is plain accounting and can be computed exactly from the chain.

```
stock-to-flow = BTC issued so far / BTC issued over the past 365 days      (years)
```

**How to read it**
- The level shows how scarce new supply is; a high ratio means a year's issuance adds very little to the total.
- Halvings drive the steps: as the trailing year fills with lower-reward blocks, the ratio roughly doubles over the twelve months after each halving.
- Small dips and bumps between halvings come from blocks arriving slightly faster or slower than every ten minutes.

**Limitations**
- The stock includes lost and unspendable coins.
- Comparisons with gold and other metals mix exact chain data with survey estimates.
- It says nothing about demand and is not a valuation. Models that turn the ratio into a price forecast are a separate, much-debated matter; CryptoKeyhole publishes the ratio, not a price model.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.StockToFlow365D) · [Guide](https://cryptokeyhole.com/learn/stock-to-flow/) · Also for [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.StockToFlow), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.StockToFlow), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.StockToFlow)

### Litecoin Supply

How many litecoin have been issued, counted block by block from Litecoin's genesis block of 7 October 2011. Litecoin copies Bitcoin's supply design at four times the scale: 2.5-minute blocks, a starting reward of 50 LTC, a halving every 840,000 blocks (about four years) and a cap of 84 million LTC.

```
issued supply = sum over every block since genesis of (coinbase payout - fees collected)
share of cap  = issued supply / 84,000,000
```

Coins inside Litecoin's MimbleWimble extension blocks (MWEB, since May 2022) are confidential and counted as one pool that grows with peg-ins and shrinks with peg-outs and fees.

**How to read it**
- The share of the 84 million cap shows how much supply remains to be mined.
- Each halving halves the flow of new coins and doubles the [stock-to-flow ratio](#bitcoin-stock-to-flow-ratio).
- The next halving falls at the next multiple of 840,000 blocks; its date is estimated from the recent average block interval.

**Limitations**
- Coins whose keys are lost still count as issued.
- The halving date is an estimate, since block times vary with hash rate.
- Coins sent to unspendable outputs after issuance are not subtracted.

Litecoin's work also secures Dogecoin through [merged mining](#dogecoin-merged-mining).

[Live chart](https://cryptokeyhole.com/charts/ltc/?metric=ltc.CirculatingSupply) · [Guide](https://cryptokeyhole.com/learn/litecoin-supply/)

### Dogecoin Supply and Inflation

How many dogecoin exist, and how fast the supply grows. After early periods of large and at times random rewards, every block since block 600,000 (early 2015) issues a fixed 10,000 DOGE with no cap. With blocks about once a minute, that adds roughly 5 billion DOGE a year, indefinitely.

```
issued supply  = sum over every block since genesis of (coinbase payout - fees collected)
inflation rate = DOGE issued over the past year / supply a year earlier
```

**How to read it**
- In absolute terms issuance is roughly constant every year.
- In percentage terms it falls every year, because the same number of new coins is added to an ever larger total. The rate approaches zero but never reaches it: unlimited, yet disinflationary.
- Compared with capped coins, a fixed reward keeps paying miners and helps replace lost coins, at the cost of permanent, if shrinking, dilution.

**Limitations**
- Lost coins stay in the total.
- Coins sent to unspendable outputs after issuance are not subtracted.
- The yearly amount varies slightly with how many blocks are found around the one-minute target.

[Live chart](https://cryptokeyhole.com/charts/doge/?metric=doge.CirculatingSupply) · [Guide](https://cryptokeyhole.com/learn/dogecoin-supply/)

---

## Ethereum

### Ethereum Supply

All ETH in existence, and whether it is growing or shrinking. Ether has no fixed schedule: new ETH is issued to validators (to miners before the Merge of 15 September 2022), and since the London upgrade of August 2021 the base fee of every transaction is burned.

```
supply = ETH held by execution-layer accounts
       + consensus-layer ledger (staking deposits - withdrawals + validator rewards net of penalties)
```

Measured at the end of each UTC day from an execution archive node and a consensus node; deposits the consensus layer rejected are not counted.

**How to read it**
- A falling line means the burn exceeded issuance over that stretch: net deflation.
- A rising line means issuance exceeded the burn.
- The slope matters more than the level; the annualised 30-day pace is the best single summary of the current direction.
- Busy blocks raise the burn ([gas price](#ethereum-gas-price)); more stake raises issuance ([staking](#ethereum-staking)).

**Limitations**
- Staked ETH counts, although it cannot be traded until withdrawn.
- ETH sent to unusable addresses still counts; only the burn removes ETH.
- A few busy days can swing the 30-day pace, so compare it with yearly changes.

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.CirculatingSupply) · [Guide](https://cryptokeyhole.com/learn/ethereum-supply/)

### Ethereum Staking

How much of all ETH is staked to secure Ethereum, how much has been deposited since staking began in November 2020, and how long the entry and exit queues are. The stake sets the security budget and the issuance rate, and the queues show whether more ETH is trying to join or to leave.

```
staked share   = effective balance of active validators / all ETH in existence
total deposits = balance of the staking deposit contract (cumulative; never reduced by withdrawals)
queue wait     = queue length / churn limit per epoch, in days of 225 epochs
```

A second share counts everything on the consensus layer, including balances above a validator's effective cap and ETH waiting in the queues.

**How to read it**
- The share staked moves slowly; a percentage point takes weeks or months.
- The deposit total only rises.
- Queue waits can change quickly: when entries wait longer than exits, more ETH wants to join than leave, and the reverse.

**Limitations**
- Staked is not the same as illiquid; much staked ETH is represented by liquid staking tokens that trade and serve as collateral.
- The deposit total is not current stake, because withdrawals are paid by the protocol, not out of the contract.
- Waits assume the queue and churn limit stay as they are.

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.StakedShareOfSupply) · [Deposits chart](https://cryptokeyhole.com/charts/eth/?metric=eth.TotalValueStaked) · [Guide](https://cryptokeyhole.com/learn/ethereum-staking/)

### Ethereum Gas Price

The median gas price Ethereum transactions actually paid each day, in gwei (a billionth of an ETH). Gas measures computation: a plain ETH transfer uses 21,000 gas and a swap several times that. The gas price mostly reflects demand for block space against the capacity on offer.

```
fee                 = gas used * effective gas price
effective gas price = base fee + priority fee (capped by the sender's maximum)
daily figure        = exact median of effective gas prices across the day's transactions
```

Each block's base fee rises by up to 12.5% when the previous block was more than half full and falls when it was less; the base fee is burned, while the priority fee goes to the validator.

**How to read it**
- The median is what a typical transaction paid and is hard to move with a few extreme bids.
- The mean, pulled up by the highest bidders, sits above the median.
- The long-run trend reflects capacity changes: higher gas limits, rollups batching users' transactions, and since March 2024 a separate, cheaper blob market for rollup data.
- Lower fees mean a smaller burn, so [supply](#ethereum-supply) grows faster.

**Limitations**
- It is what was paid after the fact, not a wallet's quote for the next block.
- Every transaction counts once, whether it used 21,000 gas or millions.
- Main chain only; rollup gas is priced separately.

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.GasPriceMedian) · [Guide](https://cryptokeyhole.com/learn/ethereum-gas-price/)

### Ethereum Transactions per Second

How many transactions Ethereum's main chain actually processed per second, averaged over each day. It measures what the chain did, not what it theoretically could do: maximum-throughput claims assume every block is packed with the cheapest possible transactions.

```
TPS = successful transactions in the UTC day's canonical blocks / 86,400
```

Failed transactions, which still pay fees, are left out. The hourly chart divides by 3,600.

**How to read it**
- Compare with Ethereum's own past; the trend matters more than any day, since airdrops, launches and spam waves can lift a single day sharply.
- A rollup's batch counts as one transaction here, even if it carries thousands of rollup transactions, so activity moving to rollups can lower the count.
- Gas, not count, is the real capacity: many simple transfers can show a high TPS without using more of the chain than fewer heavy contract calls.
- Cross-chain comparisons need care; a transaction is not the same unit on every chain.

**Limitations**
- Layer 1 only.
- Successful transactions only.
- Count, not value: a thousand dust transfers and a thousand large trades score the same.

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.TransactionRate) · [Guide](https://cryptokeyhole.com/learn/ethereum-transactions-per-second/) · Also for [Bitcoin](https://cryptokeyhole.com/charts/btc/?metric=transactions.Rate), [Litecoin](https://cryptokeyhole.com/charts/ltc/?metric=ltc.TransactionRate), [Dogecoin](https://cryptokeyhole.com/charts/doge/?metric=doge.TransactionRate), [Bitcoin Cash](https://cryptokeyhole.com/charts/bch/?metric=bch.TransactionRate)

### Ethereum Active Addresses

The number of ordinary Ethereum accounts (externally owned accounts, not smart contracts) that sent a transaction or received ETH each day. It is the broadest simple measure of how many accounts use the main chain.

```
active = distinct externally owned accounts that, in the UTC day,
         originated a transaction OR had their ETH balance increased by a transfer
```

Contracts are excluded, as are credits that are not transfers: block rewards, fee payments to the block's recipient and staking withdrawals. Each address counts once.

**How to read it**
- Use the 30-day average; airdrops, token launches and spam waves can multiply one day's count.
- Compare years rather than days.
- Read it with fees: a high count with low fees can mean cheap, automated activity, while a high count with high fees means many accounts are paying up to transact.

**Limitations**
- Addresses are not people; wallets, services and bots create many.
- Cheap transactions make spam and airdrop farming show up here.
- Main chain only: rollup users appear only indirectly, and receiving only tokens (no ETH) does not make an address active.

The definition is close in spirit, but not identical, to the input-and-output count used for [Bitcoin](#bitcoin-active-addresses).

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.ActiveAddresses) · [Guide](https://cryptokeyhole.com/learn/ethereum-active-addresses/)

### Stablecoin Supply on Ethereum

The total of US-dollar stablecoins issued on Ethereum, read from each token's own contract and counted at one dollar per token. Issuers mint when users deposit dollars or collateral and burn on redemption, so the total is a running count of dollars brought on chain and not yet taken out, one of the few on-chain numbers that directly reflects capital arriving or leaving.

```
supply = sum over tracked tokens of (contract total supply * $1), at the end of each UTC day
```

Tracked tokens are US-dollar stablecoins with about $500 million or more of supply on Ethereum, now or at their peak (such as USDT, USDC and DAI), plus formerly large ones for their history.

**How to read it**
- Rising: minting exceeds redemptions; new dollars are arriving.
- Falling: redemptions exceed minting; dollars are leaving or moving to other chains.
- Flat in a rising market: the rally is not being funded by new stablecoin issuance.
- Check the composition: a steady total can hide one token shrinking while another grows.

**Limitations**
- Ethereum only; issuance on other chains is excluded, so it is not the whole stablecoin market.
- Tokens count at face value whatever they trade at.
- Supply is not demand: tokens minted into an issuer's treasury count, and a stablecoin held as backing for another is counted in both.

[Live chart](https://cryptokeyhole.com/charts/eth/?metric=eth.StablecoinSupply) · [Guide](https://cryptokeyhole.com/learn/stablecoin-supply/)

---

## Exchanges, ETFs and derivatives

These entries have a live chart but no guide yet; each definition follows the method notes on its chart.

### Exchange Balance and Netflow

The bitcoin held in the wallets of exchanges that publish their reserve addresses for proof of reserves, and the daily net movement of coins into or out of them. The address lists are extended with addresses CryptoKeyhole's own clustering ties to those wallets when they are spent together in consolidation transactions.

```
exchange balance = sum of unspent balances of labelled exchange addresses at the end of each period
exchange netflow = exchange inflow - exchange outflow        (fees excluded)
```

Transfers between two covered exchanges are left out of the all-exchange totals and reported separately as inter-exchange volume.

**How to read it**
- Coins on exchanges are available to sell. A falling balance means coins are moving into self-custody (net withdrawals); a rising balance means net deposits.
- Positive netflow means net deposits; negative means net withdrawals.
- Watch the sustained trend rather than single days.

**Limitations**
- Exchanges that publish no addresses are not covered, so totals understate exchanges as a whole.
- Customer deposit addresses are not identified; a deposit counts when the exchange moves it into its reserve wallets.
- Labels apply across each address's whole history, so history can change when address lists are refreshed, and the published lists are snapshots of different ages.

[Exchange balance chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.ExchangeBalanceBTC) · [Netflow chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.ExchangeNetflowVolumeBTC) · Paid charts: free visitors see a 2019–2022 sample.

### US Spot Bitcoin ETF Net Flows

The total daily net flow of the US spot bitcoin ETFs whose issuers publish daily holdings files, measured in bitcoin rather than dollars. It shows net ETF demand: how much bitcoin the funds together took in or paid out each US trading day.

```
net flow (day) = sum over included funds of (BTC held on the day - BTC held on the previous US trading day)
```

A day is shown only when every included fund has published it, and a fund joining the coverage adds holdings but no flow. Each day's figures are checked for internal consistency within the issuer's own file, and quarter-end figures are reconciled with the funds' SEC filings.

**How to read it**
- Positive bars: the funds together took in more bitcoin than they paid out; negative bars: the reverse.
- Sum the bars over a period for that period's net flow.
- Each bar is dated the day the holdings are as of, which is kept separate from when the issuer published it.

**Limitations**
- Funds whose issuers' websites refuse automated reads are not included, so the total understates the whole US spot ETF market.
- The latest day can wait for the slowest issuer; weekends and US market holidays have no figure.
- A day missing from an issuer's own history is left out, never filled in, and issuers can restate figures.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.UsSpotEtfNetFlowsDaily)

### Perpetual Funding Rate

The average funding rate on BTC perpetual swaps, weighted by each contract's open interest and expressed as an 8-hour rate. Funding is the periodic payment that keeps a perpetual's price near spot, so its sign and size show which side of the leveraged market is crowded.

```
funding rate = sum(8-hour-equivalent rate_i * USD open interest_i) / sum(USD open interest_i)
```

Each minute's current rate on every collected perpetual is converted to an 8-hour equivalent, then averaged across contracts by open interest and over the period.

**How to read it**
- Positive: longs pay shorts, a sign of leveraged long demand.
- Negative: shorts pay longs.
- Values far from zero mark crowded positioning; 0.01% per 8 hours is a common baseline.

**Limitations**
- Covers only the venues CryptoKeyhole collects, so it is not the whole market.
- History begins when collection started (October 2026); earlier values are not reconstructed.
- It reflects positioning on perpetual swaps only; read it with open interest and price.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MktPerpetualFundingRate) · Plus chart; no free sample.

### Futures Open Interest

The total US-dollar value of open BTC perpetual and dated futures positions across the venues CryptoKeyhole collects. It shows how much leverage is in the market and whether it is building or unwinding.

```
open interest = sum over contracts of the latest open-interest sample in the period
                (linear contracts: coins * mark price; inverse contracts: USD face value)
```

One side of each position is counted (venues that publish both sides are halved), and a venue missing from a period keeps its last value for up to an hour.

**How to read it**
- Rising open interest: more leverage entering the market.
- Falling open interest: positions closing.
- Read the level and its changes alongside price: price rising with open interest rising suggests new leveraged positions are opening, long or short.

**Limitations**
- Venues that are not collected are left out, so totals understate the whole market.
- History begins when collection started (October 2026); earlier values are not reconstructed.
- Open interest says how much leverage there is, not which side holds it.

[Live chart](https://cryptokeyhole.com/charts/btc/?metric=ckh.MktFuturesOpenInterestUSD) · Plus chart; no free sample.

---

## Using the data

- **Free chart embeds.** Put any of these charts on a blog, newsletter, forum post or Notion page as an interactive iframe, an image with a link, or Markdown and BBCode. Embeds refresh hourly and carry a small credit link back to the full chart: [cryptokeyhole.com/embed](https://cryptokeyhole.com/embed/).
- **Free API, Google Sheets, Excel and Python.** The same functions (values, latest value, exact series and metric lists) work in all three. Without an API key they read the free view: daily, weekly, monthly and yearly values of the free charts, with full history. A key from a paid plan adds what that plan includes: [cryptokeyhole.com/data-access](https://cryptokeyhole.com/data-access/).

  ```
  curl "https://cryptokeyhole.com/api/v1/series?metric=ckh.MayerMultiple&interval=24h"
  ```

  A chart's id is the `metric` value in its address.
- **MCP server for AI assistants.** A read-only Model Context Protocol server lets assistants and agents list assets and metrics, read metric definitions and fetch exact observations with their provenance. It needs no account or key and serves what the free plan includes: [cryptokeyhole.com/mcp](https://cryptokeyhole.com/mcp/) and [github.com/cryptokeyhole/cryptokeyhole-mcp](https://github.com/cryptokeyhole/cryptokeyhole-mcp).

Reuse of the chart data itself is covered by the [terms of service](https://cryptokeyhole.com/terms/) and the reuse notes on the [data access page](https://cryptokeyhole.com/data-access/).

## Contributing

Corrections and clarifications are welcome. If a definition, formula or threshold here does not match the method shown on the linked chart or guide, or something is unclear, please open an issue or a pull request. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Licence

The text of this repository is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE). You may share and adapt it for any purpose, provided you credit CryptoKeyhole with a link to [https://cryptokeyhole.com](https://cryptokeyhole.com) and indicate if changes were made. The licence covers this text only; chart data is provided under CryptoKeyhole's own terms.
