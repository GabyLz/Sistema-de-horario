Preparación para desplegar en Render

1. Descripción

   Este proyecto está preparado para ejecutarse en Render como servicio web Node/Next.js.

2. Archivos añadidos

   - `render.yaml`: configuración del servicio (sin secrets).
   - `.renderignore`: archivos a excluir durante deploy.

3. Pasos en Render (resumen)

   - Conecta tu repositorio GitHub a Render.
   - Crea un nuevo servicio web y selecciona la rama `main`.
   - En Build Command usa: `npm run build`.
   - En Start Command usa: `npm run start`.
   - Añade las variables de entorno necesarias (no incluidas en el repo):
     - `DATABASE_URL` (Postgres, p.ej. Neon)
     - `REDIS_URL` y `REDIS_PASSWORD` si usas Redis
     - `NEXT_PUBLIC_API_URL` (URL pública de la app)
     - `JWT_SECRET`, `SMTP_*`, `WHATSAPP_*`, `TELEGRAM_BOT_TOKEN`, etc.
   - Si usas Prisma, en la sección de Environment vars añade `DATABASE_URL` y ejecuta `prisma migrate deploy` manualmente si necesitas migraciones.

4. Seguridad

   - NUNCA subas `.env.local` con secretos al repositorio.
   - Define todos los secretos en la interfaz de Render o en `render.yaml` usando secretos de Render (no aquí).

5. Notas adicionales

   - Si el build necesita más memoria (p.ej. por Puppeteer), ajusta el plan o usa un servicio con más recursos.
