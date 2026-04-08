> **Nota:** Este repositorio es la documentación pública del proyecto. El código fuente se mantiene privado por contener lógica de negocio sensible y datos de clientes reales en producción. Si querés ver una demo en vivo o tenés preguntas técnicas específicas, escribime directamente a natalia.zoque@gmail.com.

---

# Chef Intelligence 🍳

**Chef Intelligence** es un centro de comando operativo para restaurantes y puntos de venta. Centraliza consumos de insumos, ventas diarias y catálogo de costos para permitir análisis en tiempo real, costeo de recetas interactivo y proyecciones de pedidos mediante un asistente de IA conversacional.

---

## El problema real

Los operadores de restaurantes manejan su información operativa en hojas de cálculo de Excel o Drive desconectadas que se actualizan a destiempo. El dolor es concreto:

1. **Blind spots financieros:** Se calculan costos de recetas usando precios de insumos de hace 3 meses.
2. **Desgaste operativo:** Se pierden horas cruzando datos entre sistemas para entender el margen real.
3. **Pedidos "a ojo":** Se pide mercadería basándose en intuición, lo que termina en mermas por sobre-stock o quiebres de inventario que frenan las ventas.

---

## Cómo funciona

1. **Ingesta automatizada:** Los administradores suben archivos de cierre (ventas, consumos, costos) a Google Drive. Un pipeline limpia, cruza y guarda los datos en la plataforma, permitiendo cargas incrementales o reconstrucciones completas del mes.
2. **Visualización hiper-filtrada:** Cada operador ve automáticamente solo la data de su PDV asignado — KPIs inmediatos: ticket promedio, top insumos gastados, picos de venta.
3. **Chat analítico y costeo:** El usuario le pregunta al bot: *"¿Cuánto consumí de carne la semana pasada?"* o *"Costea esta receta: 150g de carne, 2 panes y 20g de cheddar."* El sistema convierte unidades, calcula con el último costo en base de datos y predice el stock sugerido para la siguiente semana.

---

## Tecnologías usadas

| Capa | Tecnología |
|------|-----------|
| Frontend | React + Vite + TypeScript |
| Estilos y UI | Tailwind CSS + shadcn/ui + Lucide Icons |
| Estado y ruteo | Zustand + React Router |
| Backend & Auth | Supabase (PostgreSQL + RLS) |
| Gráficos y exportes | Recharts + jsPDF + SheetJS |
| ETL e inteligencia | n8n + agentes LLM (Claude/Gemini vía webhook) |

---

## Demo

🎥 [Ver video demostrativo](#) — *(link disponible bajo solicitud)*
🔗 [Probar la aplicación en vivo](#) — *(link disponible bajo solicitud)*

---

## Estado actual

MVP en iteración activa. Toda la arquitectura base, autenticación, flujos RLS en Supabase y scaffolding de módulos del frontend (Dashboards, Consumos, Ventas, Costeos) están operativos. Actualmente enfocados en pulir el webhook bidireccional del bot y la solidez del motor predictivo de pedidos.

---

## Decisiones de producto

**Row Level Security desde el día 1**
Manejar datos financieros de distintos franquiciados es delicado. La seguridad se trasladó directamente a las políticas de tabla en Postgres. Un cajero de un PDV no puede ver la data de otro, y el bot hereda exactamente el mismo token de sesión. Cero fugas de información.

**ETL asíncrono en lugar de parseo en el frontend**
Procesar miles de transacciones en el navegador bloqueaba el hilo principal y era muy frágil ante errores de formato. Moverlo a n8n leyendo de Drive mantuvo el frontend como un visor rápido y delegó el trabajo pesado al backend.

**UI de chat híbrida**
Un chat de texto plano es poco denso en información. El LLM devuelve un esquema JSON y el frontend renderiza tarjetas, tablas y gráficos interactivos al lado de la conversación.

---

## Qué haría diferente hoy

**Esquematización para la IA desde el inicio**
El lenguaje natural a SQL es propenso a alucinaciones. Hubiera sido más inteligente construir vistas materializadas específicas en PostgreSQL más planas y digeribles para el agente LLM, en lugar de hacerlo pelear con la estructura relacional pura.

**Validación temprana del input humano**
La dependencia de que los gerentes llenen bien el Excel en Drive es el eslabón débil. Agregaría una capa de pre-validación más dura — un linter de Excels — que detecte errores de formato antes de que el job de ingesta falle silenciosamente.

---

Construido por Natalia Zoque · [Labs IngenIA](https://www.labsingenia.com)
