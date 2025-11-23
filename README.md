# vaquita-celo

Vaquita is a Celo miniapp that gamifies saving: you deposit, grow your virtual cow, and earn rewards for staying consistent.

A modern Celo blockchain application built with Next.js, TypeScript, and Turborepo.

## Getting Started

1. Install dependencies:
   ```bash
   pnpm install
   ```

2. Start the development server:
   ```bash
   pnpm dev
   ```

3. Open [http://localhost:3000](http://localhost:3000) in your browser.

## Project Structure

This is a monorepo managed by Turborepo with the following structure:

- `apps/web` - Next.js application with embedded UI components and utilities

## Available Scripts

- `pnpm dev` - Start development servers
- `pnpm build` - Build all packages and apps
- `pnpm lint` - Lint all packages and apps
- `pnpm type-check` - Run TypeScript type checking

## Tech Stack

- **Framework**: Next.js 14 with App Router
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **UI Components**: shadcn/ui
- **Monorepo**: Turborepo
- **Package Manager**: PNPM

## Despliegue en Farcaster

Esta guía te ayudará a desplegar tu miniapp de Farcaster paso a paso.

### Prerrequisitos

- Una cuenta de Farcaster
- Un dominio público (para producción) o ngrok (para desarrollo)
- Una plataforma de despliegue (Vercel, Railway, Render, etc.)

### Opción 1: Despliegue en Vercel (Recomendado)

#### Paso 1: Preparar el proyecto

1. **Asegúrate de que el proyecto compile correctamente:**
   ```bash
   pnpm build
   ```

2. **Verifica que todos los archivos necesarios estén presentes:**
   - `apps/web/public/icon.png` - Icono de la app (512x512px recomendado)
   - `apps/web/public/opengraph-image.png` - Imagen para Open Graph (1200x630px)

#### Paso 2: Conectar con Vercel

**Opción A: Usando el Dashboard de Vercel (Recomendado)**

1. **Ve a [vercel.com](https://vercel.com)** e inicia sesión con tu cuenta de GitHub

2. **Importa tu repositorio:**
   - Haz clic en **"Add New..."** → **"Project"**
   - Selecciona el repositorio `vaquita-fi/vaquita-celo`
   - En la configuración del proyecto, busca **"Root Directory"**
   - Haz clic en **"Edit"** y cambia el Root Directory a: `apps/web`
   - Framework Preset: **Next.js** (debería detectarse automáticamente)
   - Build Command: `pnpm build` (o déjalo vacío para usar el default)
   - Output Directory: `.next` (default)
   - Install Command: `pnpm install` (o déjalo vacío para usar el default)

3. **Haz clic en "Deploy"**

4. **Obtén tu URL de producción:**
   Vercel te dará una URL como `https://tu-proyecto.vercel.app`

**Opción B: Usando Vercel CLI**

1. **Instala Vercel CLI (si no lo tienes):**
   ```bash
   npm i -g vercel
   ```

2. **Inicia sesión en Vercel:**
   ```bash
   vercel login
   ```

3. **Despliega el proyecto desde la raíz:**
   ```bash
   vercel
   ```
   
   Cuando te pregunte:
   - ¿Quieres modificar la configuración? → **Sí**
   - ¿Qué directorio quieres desplegar? → **apps/web**
   - Build Command → `pnpm build`
   - Output Directory → `.next`
   - Install Command → `pnpm install`

4. **Obtén tu URL de producción:**
   Vercel te dará una URL como `https://tu-proyecto.vercel.app`

**⚠️ IMPORTANTE para Monorepos:**
- El archivo `vercel.json` en la raíz del proyecto ya está configurado para ayudar a Vercel
- Si aún tienes problemas, asegúrate de que en el **Dashboard de Vercel** → **Settings** → **General**, el **Root Directory** esté configurado como `apps/web`

#### Paso 3: Configurar el dominio personalizado (Opcional)

1. En el dashboard de Vercel, ve a **Settings** → **Domains**
2. Agrega tu dominio personalizado (ej: `vaquita-celo.com`)
3. Configura los registros DNS según las instrucciones de Vercel

#### Paso 4: Generar Account Association

1. **Ve a la herramienta de generación de manifest:**
   ```
   https://farcaster.xyz/~/developers/mini-apps/manifest?domain=tu-dominio.com
   ```
   
   Reemplaza `tu-dominio.com` con tu dominio real (ej: `tu-proyecto.vercel.app` o `vaquita-celo.com`)

2. **Inicia sesión con tu cuenta de Farcaster**

3. **Firma el manifest** usando tu wallet de Farcaster

4. **Copia los tres valores generados:**
   - `header` (ej: `eyJmaWQiOjM2MjEsInR5cGUiOiJjdXN0b2R5Iiwia2V5Ijoi...`)
   - `payload` (ej: `eyJkb21haW4iOiJ0dS1kb21pbmlvLmNvbSJ9`)
   - `signature` (ej: `0x1234abcd...`)

#### Paso 5: Configurar Variables de Entorno en Vercel

1. **Ve al dashboard de Vercel** → Tu proyecto → **Settings** → **Environment Variables**

2. **Agrega las siguientes variables:**

   ```
   NEXT_PUBLIC_URL=https://tu-dominio.com
   NEXT_PUBLIC_APP_ENV=production
   NEXT_PUBLIC_FARCASTER_HEADER=tu-header-aqui
   NEXT_PUBLIC_FARCASTER_PAYLOAD=tu-payload-aqui
   NEXT_PUBLIC_FARCASTER_SIGNATURE=tu-signature-aqui
   JWT_SECRET=genera-un-secreto-seguro-aqui
   ```

   **Importante:**
   - `NEXT_PUBLIC_URL` debe coincidir EXACTAMENTE con el dominio que usaste para generar el account association
   - `JWT_SECRET` debe ser una cadena aleatoria segura (puedes generarla con: `openssl rand -base64 32`)

3. **Selecciona los entornos** donde aplicar las variables (Production, Preview, Development)

4. **Guarda los cambios**

#### Paso 6: Redesplegar la aplicación

1. **En Vercel**, ve a **Deployments**
2. Haz clic en los **3 puntos** del último deployment → **Redeploy**
3. O ejecuta:
   ```bash
   vercel --prod
   ```

#### Paso 7: Verificar el despliegue

1. **Verifica el endpoint del manifest:**
   ```bash
   curl https://tu-dominio.com/.well-known/farcaster.json
   ```
   
   Deberías ver un JSON con la estructura del manifest y el `accountAssociation`.

2. **Verifica en Warpcast:**
   - Abre Warpcast en tu móvil
   - Ve a **Settings** → **Developer** → **Domains**
   - Ingresa tu dominio
   - Deberías ver que el account association es válido

3. **Prueba la miniapp:**
   - En Warpcast, busca tu dominio en el embed tool
   - O comparte un link de tu dominio en un cast
   - La miniapp debería cargar correctamente

### Opción 2: Despliegue en Railway

#### Paso 1: Preparar el proyecto

1. Crea un archivo `railway.json` en la raíz del proyecto:
   ```json
   {
     "build": {
       "builder": "NIXPACKS"
     },
     "deploy": {
       "startCommand": "cd apps/web && pnpm start",
       "restartPolicyType": "ON_FAILURE",
       "restartPolicyMaxRetries": 10
     }
   }
   ```

2. Crea un archivo `Procfile` en `apps/web/`:
   ```
   web: pnpm start
   ```

#### Paso 2: Desplegar en Railway

1. **Conecta tu repositorio** en Railway
2. **Selecciona el proyecto** y configura:
   - **Root Directory**: `apps/web`
   - **Build Command**: `pnpm install && pnpm build`
   - **Start Command**: `pnpm start`

#### Paso 3: Configurar Variables de Entorno

1. En Railway, ve a **Variables**
2. Agrega las mismas variables que en Vercel (ver Paso 5 arriba)

#### Paso 4: Obtener dominio y generar Account Association

1. Railway te dará un dominio como `tu-proyecto.up.railway.app`
2. Sigue los pasos 4-7 de la opción Vercel

### Opción 3: Despliegue en Render

#### Paso 1: Preparar el proyecto

1. Crea un archivo `render.yaml` en la raíz:
   ```yaml
   services:
     - type: web
       name: vaquita-celo
       env: node
       buildCommand: cd apps/web && pnpm install && pnpm build
       startCommand: cd apps/web && pnpm start
       envVars:
         - key: NODE_VERSION
           value: 18.x
   ```

#### Paso 2: Desplegar en Render

1. Conecta tu repositorio en Render
2. Crea un nuevo **Web Service**
3. Configura:
   - **Root Directory**: `apps/web`
   - **Build Command**: `pnpm install && pnpm build`
   - **Start Command**: `pnpm start`

#### Paso 3: Configurar Variables y Account Association

Sigue los mismos pasos que en Vercel (Pasos 4-7)

### Troubleshooting

#### Error: "Account association not configured"

- Verifica que las tres variables (`NEXT_PUBLIC_FARCASTER_HEADER`, `NEXT_PUBLIC_FARCASTER_PAYLOAD`, `NEXT_PUBLIC_FARCASTER_SIGNATURE`) estén configuradas
- Asegúrate de que los valores no sean placeholders
- Verifica que `NEXT_PUBLIC_URL` coincida exactamente con el dominio usado para generar el account association

#### Error: "No valid account association" en Warpcast

- El dominio en el payload firmado debe coincidir EXACTAMENTE con tu dominio desplegado
- Verifica que el endpoint `/.well-known/farcaster.json` retorne un 200
- Asegúrate de que el JSON tenga la estructura correcta

#### Error de dominio no coincide

- El dominio firmado debe coincidir exactamente, incluyendo:
  - Protocolo (`https://`)
  - Subdominio (si aplica)
  - Sin trailing slash
- Si cambias de dominio, debes generar un nuevo account association

#### La miniapp no carga en Warpcast

- Verifica que el manifest endpoint funcione: `curl https://tu-dominio.com/.well-known/farcaster.json`
- Revisa la consola del navegador para errores
- Asegúrate de que todas las imágenes (`icon.png`, `opengraph-image.png`) sean accesibles

### Recursos Adicionales

- [Documentación de Farcaster Mini Apps](https://miniapps.farcaster.xyz/)
- [Especificación del Manifest](https://miniapps.farcaster.xyz/specification/manifest)
- [Guía de Account Association](https://farcaster.xyz/~/developers/mini-apps/manifest)
- [Documentación de Vercel](https://vercel.com/docs)
- [Documentación de Railway](https://docs.railway.app/)
- [Documentación de Render](https://render.com/docs)

## Learn More

- [Next.js Documentation](https://nextjs.org/docs)
- [Celo Documentation](https://docs.celo.org/)
- [Turborepo Documentation](https://turbo.build/repo/docs)
- [shadcn/ui Documentation](https://ui.shadcn.com/)
