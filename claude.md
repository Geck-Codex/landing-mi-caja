# CLAUDE.md — MI CAJA · Landing Page Project

> Este archivo es la fuente de verdad para Claude en este proyecto.
> Léelo completo antes de escribir una sola línea de código.

---

## 📦 Qué es MI CAJA

**MI CAJA** es un software de punto de venta (POS) desarrollado en el estado de **Chihuahua, México**.
Está diseñado para negocios locales: abarrotes, fondas, restaurantes, minisupers, farmacias, estéticas y cualquier local comercial.

**Precio único:** $3,000 MXN — pago único, sin mensualidades, soporte 24/7 incluido.

---

## 🎯 Objetivo de este proyecto

Construir la **landing page oficial** de MI CAJA.
Una página de una sola entrega que convierte visitantes en leads por WhatsApp.

**KPI principal:** el usuario llega → se enamora del producto → manda WhatsApp.

---

## 🛠 Stack técnico

| Capa | Tecnología |
|---|---|
| Framework | **Astro 4** (islands architecture) |
| Estilos | **Tailwind CSS v3** (JIT mode) |
| Componentes interactivos | **Astro + vanilla JS** (nada de React a menos que sea necesario) |
| Fuentes | **Syne** (display/headings) + **DM Sans** (body) — Google Fonts |
| Animaciones | **CSS transitions + @keyframes** primero · GSAP solo si se necesita scroll complejo |
| Iconos | **Lucide** (via CDN o npm) |
| Deploy | Vercel / Netlify (estático) |

> **Regla de oro:** cero dependencias innecesarias. Si se puede hacer en CSS puro, se hace en CSS.

---

## 🎨 Branding & Design Tokens

### Colores
```js
// tailwind.config.mjs
colors: {
  verde:  '#1AE17A',   // acento principal — verde eléctrico
  azul:   '#1A6CFF',   // acento secundario
  dark:   '#07090F',   // fondo base
  card:   '#0D1017',   // fondo de cards
  card2:  '#111520',   // fondo alternativo
  border: 'rgba(255,255,255,0.07)',
  text:   '#F0F2F8',
  muted:  'rgba(240,242,248,0.45)',
}
```

### Tipografía
- **Syne** → títulos, navbar, botones CTA, números grandes
- **DM Sans** → párrafos, subtítulos, labels, UI en general
- Escala: `text-7xl` a `text-9xl` para hero H1 · `tracking-tight` siempre en headings

### Logo
- Archivo: `mi_caja.png` (adjunto en el repo)
- Usar como `<img>` con `h-9` en navbar y como favicon
- Logotipo de texto: "MI CAJA" en Syne 800, color `text-text`
- No distorsionar, no recolorear el logo

### Paleta de gradientes
```css
/* Gradiente de acento — usar en texto destacado, botones primarios */
background: linear-gradient(135deg, #1AE17A, #1A6CFF);

/* Glow verde — box-shadow en hover de CTAs */
box-shadow: 0 0 40px rgba(26,225,122,0.4);

/* Glow azul */
box-shadow: 0 0 40px rgba(26,108,255,0.35);
```

---

## 🌐 Inspiración de diseño

Este proyecto es una **fusión intencional** de tres referencias:

### Square (squareup.com) — estructura principal
- Hero gigante con headline brutal, subheadline claro, CTA inmediato
- **Video de fondo en el hero** — loop de un negocio usando el POS en acción
- Stats row debajo del hero (números que impactan: precio, soporte, módulos)
- Sección "Para cada tipo de negocio" con chips horizontales scrolleables
- Dark mode profundo con orbs de luz animados como fondo

### Toast (pos.toasttab.com) — sección de casos de uso / negocios
- **Cards de video** por tipo de negocio: abarrote, fonda, restaurante, etc.
- Cada card tiene un video corto (`.mp4` en loop, muted, autoplay) con overlay oscuro
- Hover en la card → el video se activa / el overlay se aclara
- Copy estilo: "Para tu abarrote" · "Para tu fonda" · "Para tu restaurante"
- Testimoniales reales de dueños de negocios chihuahuenses

### Clover (clover.com) — bento grid de features
- Bento grid asimétrico para mostrar las features
- Cards con iconos grandes + descripción corta
- Card especial del precio con tratamiento visual distinto (borde verde)
- Card interactiva con mockup del POS en funcionamiento
- Hover shimmer effect en cada card (glow que sigue al cursor)

---

## 📐 Estructura de la página (secciones en orden)

