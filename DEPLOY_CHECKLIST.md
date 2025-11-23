# Checklist de Despliegue - Vaquita Celo

Usa este checklist para asegurarte de que todo esté configurado correctamente antes y después del despliegue.

## Pre-despliegue

- [ ] El proyecto compila sin errores (`pnpm build`)
- [ ] Todos los tests pasan (si los hay)
- [ ] El archivo `icon.png` existe en `apps/web/public/` (512x512px recomendado)
- [ ] El archivo `opengraph-image.png` existe en `apps/web/public/` (1200x630px)
- [ ] Has elegido una plataforma de despliegue (Vercel, Railway, Render, etc.)
- [ ] Tienes un dominio listo (o usarás el dominio proporcionado por la plataforma)

## Despliegue

- [ ] Proyecto desplegado en la plataforma elegida
- [ ] URL de producción obtenida (ej: `https://tu-proyecto.vercel.app`)
- [ ] Variables de entorno configuradas en la plataforma:
  - [ ] `NEXT_PUBLIC_URL` (debe ser la URL exacta de producción)
  - [ ] `NEXT_PUBLIC_APP_ENV=production`
  - [ ] `JWT_SECRET` (cadena aleatoria segura)

## Account Association

- [ ] Visitaste: `https://farcaster.xyz/~/developers/mini-apps/manifest?domain=tu-dominio.com`
- [ ] Iniciaste sesión con tu cuenta de Farcaster
- [ ] Firmaste el manifest con tu wallet
- [ ] Copiaste los tres valores:
  - [ ] `header`
  - [ ] `payload`
  - [ ] `signature`
- [ ] Agregaste las variables de entorno:
  - [ ] `NEXT_PUBLIC_FARCASTER_HEADER`
  - [ ] `NEXT_PUBLIC_FARCASTER_PAYLOAD`
  - [ ] `NEXT_PUBLIC_FARCASTER_SIGNATURE`
- [ ] Redesplegaste la aplicación después de agregar las variables

## Verificación Post-despliegue

- [ ] El endpoint del manifest funciona:
  ```bash
  curl https://tu-dominio.com/.well-known/farcaster.json
  ```
  - [ ] Retorna un JSON válido
  - [ ] Incluye `accountAssociation` con los valores correctos
  - [ ] Retorna código 200

- [ ] Las imágenes son accesibles:
  - [ ] `https://tu-dominio.com/icon.png`
  - [ ] `https://tu-dominio.com/opengraph-image.png`

- [ ] Verificación en Warpcast:
  - [ ] Abres Warpcast en móvil
  - [ ] Settings → Developer → Domains
  - [ ] Ingresas tu dominio
  - [ ] El account association aparece como válido

- [ ] Prueba funcional:
  - [ ] La miniapp carga correctamente en Warpcast
  - [ ] El botón "Add Miniapp" funciona
  - [ ] La autenticación funciona
  - [ ] La conexión de wallet funciona

## Troubleshooting

Si algo no funciona, verifica:

- [ ] El dominio en `NEXT_PUBLIC_URL` coincide EXACTAMENTE con el dominio usado para generar el account association
- [ ] Todas las variables de entorno están configuradas (sin valores placeholder)
- [ ] La aplicación fue redesplegada después de agregar las variables
- [ ] El build fue exitoso (revisa los logs de despliegue)
- [ ] No hay errores en la consola del navegador
- [ ] El dominio tiene SSL/HTTPS habilitado

## Comandos Útiles

```bash
# Verificar el manifest
curl https://tu-dominio.com/.well-known/farcaster.json | jq

# Generar JWT_SECRET seguro
openssl rand -base64 32

# Verificar build local
cd apps/web
pnpm build
pnpm start
```

## Notas Importantes

- ⚠️ El dominio debe coincidir EXACTAMENTE (incluyendo protocolo, subdominio, sin trailing slash)
- ⚠️ Si cambias de dominio, debes generar un nuevo account association
- ⚠️ Las variables de entorno deben estar configuradas ANTES del build en producción
- ⚠️ `JWT_SECRET` debe ser único y seguro, nunca lo compartas públicamente

