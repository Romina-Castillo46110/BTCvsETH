# 🪙 BTC vs ETH — Proyecto de insights ejecutivos y Modelado Predictivo (2017–2025)

Comparativo integral entre **Bitcoin (BTC)** y **Ethereum (ETH)** con datos diarios, orientado a generar *insights ejecutivos y modelos predictivos*.  
El análisis combina visualizaciones financieras con modelos de Machine Learning para responder preguntas sobre **rentabilidad, riesgo, momentum y diversificación**.

---

## 🎯 Objetivo y Audiencia

**Objetivo:**  
Analizar y modelar el comportamiento conjunto de BTC y ETH maximizando retorno y controlando riesgo (volatilidad, drawdowns).  
El proyecto evoluciona desde análisis descriptivo hasta **predicción temporal de retornos diarios** con modelos basados en datos tabulares y secuenciales.

**Audiencia:**  
Tesorerías corporativas, gestores de cartera, asset managers, analistas cuantitativos y financieros.

---

## ❓ Preguntas clave

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

## 🧪 Feature Engineering mínimo

- **Retornos diarios (`ret_d`)**: variación porcentual del precio día a día.  
- **Volatilidad (`vol_30`, `vol_90`)**: riesgo medido como desviación estándar móvil de 30 y 90 días.  
- **Drawdown (`dd`)**: caída porcentual desde el máximo anterior, útil para medir pérdidas potenciales.  
- **ETH/BTC (fortaleza relativa)**: relación entre ambos precios para identificar liderazgo de activos.  
- **Momentum (30d / 90d)**: retornos acumulados que reflejan la aceleración o inercia del precio.  
- **Correlación (Spearman)**: mide la relación no lineal entre los movimientos de BTC y ETH.  
- **Curva de cartera combinada**: volatilidad anualizada de una cartera \( w \cdot BTC + (1-w) \cdot ETH \), usada para evaluar el efecto diversificador.

Estas métricas permiten construir tanto análisis descriptivos como modelos predictivos basados en dependencias temporales y estructurales.

---

## 📊 Visualizaciones ejecutivas

Las figuras se guardan automáticamente en /figures/:

| Figura                               | Descripción                           |
| :----------------------------------- | :------------------------------------ |
| **01_precio_normalizado.png**        | Precio normalizado base 100           |
| **02_volatilidad_90d.png**           | Volatilidad anualizada rolling        |
| **03_drawdowns.png**                 | Drawdowns acumulados                  |
| **04_correlacion_spearman.png**      | Correlación BTC–ETH                   |
| **05_eth_btc.png**                   | Fortaleza relativa ETH/BTC            |
| **06_momentum_barras.png**           | Momentum 30d y 90d                    |
| **07_diversificacion_curva_vol.png** | Volatilidad cartera BTC–ETH           |
| **08_comparacion_auc_modelos.png**   | Comparativa LightGBM / LSTM / Híbrido |

Cada gráfico incluye su interpretación: qué muestra, cómo leerlo, conclusiones y decisión por perfil (conservador vs táctico) en el documento PDF del informe.

## 🧠 Modelado Predictivo (Machine Learning)

El análisis se amplía a un problema de **clasificación binaria temporal**, prediciendo si el retorno diario de BTC será positivo (`1`) o negativo (`0`).

### **Modelos utilizados (progresión de complejidad)**

#### 1️⃣ LightGBM con lags y rolling features
- Modelo basado en árboles de decisión.  
- Incorpora memoria manual mediante rezagos y medias móviles.  
- **AUC promedio:** ~0.77 (validación cruzada 5 folds).  
- Sirve como baseline tabular.

#### 2️⃣ LSTM (Long Short-Term Memory)
- Red neuronal recurrente secuencial.  
- Aprende dependencias temporales de 10 días consecutivos.  
- **AUC promedio:** ~0.70.  
- Capta momentum y volatilidad, aunque con mayor costo computacional.

#### 3️⃣ Modelo híbrido (LSTM + LightGBM)
- Integra la **señal secuencial aprendida por la LSTM (`lstm_score`)** como feature adicional para LightGBM.  
- Combina explicabilidad estructural con memoria temporal.  
- **AUC promedio:** ~0.71.  
- Más estable ante cambios de régimen de mercado.

Esta combinación de modelos permite no solo comparar rendimientos, sino también anticipar direccionalidad de precios con una base estadística sólida.

---

## ⚗️ Comparación de desempeño

| Modelo | Tipo | AUC | Características |
|:--|:--|:--:|:--|
| **LightGBM** | Tabular (lags y rolling) | **0.769** | Base sólida con memoria manual |
| **LSTM** | Secuencial (10 pasos) | **0.701** | Capta dependencias temporales |
| **Híbrido (LSTM + LightGBM)** | Mixto | **0.711** | Integra memoria temporal y estructura explicativa |

**Conclusión:**  
Aunque LightGBM mantiene el mejor AUC, el modelo híbrido aporta **mayor coherencia y estabilidad** en fases volátiles, combinando señales de tendencia y riesgo. Ver sección 10 en PDF.

---

## ⚙️ Reproducibilidad

1. **Clonar el repositorio**
 ```bash
   git clone https://github.com/Romina-Castillo46110/BTCvsETH.git
   cd BTCvsETH
 ```

2. **Instalar dependencias**
```bash
  pip install -r requirements.txt
```
3. **Ejecutar el notebook**

finalProject.ipynb

4. **Visualizar resultados**
   - Las figuras se generan en `figures/`
   - Las métricas se imprimen en el notebook

## 📦 Entorno y librerías

- Python 3.10+
- pandas, numpy, matplotlib, scipy  
- scikit-learn, lightgbm, xgboost  
- tensorflow / keras (para LSTM)

## ⚠️ Descargo

Este proyecto tiene fines educativos y de análisis cuantitativo.
No constituye asesoramiento financiero.
Los criptoactivos son volátiles: aplicá gestión de riesgo (posición, VaR/ES, stops).

## 👩‍💻 Autora

**Romina Castillo** — Data Analyst Jr & Data Scientist Trainee | Machine Learning  
Proyecto desarrollado como trabajo final del curso de Ciencia de Datos.  
📫 [LinkedIn](www.linkedin.com/in/romina-castillo-239370281) | [GitHub](https://github.com/Romina-Castillo46110)










