# Impulso — CRM de ventas de OmniCore AI

CRM diseñado específicamente para el equipo de closers de OmniCore AI: prospectar, calificar, cerrar y dar seguimiento a restaurantes, hoteles y clínicas interesados en los 3 servicios de la agencia (Automatizaciones con IA / WhatsApp, Páginas web, Marketing & Publicidad con IA).

Es un archivo único (`index.html`) sin instalación ni build — se abre directo en el navegador. Los datos viven en `localStorage` por defecto, y se sincronizan en tiempo real entre todo el equipo si se conecta Supabase (recomendado para uso real).

---

## 🚀 Primeros pasos

1. Abre `index.html` en tu navegador (doble clic, o súbelo a cualquier hosting estático).
2. Sin configurar nada, verás un aviso de login con un enlace **"Continuar sin login por ahora"** — úsalo para probar la herramienta en modo local, con datos solo en tu navegador.
3. Para uso real en equipo, sigue **`SUPABASE_SETUP.md`** (10 minutos, no requiere saber programar) — activa login con correo/contraseña y comparte todos los datos entre el equipo.
4. Dentro del CRM, el **Dashboard** tiene una guía de inicio con 6 pasos que se marcan solos según lo que vayas completando.

---

## 📁 Archivos del proyecto

| Archivo | Para qué sirve |
|---|---|
| `index.html` | La aplicación completa. Es el único archivo que se necesita para usar el CRM. |
| `SUPABASE_SETUP.md` | Instrucciones paso a paso para conectar el login y la base de datos compartida. **Empieza aquí** si vas a usar el CRM con tu equipo. |
| `supabase_migration.sql` | Script SQL que crea las 7 tablas compartidas (equipo, prospectos, clientes, empresas, contactos, oportunidades, tareas). Se corre una sola vez en el SQL Editor de Supabase — es seguro repetirlo si algo falla. |
| `diagnostico_supabase.sql` | Consultas de diagnóstico si el login no sincroniza — confirma si las tablas y políticas de seguridad están bien creadas. |
| `diagnostico_oportunidades_tareas.sql` | Mismo diagnóstico, enfocado en los módulos de Pipeline y Tareas específicamente. |
| `GOOGLE_MAPS_SETUP.md` | Opcional. El Mapa global funciona de fábrica con OpenStreetMap (gratis); esto es solo si prefieren Google Maps. |

---

## 🧩 Módulos del CRM

- **Dashboard** — indicadores clave en tiempo real (nunca datos de ejemplo) + guía de inicio.
- **Alertas y tareas** — pendientes, llamadas y reuniones agendadas.
- **Equipo** — closers que usan el CRM; se vinculan solos al iniciar sesión.
- **Prospectos y clientes** — módulo unificado con pestañas: Prospectos (pre-venta, con encaje y falencia detectada), Clientes (post-venta, LTV y riesgo), Empresas (se completa sola al cargar un prospecto).
- **Mapa global** — ubicación y hora local exacta de cada prospecto, para coordinar llamadas entre husos horarios.
- **Pipeline** — mismas oportunidades de venta en dos vistas: Tablero (arrastrar y soltar entre etapas) y Tabla (filtrar/ordenar).

---

## ✨ Funciones destacadas

- **Encaje comercial automático** (0–100 pts): calcula qué tan buen prospecto es alguien según nicho, presupuesto, tamaño del negocio y falencia detectada — sin intervención manual.
- **Cotización y comisión en vivo**: al marcar los servicios de interés en un prospecto, calcula el precio (individual o paquete completo) y tu comisión estimada (20% setup + 10% mensual).
- **Mensajes de captación personalizados**: genera 3 mensajes de WhatsApp (uno por servicio) combinando el dolor típico del nicho, la falencia registrada, y un detalle que tú mismo agregues — se sugiere automáticamente después de crear cada prospecto o cliente.
- **Importación masiva** (CSV/Excel) con detección automática de columnas y vista previa antes de confirmar.
- **Captación desde Google** — registra negocios sin sitio web encontrados en Google Maps/reseñas.
- **Passkeys** (huella/Face ID) — solo funciona una vez el CRM esté publicado en un dominio real con HTTPS (no en modo local).
- **Modo compacto** (⊟ en la barra superior) — reduce espaciado en pantalla para ver más información de un vistazo.

---

## 🔒 Seguridad y decisiones de arquitectura

- **Nunca se expone una clave maestra de Supabase en el código.** Editar/eliminar cuentas de usuario se hace desde el panel de Supabase (Authentication → Users), no desde el CRM — hacerlo desde el navegador requeriría una clave que cualquiera podría ver en el código fuente.
- **Sincronización resistente a fallos**: si Supabase no responde (tabla no creada, política pendiente, sin internet), cada guardado cae automáticamente a modo local sin bloquear al usuario ni perder el trabajo, avisando claramente qué no se sincronizó.
- **Sin datos de ejemplo**: el CRM arranca completamente vacío. Todo lo que ves es lo que tú o tu equipo cargaron.

---

## 🛠️ Stack técnico

- HTML/CSS/JavaScript vanilla — sin frameworks, sin paso de build.
- [Supabase](https://supabase.com) — autenticación (correo/contraseña + passkeys) y base de datos Postgres compartida (opcional pero recomendado).
- [Leaflet](https://leafletjs.com) + OpenStreetMap — mapa global, gratis y sin llave de API.
- [SheetJS (xlsx)](https://sheetjs.com) — lectura de archivos CSV/Excel para importación masiva.

---

## 🗺️ Qué sigue (pendiente conocido)

- El campo `serviciosInteres` (servicios de interés marcados en un prospecto) todavía no viaja a Supabase — solo vive en el navegador. Requeriría agregar esa columna a la tabla `leads`.
- Ediciones que dependen de más de un registro (ej. eliminar una empresa con clientes vinculados) no tienen validación de integridad todavía.
