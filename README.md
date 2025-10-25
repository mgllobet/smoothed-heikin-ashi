# Smoothed Heiken Ashi Indicator

A TradingView Pine Script indicator that applies double exponential smoothing to Heiken Ashi candles, providing a cleaner view of market trends with reduced noise.

## Overview

This indicator enhances traditional Heiken Ashi candles by applying two layers of EMA (Exponential Moving Average) smoothing:

1. **First smoothing**: Applied to regular OHLC (Open, High, Low, Close) data
2. **Heiken Ashi calculation**: Performed on the smoothed OHLC values
3. **Second smoothing**: Applied to the resulting Heiken Ashi candles

The result is a highly smoothed visualization that makes trend identification easier while filtering out market noise.

## Features

- **Double smoothing**: Combines EMA smoothing with Heiken Ashi for maximum noise reduction
- **Confirmation filter**: Requires N consecutive candles before confirming trend changes (reduces false signals)
- **Customizable periods**: Adjust both smoothing lengths to match your trading style
- **Visual confirmation markers**: Small triangles appear when trend changes are confirmed
- **Color customization**: Choose your preferred bullish and bearish colors
- **Trend background**: Subtle background color indicates current trend direction
- **Clean code**: Written in Pine Script v5 with clear documentation

## How It Works

### Traditional Heiken Ashi

Heiken Ashi candles are calculated as:
- **HA Close** = (Open + High + Low + Close) / 4
- **HA Open** = (Previous HA Open + Previous HA Close) / 2
- **HA High** = Max(High, HA Open, HA Close)
- **HA Low** = Min(Low, HA Open, HA Close)

### Smoothed Heiken Ashi Enhancement

This indicator adds two EMA smoothing layers:

```
Regular OHLC → EMA Smoothing → Heiken Ashi Calculation → EMA Smoothing → Confirmation Filter → Final Candles
```

**Benefits:**
- Removes short-term volatility
- Makes trend reversals more visible
- Reduces false signals with confirmation filter
- Creates cleaner sequences of same-colored candles
- Filters out noise from minor corrections

## Installation

1. Copy the code from `src/pinescript/smoothed_heikin_ashi.pine`
2. Open TradingView and create a new indicator
3. Paste the code and save
4. Add the indicator to your chart

## Configuration

### Input Parameters

| Parameter | Default | Range | Description |
|-----------|---------|-------|-------------|
| First Smoothing Length | 10 | 1-100 | EMA period applied to OHLC before Heiken Ashi calculation |
| Second Smoothing Length | 10 | 1-100 | EMA period applied to Heiken Ashi candles |
| **Confirmation Candles** | **1** | **1-10** | **Number of consecutive opposite-color candles required before trend change is confirmed** |
| Bullish Color | Lime | - | Color for uptrend candles |
| Bearish Color | Red | - | Color for downtrend candles |

### Confirmation Candles Feature

The **Confirmation Candles** parameter is a key feature that prevents premature trend changes during minor corrections:

**How it works:**
- Set to `1`: Immediate color change (default, most responsive)
- Set to `2`: Requires 2 consecutive opposite candles before confirming trend change
- Set to `3`: Requires 3 consecutive opposite candles (recommended for most users)
- Higher values: More filtering, but increased lag

**Example with Confirmation Candles = 3:**
```
Current trend: GREEN (bullish)
Bar 1: RED candle appears → stays GREEN (1/3)
Bar 2: RED candle appears → stays GREEN (2/3)
Bar 3: RED candle appears → changes to RED (3/3 - CONFIRMED!)
```

**Benefits:**
- ✅ Filters out noise from small corrections
- ✅ Reduces false signals in choppy markets
- ✅ Keeps you in trends longer
- ✅ Avoids premature exits during pullbacks

**Trade-off:**
- ⚠️ Increases lag when real trend changes occur
- ⚠️ May miss quick reversals

### Recommended Settings

