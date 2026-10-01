# Guía del repositorio

## Ejecución y verificación
- Abrir `index.html` en el navegador; no hay instalación ni compilación.
- No hay suite de pruebas, lint ni typecheck configurados. Si Node está disponible,
  `node --check game.js` comprueba únicamente la sintaxis.
- Para cambios de comportamiento, verificar en el navegador inicio/reinicio,
  pausa/reanudación, controles afectados y actualización de puntuación/récord.

## Integración
- `index.html` carga `game.js` con `defer`. El juego es una IIFE sin exports:
  obtiene directamente los elementos por ID y registra sus eventos al cargar.
- `game.js` lee `--lcd`, `--lcd-apagado` y `--tinta-lcd` de `styles.css` una sola
  vez al arrancar; cambiar esas variables después no actualiza el canvas.
- La geometría está repartida entre archivos: `COLS=20`, `ROWS=14`, `CELDA=24`
  y `MARCO=4` corresponden al canvas de 488 × 344 en `index.html`.
- `paso()` gobierna movimiento y puntuación; `cuadro()` lo ejecuta mediante
  un acumulador de tiempo y puede procesar hasta cuatro pasos por frame.
  Las reglas basadas en pasos deben contar movimientos, no frames.
- Al comer, el nivel se actualiza antes de sumar `10 * nivel`: la quinta
  manzana ya puntúa con nivel 2.
- El récord usa la clave `viborita-lcd.mejor` de localStorage, se guarda al
  perder y aparece tanto en `#record` como en `#d-mejor`. Si falla el
  almacenamiento, se conserva en memoria durante la sesión.
- Enter y Espacio tienen manejadores globales. Los botones direccionales
  detienen su propagación para evitar reiniciar o pausar accidentalmente.
- El tema oscuro cambia solo la superficie exterior; la paleta del aparato
  permanece fija. El parpadeo respeta `prefers-reduced-motion`.

## Cambios solicitados
- Consultar `CAMBIOS.md` al trabajar en borrar récord, manzana dorada o racha.
  Para esas tres tareas exige una rama y un worktree por cambio, desarrollo
  en paralelo e integración en `main`; dorada × racha debe dar ×6.
