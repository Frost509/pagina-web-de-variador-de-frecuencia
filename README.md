#  SCADA — Analizador de Red Eléctrica

Panel web de monitoreo eléctrico en tiempo real. Muestra potencia activa, aparente y reactiva, tensión, corriente, frecuencia, factor de potencia y consumo en kWh con su equivalente en pesos argentinos.

![Estado](https://img.shields.io/badge/estado-demo%2Fsimulado-yellow)
![Tecnología](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-blue)
![Licencia](https://img.shields.io/badge/licencia-MIT-green)

---

##  Vista previa

> Panel oscuro estilo industrial con actualización automática, semáforo de estado y cálculo de costos en ARS.

---

##  Funcionalidades

- **Potencias** — activa (kW), aparente (kVA), reactiva (kVAr) y energía acumulada (kWh)
- **Mediciones básicas** — tensión promedio (V), corriente total (A), frecuencia (Hz) y factor de potencia
- **Semáforo de estado** — LED verde / amarillo / rojo con detección de alerta y falla
- **Consumo histórico** — historial mensual con costo en pesos argentinos
- **Totales del período** — consumo del día y del mes en kWh y en ARS (tarifa + cargo fijo)
- **Proyección mensual** — estimación de consumo al fin de mes según el promedio diario
- **Simulación** — botón para probar distintos escenarios de red (normal / alerta / falla)

---

##  Uso rápido

No requiere instalación ni servidor. Es un único archivo HTML autocontenido.

```bash
# Clonar el repositorio
git clone https://github.com/tu-usuario/scada-analizador-red.git

# Abrir en el navegador
cd scada-analizador-red
open scada-analizador-red.html   # macOS
start scada-analizador-red.html  # Windows
xdg-open scada-analizador-red.html  # Linux
```

O simplemente descargá el archivo y hacé doble clic.

---

##  Configuración de tarifa

Dentro del archivo `scada-analizador-red.html`, buscá estas dos constantes al inicio del bloque `<script>` y modificalas según tu categoría tarifaria:

```js
const TARIFA = 215;      // $/kWh — ajustar según tu factura
const CARGO_FIJO = 4200; // $/mes — cargo fijo de distribuidora
```

---

##  Integración con medidor real

Actualmente el panel usa datos simulados. Para conectarlo a un medidor físico real existen tres caminos:

### Opción A — Modbus TCP/IP
La mayoría de los analizadores de red industriales soportan Modbus TCP. Se puede agregar un backend liviano (Node.js o Python) que lea los registros y los exponga como una API REST que el panel consulta cada pocos segundos.

```
Medidor Modbus TCP → Backend Node.js/Python → API REST → Panel HTML
```

Protocolos compatibles: Modbus TCP, Modbus RTU sobre RS485.

### Opción B — ESP32 / Arduino como pasarela
Si el medidor tiene salida por pulsos o RS485, un ESP32 puede leer los datos y publicarlos por WiFi en formato JSON, eliminando la necesidad de una PC como servidor.

```
Medidor RS485 → ESP32 WiFi → JSON local → Panel HTML
```

### Opción C — Medidor con API HTTP propia
Algunos equipos modernos tienen servidor web integrado. En ese caso basta con hacer un `fetch()` a la IP del medidor desde el panel.

---

##  Estructura del proyecto

```
scada-analizador-red/
│
├── scada-analizador-red.html   # Aplicación completa (un solo archivo)
└── README.md
```

---

##  Tecnologías

- HTML5 / CSS3 / JavaScript vanilla
- Fuentes: [Barlow](https://fonts.google.com/specimen/Barlow) + [Share Tech Mono](https://fonts.google.com/specimen/Share+Tech+Mono) (Google Fonts)
- Sin frameworks, sin dependencias, sin build step

---

##  Parámetros monitoreados

| Parámetro | Unidad | Descripción |
|---|---|---|
| Potencia activa | kW | Potencia real consumida |
| Potencia aparente | kVA | Potencia total demandada |
| Potencia reactiva | kVAr | Componente no productiva |
| Energía acumulada | kWh | Contador total desde inicio |
| Tensión promedio | V | Promedio de las tres fases |
| Corriente total | A | Suma de corrientes de fase |
| Frecuencia | Hz | Frecuencia de red |
| Factor de potencia | — | Relación entre activa y aparente (0 a 1) |

---

##  Lógica del semáforo

| Estado | Color | Condición típica |
|---|---|---|
| Normal | 🟢 Verde | Tensión 210–240 V, FP ≥ 0.90, I normal |
| Alerta | 🟡 Amarillo | Tensión baja, FP < 0.85, sobrecarga leve |
| Falla | 🔴 Rojo | Tensión crítica, FP < 0.70, sobrecarga severa |

Los umbrales son configurables en el código fuente.

---

##  Roadmap

- [ ] Conexión a medidor real vía Modbus TCP
- [ ] Exportar historial a CSV
- [ ] Alertas por correo o Telegram al superar umbrales
- [ ] Gráfico de consumo por hora
- [ ] Soporte multifase (L1, L2, L3)

---

##  Licencia

MIT — libre para uso personal y comercial.
