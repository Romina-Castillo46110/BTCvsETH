# BTC vs ETH — Proyecto de insights ejecutivos (2017–2025)

Comparativo BTC vs ETH con datos diarios para responder preguntas ejecutivas: ¿qué conviene según el perfil (conservador vs crecimiento)?, ¿qué tan correlacionadas están?, ¿cuánto ayuda diversificar?, ¿qué activo hoy tiene mejor momentum?, etc. El análisis usa la API pública de Binance y métricas robustas como **Spearman** para correlación.

El PDF del repo resume el propósito, preguntas y lecturas ejecutivas de cada gráfico (abstracto, hipótesis, “cómo leer” y decisiones por perfil).
URL: https://docs.google.com/document/d/1f_cA2VTROM4JstWPx7nrsYGdTWW3CX_kYdZRSmqQvlU/edit?usp=sharing

---

## 🎯 Objetivo & Audiencia

- **Objetivo**: comparar BTC y ETH maximizando retorno y controlando riesgo (volatilidad, drawdowns), con decisiones por **perfil conservador** (Sharpe/vol y DD) vs **perfil crecimiento** (momentum/fortaleza relativa).
- **Audiencia**: tesorería corporativa, asset managers, PMs, analistas de riesgo.

---

## ❓ Preguntas que responde

1. ¿Es mejor Bitcoin o Ethereum?
2. ¿Cuál es la mejor para invertir **hoy** (Sharpe simple y momentum)?
3. ¿Cuál fue la **más rentable** en el período analizado?
4. ¿Cuál tiene **más futuro** (ETH/BTC + momentum)?
5. ¿Cuál conviene para **2025 (YTD)**?
6. ¿Cuánto al día puedo **ganar** con cripto (P&L diario con capital dado)?
7. ¿Qué tan **correlacionadas** están BTC y ETH (**Spearman**) y **cuánto ayuda diversificar**?

---

## 🧰 Datos & Fuentes

- **Binance Spot API** (gratis; sin API key) para OHLCV diarios.
- CSV persistidos en `data/raw/`:
  - `binance_BTCUSDT_1d.csv`
  - `binance_ETHUSDT_1d.csv`

---

## 🛠️ Reproducibilidad

### Opción A: bajar datos (primera vez)
En el notebook activar `RUN_API=True` y ejecutar la celda de descarga (crea `data/raw/…csv`).

### Opción B: trabajo offline
Dejar `RUN_API=False` (o comentar la celda) y cargar desde `data/raw/…csv`.

---

## 📦 Entorno

- Python 3.10+  
- Paquetes: `pandas numpy matplotlib requests scipy` (Spearman se calcula también con `pandas.DataFrame.corr(method="spearman")`).

---

## 🧪 Feature engineering mínimo

- **Retornos**: `ret_d = price.pct_change()`
- **Volatilidad**: `vol_30`, `vol_90` = `rolling(std) * sqrt(365)`
- **Drawdown**: `dd = price / price.cummax() - 1`
- **ETH/BTC**: razón `ETH / BTC`
- **Momentum**: retornos acumulados 30/90 días
- **Spearman**: `pv_ret.corr(method="spearman")` y **rolling** (60/90d)
- **Curva de cartera**: vol anualizada de `w*BTC + (1-w)*ETH`

---

## 📊 Gráficos principales (se guardan en `figures/`)

1. `01_precio_normalizado.png` — Precio normalizado (base=100)
2. `02_volatilidad_90d.png` — Volatilidad anualizada (rolling 90d)
3. `03_drawdowns.png` — Drawdowns (desde máximos previos)
4. `04_correlacion_spearman.png` — Dispersión de retornos y ρₛ
5. `05_eth_btc.png` — ETH/BTC (fortaleza relativa)
6. `06_momentum_barras.png` — Momentum 30/90 días
7. `07_diversificacion_curva_vol.png` — Vol de cartera vs peso BTC
8. `07_spearman_rolling.png` — ρₛ rolling 60/90 días

---

## ▶️ Cómo ejecutar

1. Abrí `finalProject.ipynb` en Jupyter/VS Code.
2. (Opcional) Ejecutá la celda de descarga de datos (solo la primera vez).
3. Ejecutá el resto de celdas (transformaciones, métricas, figuras).
4. Las imágenes/CSV de resultados quedan en `figures/` y `export/` (si el helper `savefig` y export está activo).

---

## 🧾 Resultados automáticos

El notebook imprime respuestas cortas para cada pregunta (p. ej., “más rentable en el período”, “mejor para invertir hoy”, “ρₛ y diversificación”, etc.) y guarda las imágenes.

---

## ⚠️ Descargo

Este proyecto es **educativo**. No constituye asesoramiento financiero. Los criptoactivos son volátiles; usá gestión de riesgo (tamaño de posición, VaR/ES, stops).

---

## 📚 Documento del informe

El PDF “BTC vs ETH — Proyecto de insights ejecutivos (2017–2025)” acompaña el notebook con la narrativa ejecutiva y las lecturas de cada gráfico.
