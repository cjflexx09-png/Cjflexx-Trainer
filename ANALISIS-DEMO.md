# Análisis del demo CJ FLEXX — ¿Te va a ahorrar tiempo?

*Análisis del código del demo + investigación de mercado (julio 2026)*

## Veredicto rápido

**El demo actual resuelve bien 3 tareas (clientes, cobros, seguimiento InBody), pero NO cumple
tu requisito principal: mandar una rutina a un cliente en menos de 5 minutos. Esa función no
existe en el código — no hay constructor de rutinas, ni biblioteca de ejercicios, ni plantillas,
ni envío.** Es el módulo que falta construir antes de que la app te ahorre tiempo de verdad.

---

## 1. Lo que el demo SÍ hace (y lo hace bien)

| Módulo | Qué hace | ¿Ahorra tiempo? |
|---|---|---|
| **Dashboard** | MRR, clientes en riesgo, cobros pendientes, alertas de salud en una pantalla | Sí — reemplaza revisar Excel/WhatsApp cliente por cliente |
| **Riesgo de abandono (churn)** | Puntúa por días sin actividad (+1 si >7d, +2 si >14d), pago vencido (+2) y estancamiento (+1) | Sí — te dice a quién escribirle HOY sin que lo pienses |
| **Seguimiento InBody** | Peso, % grasa, músculo, agua, visceral, score; compara las 2 últimas mediciones y clasifica el progreso | Sí — registro en ~1 min por medición |
| **Alertas de salud** | Grasa visceral ≥7 e IMC fuera de rango, con aviso de que la decisión clínica es tuya | Sí — detección automática |
| **Cobros con Yappy** | Simula solicitud de pago, genera links y recordatorios para copiar a WhatsApp | Sí — Yappy real cobra 1% + ITBMS por transacción (mín. $0.02) |
| **Mensajes de re-enganche** | Genera texto personalizado según estado del cliente para copiar a WhatsApp | Sí — de 5 min pensando el mensaje a 10 segundos |

La lógica de negocio (churn, semáforo, estado de pago, alertas) es sólida y está bien pensada
para un flujo de trabajo panameño centrado en WhatsApp + Yappy.

## 2. Lo que FALTA para tu meta de "rutina en 5 minutos"

El demo no tiene nada de rutinas. Para cumplir esa meta necesitas construir:

1. **Biblioteca de ejercicios** — nombre, grupo muscular, video/GIF, notas técnicas.
2. **Plantillas de rutina** — armas una vez "Pérdida de grasa 3 días", "Hipertrofia 4 días", etc.
3. **Asignar = duplicar plantilla + ajustar cargas/series** — esto es lo que convierte 30–60 min
   en 2–3 min. Es el patrón estándar de la industria: las plantillas ahorran varias horas por
   semana y la asignación en sí toma segundos.
4. **Envío por WhatsApp** — link `wa.me/{telefono}?text=...` con la rutina o un link a la vista
   del cliente. Cero apps que el cliente tenga que instalar.

Con eso, el flujo queda: elegir cliente → elegir plantilla → ajustar → enviar = **menos de 5
minutos, realista y comprobado en el mercado**.

## 3. Estado técnico del demo

- **Todo vive en memoria**: al recargar la página se pierde todo (`state` en JS, sin backend ni
  base de datos). Para uso real necesita persistencia (aunque sea `localStorage` al inicio,
  luego un backend).
- **El código pegado tiene template literals sin backticks** en varias funciones (`fmtDate`,
  `payStatus`, badges, `toast`, mensajes, etc.) — probablemente se perdieron al copiar/pegar,
  pero tal cual está el archivo no ejecuta. Hay que restaurarlos.
- Sin autenticación, sin multiusuario, sin respaldo — normal para un demo, crítico para producción.
- **Datos de salud**: en producción, guardar composición corporal de clientes implica cumplir la
  Ley 81 de 2019 de protección de datos personales de Panamá (consentimiento, derecho a borrado).
  El demo ya tiene el campo `consent: true` — buen instinto.

## 4. ¿Comprar algo hecho o seguir con la app propia?

| Opción | Precio | Rutinas | Yappy | Español | WhatsApp |
|---|---|---|---|---|---|
| **TrueCoach** | $20/mes (5 clientes) | Excelente (fuerza/periodización) | No | No | No |
| **Everfit** | desde $19/mes | Muy bueno + IA, 1,000+ ejercicios | No | Parcial | No |
| **Trainerize** | desde ~$9/mes (2 clientes) | Bueno, general | No | Parcial | No |
| **Harbiz** | desde 14€/mes | Bueno, 100% español | No | No | No |
| **App propia (este demo)** | $0/mes + tu tiempo de desarrollo | **No existe aún** | **Sí (1% + ITBMS)** | Sí | Sí (copy/link) |

- Las plataformas comerciales ya tienen constructores de rutinas maduros con plantillas — ahí te
  ganan hoy. Pero **ninguna cobra con Yappy** (usan Stripe/tarjeta, que en Panamá es fricción
  para el cliente) y ninguna está pensada alrededor de WhatsApp.
- La ventaja real de tu app propia es exactamente esa combinación: Yappy nativo + WhatsApp +
  español + churn/InBody, sin cuota mensual por cliente.

## 5. Recomendación — roadmap para que sí te ahorre tiempo

1. **(Crítico)** Módulo de rutinas: biblioteca de ejercicios + plantillas + duplicar-y-ajustar.
2. **(Crítico)** Botón "Enviar por WhatsApp" (`wa.me`) para rutinas, cobros y re-enganche —
   hoy el demo solo copia al portapapeles.
3. Persistencia (localStorage → backend) para no perder datos al recargar.
4. Integración real del Botón de Pago Yappy (registro con Banco General:
   botondepagoYappy@bgeneral.com; APIs documentadas para Node/PHP/.Net).
5. Vista del cliente (link de solo lectura con su rutina y progreso) — sin app que instalar.

**Conclusión:** la app va por buen camino en gestión y cobros, y su enfoque local (Yappy +
WhatsApp) es una ventaja real que ninguna plataforma comercial ofrece. Pero tu tarea que más
tiempo consume — armar y mandar rutinas — todavía no está en el código. Ese es el próximo
módulo a construir; con plantillas, la meta de 5 minutos es totalmente alcanzable.

## Fuentes

- https://www.trainerize.com/blog/workout-builder-software/
- https://blog.everfit.io/everfit-vs-trainerize-vs-truecoach
- https://gymkee.com/compare/truecoach-vs-everfit/
- https://www.fitbudd.com/insights/everfit-vs-trainerize-vs-truecoach
- https://www.harbiz.io/en/personal-training
- https://www.feast-fit.com/blog/las-7-mejores-apps-para-entrenadores-personales-en-2026
- https://www.trainerize.com/blog/how-to-save-time-by-creating-client-workouts-in-trainerize/
- https://www.totalcoaching.com/blog/personal-training-systems-save-time/
- https://www.yappy.com.pa/comercial/boton-de-pago/
- https://www.yappy.com.pa/comercial/desarrolladores/boton-de-pago-yappy-nueva-integracion/
- https://www.yappy.com.pa/comercial/preguntas-frecuentes/