```
1.  NAVBAR          — logo + links + CTA pill
2.  HERO            — headline + video background + stats
3.  NEGOCIOS        — scroll horizontal de chips de tipos de negocio
4.  MARQUEE         — ticker animado con features clave
5.  FEATURES BENTO  — grid asimétrico con todos los módulos
6.  VIDEO CASES     — cards de video por tipo de negocio (Toast-style)
7.  PRECIO          — card de precio claro + comparativa opcional
8.  SOPORTE         — sección con enfoque regional Chihuahua
9.  TESTIMONIALES   — quotes de negocios reales (placeholders por ahora)
10. CTA FINAL       — form WhatsApp + trust badges
11. FOOTER          — mínimo y limpio
```

---

## 🎬 Animaciones & Transiciones — REQUERIMIENTOS CRÍTICOS

> **Filosofía:** suave como Square, nunca distrae. Cada animación tiene propósito.

### Page load
```css
/* Staggered reveal — todos los elementos del hero entran con delay escalonado */
.hero-badge   { animation: fadeUp .6s ease both; }
.hero-h1      { animation: fadeUp .7s ease .1s both; }
.hero-sub     { animation: fadeUp .7s ease .2s both; }
.hero-actions { animation: fadeUp .7s ease .3s both; }
.hero-stats   { animation: fadeUp .7s ease .4s both; }

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(24px); }
  to   { opacity: 1; transform: translateY(0); }
}
```

### Scroll reveal
```js
// IntersectionObserver — threshold 0.12, delay escalonado entre children
// Usar clase .reveal → agregar .visible cuando entra al viewport
// Transition: opacity .7s ease, transform .7s ease
```

### Hover en cards (bento)
```css
/* Glow que sigue al cursor — usar CSS custom properties */
.card { --mx: 50%; --my: 50%; }
.card::before {
  background: radial-gradient(600px circle at var(--mx) var(--my),
    rgba(26,225,122,0.05), transparent 50%);
}
/* Actualizar --mx y --my con JS en mousemove */
```

### Video cards (sección Toast-style)
```css
/* Por defecto: video pausado, overlay oscuro al 70% */
/* Hover: overlay baja a 20%, video se reproduce, escala leve */
.video-card video { transition: transform .5s cubic-bezier(.25,.46,.45,.94); }
.video-card:hover video { transform: scale(1.04); }
.video-card .overlay { transition: opacity .4s ease; }
```

### Navbar
```css
/* Scroll → agregar backdrop-blur y borde inferior */
/* transition: background .3s ease, border-color .3s ease */
```

### CTA buttons
```css
/* Hover: translateY(-2px) + glow shadow */
/* Active: translateY(0) + glow reducido */
/* Transition: all .2s cubic-bezier(.25,.46,.45,.94) */
```

### Marquee
```css
@keyframes marquee {
  from { transform: translateX(0); }
  to   { transform: translateX(-50%); }
}
/* Fade mask en los lados con pseudo-elementos */
/* Pausar en hover: animation-play-state: paused */
```

### Números / counters
```js
// Animar con requestAnimationFrame cuando el elemento entra al viewport
// Duración: 1800ms · easing: easeOutQuart
// Formato: toLocaleString('es-MX')
```

---

## 📋 Features a mostrar (módulos del POS)

Cada uno va en una card del bento:

| Icono | Módulo | Copy corto |
|---|---|---|
| 🛒 | Ventas rápidas | Cobra en segundos con acceso directo y búsqueda instantánea |
| 📦 | Control de inventario | Alta de productos, categorías y alertas de stock bajo |
| 📊 | Reportes | Ventas por día, turno y mes. Corte de caja automático |
| 📒 | Fiados / Crédito | Registra clientes con saldo, historial de abonos y deuda |
| 💳 | Descuentos | Por producto, categoría o global. Porcentuales o fijos |
| 👥 | Usuarios y roles | Varios cajeros con permisos diferenciados por rol |
| 🧾 | Impresión de tickets | Compatible con impresoras térmicas 58mm y 80mm |
| 📷 | Escaneo QR & barras | Lector USB o cámara — sin tipear nada |
| 🏦 | Corte de caja | Cortes parciales y finales, registro por forma de pago |
| ⚡ | Acceso rápido | Favoritos, atajos de teclado y búsqueda por nombre o código |
| 🔄 | Devoluciones | Parciales o totales, actualiza inventario automáticamente |
| 🔔 | Alertas | Notificaciones cuando un producto está por agotarse |

---

