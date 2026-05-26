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

   ## Error común: `Exited with status 127` durante el build

   Significado: `127` suele indicar "command not found" — p. ej. `prisma` no estaba disponible porque `prisma` está en `devDependencies` y no se instalaron las dependencias de desarrollo durante el install.

   Solución aplicada aquí:

    - Añadimos en `render.yaml` la propiedad `installCommand: npm ci --include=dev` para que Render instale también las `devDependencies` (esto permite ejecutar `prisma generate` durante el build).

   Alternativas:

    - Mover `prisma` a `dependencies` en `package.json` (menos recomendado).
    - Ejecutar migraciones manualmente desde la consola de Render en vez de automatizarlas.

   Después de este cambio, vuelve a disparar un deploy en la rama `render-setup`.

4. Seguridad

   - NUNCA subas `.env.local` con secretos al repositorio.
   - Define todos los secretos en la interfaz de Render o en `render.yaml` usando secretos de Render (no aquí).

5. Notas adicionales

   - Si el build necesita más memoria (p.ej. por Puppeteer), ajusta el plan o usa un servicio con más recursos.
