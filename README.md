-----

# Risk Manager Pro EA para MetaTrader 5

Un Asesor Experto (EA) avanzado para MetaTrader 5, diseñado para la gestión de riesgo semiautomática y automática. Su núcleo es una estrategia de **"Gestión de Riesgo por Zonas"** que permite asegurar ganancias en puntos clave y mover el Stop Loss de manera inteligente para proteger el capital.

## 🚀 Características Principales

  * **Gestión de Riesgo por Zonas:** Implementa una estrategia de toma de ganancias parciales y movimiento de Stop Loss en niveles de Riesgo/Beneficio (RR) predefinidos.
  * **Toma de Ganancias Parciales:** Cierra automáticamente un porcentaje configurable de la operación al alcanzar zonas de beneficio clave (ej: RR 2:1, 4:1).
  * **Panel de Información Visual:** Muestra en tiempo real todos los datos relevantes de la operación, la configuración del EA y el progreso de la gestión por zonas.
  * **Lógica de "Trade Raíz":** Todos los cálculos se basan en el precio de entrada y el Stop Loss iniciales, garantizando que los niveles de gestión no cambien si el SL se mueve manualmente.
  * **Modo Dual (Manual y Automático):**
      * **Manual:** El EA solo envía alertas, dejando la decisión final al trader.
      * **Automático:** El EA ejecuta la toma de parciales y mueve el Stop Loss sin intervención del usuario.
  * **Filtro de Volatilidad ATR:** Añade un "colchón" de seguridad al Stop Loss basado en el ATR para evitar cierres prematuros por ruido de mercado.
  * **ATR Multi-Timeframe:** El ATR puede ser calculado en cualquier temporalidad para una gestión de riesgo más robusta.
  * **Indetectable para el Broker:** El EA no abre/cierra posiciones por su cuenta (solo las modifica), por lo que en modo manual es invisible para el broker.

-----

## ⚙️ La Estrategia: Gestión de Riesgo por Zonas

El EA abandona un simple trailing stop para adoptar una metodología de gestión por zonas, basada en Ratios de Riesgo/Beneficio (RR).

1.  **Captura del Trade Raíz:** Al detectar una nueva operación, el EA guarda el **Precio de Entrada (PE)** y el **Stop Loss inicial (SL₀)**.
2.  **Cálculo de la Distancia (D):** Se calcula la distancia de riesgo `D = |PE - SL₀|`. Este valor representa **1R**.
3.  **Ejecución por Zonas:** El EA monitorea el precio y ejecuta acciones predefinidas al alcanzar los siguientes niveles de RR:

| Zona            | Nivel de Beneficio | Acción Automática                                                                                                |
| --------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------- |
| **Zona 1** | **RR 1:1** | Mueve el **Stop Loss a Breakeven** (Precio de Entrada). Se puede aplicar el filtro ATR para mayor seguridad.         |
| **Zona 2** | **RR 1:2** | **Toma de Ganancias Parciales (TP1).** Cierra un porcentaje configurable del volumen de la operación.                  |
| **Zona 3** | **RR 1:3** | Mueve el **Stop Loss para asegurar ganancias,** por ejemplo, al nivel 1.5R.                                        |
| **Zona 4 en adelante** | **Configurable** | Se pueden configurar más zonas para tomar ganancias parciales adicionales o seguir moviendo el Stop Loss. |

4.  **Aplicación del Filtro ATR (Opcional):** Antes de mover el SL, se le puede añadir un buffer para protegerlo de la volatilidad:
      * **Para Longs:** `SL Final = Nivel Calculado - (ATR * Multiplicador)`
      * **Para Shorts:** `SL Final = Nivel Calculado + (ATR * Multiplicador)`

-----

## 🔧 Parámetros de Entrada

El EA es totalmente configurable para adaptar la estrategia de zonas a tus necesidades.

| Parámetro          | Tipo                | Descripción                                                                               |
| ------------------ | ------------------- | ----------------------------------------------------------------------------------------- |
| `AutomateStopLoss` | `true`/`false`      | **Activa/desactiva el modo automático.** |
| `UseATR_Filter`    | `true`/`false`      | **Activa/desactiva el filtro de volatilidad ATR.** |
| `ATR_Timeframe`    | `ENUM_TIMEFRAMES`   | **Temporalidad para el cálculo del ATR.** |
| `ATR_Period`       | `integer`           | Período del indicador ATR (ej: 14).                                                       |
| `ATR_Multiplier`   | `double`            | Multiplicador para el valor del ATR (ej: 1.5).                                            |
| `Partial1_RR`      | `double`            | Nivel de RR para la primera toma de parciales (ej: 2.0 para 2:1).                           |
| `Partial1_Percent` | `double`            | Porcentaje del lote a cerrar en el TP1 (ej: 50.0).                                        |
| `...`              | `...`               | Se pueden añadir más parámetros para zonas de parciales adicionales.                        |

-----

## 🛠️ Instalación y Uso

1.  **Instalación:**
      * Abre MetaEditor en MT5 (`F4`).
      * Crea un nuevo "Asesor Experto" y nombra el archivo `Risk Trade Manager EA.mq5`.
      * Copia y pega el código de la última versión.
      * Haz clic en **"Compilar"** (`F7`).
2.  **Uso:**
      * Arrastra el EA desde la ventana "Navegador" al gráfico deseado.
      * Ajusta los parámetros de gestión por zonas a tu gusto.
      * En la pestaña "Común", asegúrate de que **"Permitir trading algorítmico"** esté marcado.
      * Abre una operación con un Stop Loss inicial. El EA la detectará y comenzará la gestión.
      * **Importante:** Para el modo automático, el botón **"Trading algorítmico"** de la barra principal de MT5 debe estar activado.

-----

## 📜 Historial de Versiones (Changelog)

  * **v7.0 (Próxima):** Refactorización a "Gestión de Riesgo por Zonas" con toma de ganancias parciales.
  * **v6.2 (Estable):** Corrección de la función `TimeframeToString` para una compilación limpia.
  * **v6.1:** Añadida la selección de Timeframe para el cálculo del ATR.
  * **v6.0:** Implementado el filtro de volatilidad ATR para el ajuste del SL.
  * **v5.2:** Corregido un bug de reinicialización al cambiar los parámetros del EA en un gráfico activo.
  * **v5.0:** Introducido el modo **automático** para la modificación del SL.
  * **v4.0:** Implementada la lógica de **"Trade Raíz"** para que los niveles de gestión sean fijos.
  * **v3.0:** Añadido el panel de información gráfico (GUI).
  * **v2.0:** Conversión del script inicial a un Asesor Experto con monitoreo en tiempo real y alertas.
  * **v1.0:** Versión inicial como script para dibujar niveles.

-----

### ⚠️ **Advertencia**

El trading automatizado conlleva riesgos. Prueba este Asesor Experto exhaustivamente en una **cuenta demo** antes de utilizarlo en una cuenta con dinero real. El autor no se hace responsable de ninguna pérdida financiera.