## 🏪 Tipos de negocio (para chips y video cards)

```
🛒 Abarrotes    🍽️ Restaurantes    🥘 Fondas
🧃 Minisupers   💊 Farmacias       ✂️ Estéticas
🍕 Pizzerías    🏪 Locales         🥩 Carnicerías
☕ Cafeterías   🧁 Panaderías      🍦 Heladerías
```

---

## 🎥 Videos (placeholders hasta tener los reales)

Para el **hero** y las **video cards** usar por ahora:
- `<video autoplay muted loop playsinline>` con `src` apuntando a un placeholder
- Placeholder sugerido: videos gratuitos de Pexels sobre tiendas / negocios
- Cuando se tengan los videos reales de clientes chihuahuenses → reemplazar src

Estructura de video card:
```astro
<div class="video-card group relative overflow-hidden rounded-2xl cursor-pointer">
  <video autoplay muted loop playsinline class="w-full h-full object-cover">
    <source src="/videos/abarrote.mp4" type="video/mp4" />
  </video>
  <div class="overlay absolute inset-0 bg-dark/70 group-hover:bg-dark/20 transition-all duration-500" />
  <div class="absolute bottom-0 left-0 p-6">
    <span class="text-verde text-sm font-semibold tracking-widest uppercase">Abarrotes</span>
    <h3 class="text-white font-display text-2xl font-bold mt-1">Para tu abarrote</h3>
    <p class="text-muted text-sm mt-2 opacity-0 group-hover:opacity-100 transition-opacity duration-300">
      Cobra, controla stock y fiados en un solo lugar.
    </p>
  </div>
</div>
```

---

## 💬 Copy principal

### Hero headline (opciones — escoger una)
- "El punto de venta que Chihuahua necesitaba"
- "Vende más. Controla todo. Desde $3,000."
- "Tu negocio, en control total."

### Hero subheadline
> Ventas, inventario, fiados, reportes y caja — todo en un solo sistema.
> Diseñado para los negocios de Chihuahua. Soporte 24/7, siempre en español.

### CTA principal
- "Obtener MI CAJA — $3,000 →"

### CTA secundario
- "Ver demo gratis"

### Sección soporte
> Somos un equipo del estado de Chihuahua. Sin centros de llamadas, sin bots.
> Te atendemos por WhatsApp, llamada o visita presencial — las 24 horas.

---

## 📁 Estructura del proyecto Astro

```
mi-caja/
├── public/
│   ├── logo/
│   │   └── mi_caja.png
│   ├── videos/
│   │   ├── hero-loop.mp4          ← video de fondo del hero
│   │   ├── abarrote.mp4
│   │   ├── restaurante.mp4
│   │   ├── fonda.mp4
│   │   └── ...
│   └── favicon.ico
├── src/
│   ├── components/
│   │   ├── Navbar.astro
│   │   ├── Hero.astro
│   │   ├── NegociosChips.astro
│   │   ├── Marquee.astro
│   │   ├── BentoFeatures.astro
│   │   ├── VideoCards.astro
│   │   ├── PrecioCard.astro
│   │   ├── SoporteSection.astro
│   │   ├── Testimoniales.astro
│   │   ├── CtaFinal.astro
│   │   └── Footer.astro
│   ├── layouts/
│   │   └── Base.astro             ← <html>, head, fonts, meta
│   ├── pages/
│   │   └── index.astro            ← ensambla todos los componentes
│   └── styles/
│       └── global.css             ← @tailwind, custom utilities, keyframes
├── tailwind.config.mjs
├── astro.config.mjs
└── CLAUDE.md                      ← este archivo
```

---

## ⚙️ Configuración inicial

### `astro.config.mjs`
```js
import { defineConfig } from 'astro/config';
import tailwind from '@astrojs/tailwind';

export default defineConfig({
  integrations: [tailwind()],
});
```

