# ElectroCalc — Calculadoras de Electrónica en Español

Sitio web estático de calculadoras de electrónica, completamente en español. Sin frameworks, sin dependencias, sin proceso de build — solo HTML, CSS y JavaScript vanilla.

## Calculadoras incluidas

### Divisor de Voltaje
Calcula `Vout = Vin × R2 / (R1 + R2)`. Soporta selectores de unidad para voltaje (V/mV) y resistencias (Ω/kΩ/MΩ). Muestra también la corriente en el divisor y la potencia disipada.

### Ley de Ohm
Calcula cualquiera de las cuatro magnitudes (V, I, R, P) a partir de dos valores conocidos. Interfaz dinámica con selector de lo que se quiere calcular.

### Resistencia para LED
Calcula la resistencia limitadora de corriente: `R = (Vcc - Vf) / If`. Incluye:
- Presets de color (Rojo 2.0V, Verde 2.2V, Azul 3.2V, Blanco 3.4V)
- Búsqueda del valor comercial más cercano en la serie E24
- Advertencias de seguridad para corriente >30mA y potencia >250mW

## Estructura del proyecto

```
Calculadoras/
├── index.html        # Toda la aplicación (HTML + CSS inline refs + JS)
├── css/
│   └── styles.css    # Sistema de diseño — dark mode, tokens CSS, layout
└── img/
    ├── divisor.svg   # Diagrama de circuito divisor de voltaje
    ├── ohm.svg       # Triángulo de la Ley de Ohm
    └── led.svg       # Circuito LED con resistencia
```

## Desarrollo

Abre `index.html` directamente en el navegador. No hay build, no hay servidor, no hay dependencias que instalar.

No existen comandos de lint, test ni build.

## Arquitectura

Todo el JavaScript vive en un `<script>` inline al final de `<body>` con `'use strict'`. No hay módulos ni bundler.

**Helpers principales:**

| Función | Descripción |
|---|---|
| `fmt(n, decimals)` | Formatea números con sufijos M/k |
| `fmtOhm(r)` | Formatea resistencias como Ω / kΩ / MΩ |
| `nearestE24(ohms)` | Encuentra el valor estándar E24 más cercano |
| `getPositive(id, unitId)` | Lee y valida un número positivo desde un input, multiplicado por su unidad |
| `getOhmVal(id)` | Como `getPositive` pero acepta cero (usado en la calculadora Ohm) |

**Patrón de cada calculadora:**
1. Leer inputs con `getPositive()` o `getOhmVal()`
2. Mostrar errores inline con `showError()` / `clearError()`
3. Rellenar nodos de resultado y revelarlos con `showResult()` / `hideResult()`

## Tokens de diseño CSS

Definidos en `:root` — modificar estos valores retematiza todo el sitio:

```css
--bg-dark   #1a1a2e   /* Fondo de página */
--bg-card   #16213e   /* Fondo de tarjeta calculadora */
--bg-input  #0f3460   /* Fondo de campos de entrada */
--accent    #00d4ff   /* Color de acento principal */
--error     #ff4d4d   /* Mensajes de error */
--warn      #ffd700   /* Mensajes de advertencia */
--success   #00ff99   /* Valor E24 destacado */
```

El layout es un grid de dos columnas (`main` + `.sidebar` de 260px) que colapsa a una columna por debajo de 768px.

## Agregar una nueva calculadora

1. Añadir `<section class="calc-section" id="nueva-id">` dentro de `<main>`, siguiendo la estructura de las secciones existentes.
2. Agregar el enlace de navegación: `<li><a href="#nueva-id">Nombre</a></li>`.
3. Añadir el ID al array `sections` en el bloque de scroll-highlight del JS.
4. Colocar un SVG del circuito en `img/` y referenciarlo al final de la sección.

## Despliegue

Es un sitio completamente estático. Se puede servir con cualquier servidor web (Nginx, Apache, Caddy) apuntando al directorio raíz. Sin variables de entorno, sin base de datos, sin configuración especial.

---

Hecho en Durango, México 🇲🇽