**For Swing Trading (Daily/4H charts):**
- First Smoothing: 10-14
- Second Smoothing: 10-14
- Confirmation Candles: 2-3

**For Day Trading (1H/30m charts):**
- First Smoothing: 5-8
- Second Smoothing: 5-8
- Confirmation Candles: 2-3

**For Scalping (5m/1m charts):**
- First Smoothing: 3-5
- Second Smoothing: 3-5
- Confirmation Candles: 1-2 (stay responsive)

**For Long-term Trend Analysis:**
- First Smoothing: 14-21
- Second Smoothing: 14-21
- Confirmation Candles: 3-5 (higher filtering)

**For Choppy/Ranging Markets:**
- Increase Confirmation Candles to 4-5 to avoid whipsaws

**For Strong Trending Markets:**
- Keep Confirmation Candles at 1-2 for faster entries

## Trading Signals

### Entry Signals

**Bullish (Long Entry):**
- Candles change from red to green
- Sequence of green candles indicates strong uptrend
- Enter when green candles appear after consolidation

**Bearish (Short Entry):**
- Candles change from green to red
- Sequence of red candles indicates strong downtrend
- Enter when red candles appear after consolidation

### Exit Signals

- **Long Exit**: When candles turn red
- **Short Exit**: When candles turn green
- **Consolidation**: Small candles alternating colors = avoid trading

### Confirmation

**Best practices:**
- Combine with support/resistance levels
- Use volume confirmation
- Apply on higher timeframe for trend filter
- Wait for 2-3 candles of same color before entering

## Advantages

✅ **Trend clarity**: Very clear visual representation of trend direction
✅ **Noise reduction**: Double smoothing eliminates false signals
✅ **Easy to read**: Color-coded candles are intuitive
✅ **Works on all timeframes**: From 1-minute to monthly charts
✅ **Reduces whipsaws**: Fewer false reversals than regular candles

## Disadvantages

❌ **Lagging indicator**: Significant delay due to double smoothing
❌ **Late entries**: Trend changes are detected after they begin
❌ **Price imprecision**: Smoothed values don't reflect exact prices
❌ **Not ideal for ranging markets**: Best for trending conditions
❌ **Misses quick reversals**: Fast V-shaped reversals may be smoothed out

## Use Cases

### As Primary Indicator
- **Trend following strategies**: Enter and hold during long sequences
- **Swing trading**: Capture major moves with reduced noise
- **Position trading**: Long-term trend identification

### As Trend Filter
- **Combine with other systems**: Use color as trend filter
- **Higher timeframe confirmation**: Daily smoothed HA for intraday trades
- **Risk management**: Only trade in direction of smoothed HA color

### Example: Integration with Entry System
```
1. Check Smoothed HA on higher timeframe (e.g., Daily)
2. Only take trades aligned with that trend
3. Use your normal entry system on lower timeframe
4. Exit when Smoothed HA changes color
```

## Technical Details

- **Pine Script Version**: v5
- **Overlay**: Yes (plots on main chart)
- **Calculation**: Combines EMA and Heiken Ashi formulas
- **Performance**: Lightweight, minimal CPU usage

## Comparison with Regular Heiken Ashi

| Feature | Regular HA | Smoothed HA |
|---------|------------|-------------|
| Noise filtering | Moderate | High |
| Lag | Low | High |
| Trend clarity | Good | Excellent |
| Entry precision | Moderate | Low |
| Best for | Active trading | Trend following |

## Contributing

Contributions are welcome! Please feel free to submit pull requests or open issues for bugs and feature requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Disclaimer

This indicator is for educational purposes only. Trading involves substantial risk and is not suitable for every investor. Past performance does not guarantee future results. Always do your own research and consider consulting with a financial advisor before making investment decisions.

## Credits

Based on the original Heiken Ashi formula with double EMA smoothing enhancement.

---

**Star this repository if you find it useful!** ⭐