### `tailwind.config.mjs`
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./src/**/*.{astro,html,js,ts,jsx,tsx}'],
  theme: {
    extend: {
      colors: {
        verde:  '#1AE17A',
        azul:   '#1A6CFF',
        dark:   '#07090F',
        card:   '#0D1017',
        card2:  '#111520',
        text:   '#F0F2F8',
        muted:  'rgba(240,242,248,0.45)',
      },
      fontFamily: {
        display: ['Syne', 'sans-serif'],
        body:    ['DM Sans', 'sans-serif'],
      },
      borderColor: {
        DEFAULT: 'rgba(255,255,255,0.07)',
      },
      animation: {
        'marquee':    'marquee 22s linear infinite',
        'orb-float1': 'orbFloat1 12s ease-in-out infinite',
        'orb-float2': 'orbFloat2 15s ease-in-out infinite',
        'fade-up':    'fadeUp .7s ease both',
        'pulse-dot':  'pulseDot 2s ease infinite',
      },
      keyframes: {
        marquee: {
          '0%':   { transform: 'translateX(0)' },
          '100%': { transform: 'translateX(-50%)' },
        },
        orbFloat1: {
          '0%,100%': { transform: 'translate(0,0)' },
          '50%':     { transform: 'translate(60px,40px)' },
        },
        orbFloat2: {
          '0%,100%': { transform: 'translate(0,0)' },
          '50%':     { transform: 'translate(-40px,-60px)' },
        },
        fadeUp: {
          from: { opacity: '0', transform: 'translateY(24px)' },
          to:   { opacity: '1', transform: 'translateY(0)' },
        },
        pulseDot: {
          '0%,100%': { opacity: '1', transform: 'scale(1)' },
          '50%':     { opacity: '.4', transform: 'scale(1.5)' },
        },
      },
    },
  },
  plugins: [],
};
```

### `src/styles/global.css`
```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Fuentes Google */
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:wght@300;400;500;600&display=swap');

@layer base {
  html { scroll-behavior: smooth; }
  body {
    @apply bg-dark text-text font-body overflow-x-hidden;
  }
  h1, h2, h3, h4 { @apply font-display; }
}

@layer utilities {
  /* Texto con gradiente verde → azul */
  .text-gradient {
    background: linear-gradient(90deg, #1AE17A, #1A6CFF);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }

  /* Reveal animation classes */
  .reveal {
    opacity: 0;
    transform: translateY(30px);
    transition: opacity .7s ease, transform .7s ease;
  }
  .reveal.visible {
    opacity: 1;
    transform: translateY(0);
  }

  /* Glow verde */
  .glow-verde { box-shadow: 0 0 40px rgba(26,225,122,0.4); }
  .glow-azul  { box-shadow: 0 0 40px rgba(26,108,255,0.35); }

  /* Noise overlay */
  .noise::after {
    content: '';
    position: fixed; inset: 0; pointer-events: none; z-index: 999;
    opacity: 0.025;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
  }

  /* Fade mask para marquee */
  .marquee-fade::before,
  .marquee-fade::after {
    content: ''; position: absolute; top: 0; bottom: 0; width: 200px; z-index: 2;
  }
  .marquee-fade::before { left: 0; background: linear-gradient(to right, #07090F, transparent); }
  .marquee-fade::after  { right: 0; background: linear-gradient(to left, #07090F, transparent); }
}
```

---

## 🧩 Componente Navbar.astro — spec

```astro
---
// Navbar.astro
// - Sticky top-0 z-50
// - bg transparente → backdrop-blur con scroll (JS)
// - Logo: <img> mi_caja.png + "MI CAJA" en font-display
// - Links: Funciones · Precio · Soporte · Contacto
// - CTA pill: "Obtener ahora →" bg-verde text-dark font-display font-bold
// - En mobile: ocultar links, mostrar solo logo + CTA
---
```

## 🧩 Componente Hero.astro — spec

```astro
---
// Hero.astro
// Layout: 100vh, flex col, center, text-center
// Background: <video> loop muted autoplay — hero-loop.mp4
//   overlay: bg-dark/75 absolute inset-0
//   orbs animados por encima del video
// Contenido (z-10 relativo):
//   1. Badge pill — "● Software hecho en Chihuahua · Soporte 24/7"
//      border verde/25, bg verde/06, color verde, animate-pulse-dot
//   2. H1 — Syne 800, clamp(3.5rem, 8vw, 7rem), tracking-tight
//      última palabra o frase clave → .text-gradient
//   3. Subheadline — DM Sans, text-lg, text-muted, max-w-xl mx-auto
//   4. Botones — flex gap-3 justify-center
//      Primario: bg-verde text-dark hover:glow-verde
//      Secundario: border border-white/10 text-text hover:bg-white/5
//   5. Stats row — 4 stats en fila, borde redondeado, bg-white/2
//      $3,000 · 24/7 · 12 módulos · ∞ actualizaciones
// Animaciones: staggered fadeUp en cada elemento
---
```

## 🧩 Componente VideoCards.astro — spec (Toast-style)

```astro
---
// VideoCards.astro
// Grid 3 columnas desktop / 2 tablet / 1 mobile
// Cada card: 400px de alto, overflow-hidden, rounded-2xl
// Estructura interna:
//   - <video autoplay muted loop playsinline> — object-cover w-full h-full
//   - .overlay — bg-dark/70, group-hover:bg-dark/20, transition-opacity 500ms
//   - Texto abajo-izquierda:
//       tipo negocio en verde uppercase tracking-widest text-xs
//       título "Para tu [negocio]" en Syne bold text-2xl
//       descripción corta — opacity-0 group-hover:opacity-100
// Al hacer hover: video scale(1.04) transition 500ms cubic-bezier(.25,.46,.45,.94)
// Casos a mostrar (6 cards):
//   Abarrotes · Restaurantes · Fondas · Minisupers · Farmacias · Estéticas
---
```

## 🧩 Componente BentoFeatures.astro — spec

```astro
---
// BentoFeatures.astro
// CSS Grid 12 columnas, gap-4
// Cards:
//   - POS Mockup card: col-span-8 row-span-2 — muestra la pantalla del POS animada
//   - Precio card: col-span-4 — bg verde oscuro, borde verde, precio grande
//   - Soporte card: col-span-4 — bg azul oscuro, "24/7" en azul grande
//   - Feature cards (8): col-span-4 — icono grande + h3 + p corto
//   - Compat card: col-span-8 — chips de periféricos compatibles
//   - CTA card: col-span-12 — form WhatsApp centrado
// Hover en cards: border-verde/20 + glow cursor (mousemove JS)
// reveal class en cada card con delay escalonado
---
```

---

## 📱 Responsive Breakpoints

```
mobile:  < 640px  — 1 columna, nav simplificado, sin cursor custom
tablet:  640-1024px — 2 columnas en bento
desktop: > 1024px — layout completo 12 cols
```

---

## 🔗 Contacto / WhatsApp

- Número placeholder: `+52 614 100 0000` (cambiar al número real)
- Mensaje pre-cargado:
  ```
  Hola, me interesa MI CAJA punto de venta.
  Mi negocio es: [tipo]
  Mi nombre es: [nombre]
  ```
- Abrir con: `https://wa.me/526141000000?text=...`

---

## ✅ Definition of Done — checklist

Antes de considerar la landing terminada, debe cumplir:

- [ ] Lighthouse Performance ≥ 90 en mobile
- [ ] Videos con `loading="lazy"` y `preload="none"` para performance
- [ ] Todas las animaciones respetan `prefers-reduced-motion`
- [ ] Scroll reveal funcionando en todos los componentes
- [ ] Hover en video cards — overlay + scale + texto reveal
- [ ] Cursor glow siguiendo al mouse en bento cards
- [ ] Marquee con pause on hover
- [ ] Contadores animados al entrar al viewport
- [ ] Form de WhatsApp funcional
- [ ] Responsive perfecto en 375px, 768px, 1280px, 1440px
- [ ] Meta tags completos (OG image, description, title)
- [ ] Favicon con logo MI CAJA
- [ ] `prefers-color-scheme: dark` ya es el default (no hay light mode)

---

## 🚫 Cosas que NO hacer

- No usar React/JSX a menos que sea estrictamente necesario
- No instalar GSAP hasta que CSS no sea suficiente
- No usar gradientes purple/violeta — es el look genérico de IA
- No usar Inter, Roboto o Arial
- No poner light mode — la marca es dark-first
- No poner más de 2 CTAs distintos por sección
- No usar shadows muy visibles — la profundidad viene de los fondos y orbs
- No usar Tailwind `prose` para el copy — control manual total

---

## 🗒 Notas para Claude

Cuando trabajes en este proyecto:

1. **Siempre revisa este archivo primero** antes de crear o modificar un componente
2. **Un componente por archivo Astro** — no mezclar secciones
3. **Animaciones en global.css** si son keyframes reutilizables; inline si son únicas
4. **JS mínimo y vanilla** — nada de librerías para lo que se puede hacer nativo
5. **Los videos son críticos** — la sección VideoCards es la diferenciadora vs. la competencia
6. **El precio $3,000 siempre visible** — no enterrarlo. Es la ventaja competitiva principal vs. Square y Clover que cobran mensualidades altas
7. **Énfasis regional** — "Chihuahua" aparece al menos 3 veces en la página
8. **El CTA final es WhatsApp, no un form de email** — los dueños de abarrotes usan WhatsApp, no email

---

*Última actualización: 2025 · MI CAJA · Chihuahua, México 🌵*