#work #frontend #coding #design
# Design System — Flow de trabajo

---

## Índice

1. [Herramientas y roles](#1-herramientas-y-roles)
2. [Arquitectura del proyecto](#2-arquitectura-del-proyecto)
3. [Tokens de diseño](#3-tokens-de-diseño)
4. [Flow UX → Dev](#4-flow-ux--dev)
5. [Configuración inicial del proyecto](#5-configuración-inicial-del-proyecto)
6. [Agregar y usar componentes de shadcn](#6-agregar-y-usar-componentes-de-shadcn)
7. [Crear componentes custom](#7-crear-componentes-custom)
8. [Documentar en Storybook](#8-documentar-en-storybook)
9. [Governance](#9-governance)
10. [Glosario](#10-glosario)

---

## 1. Herramientas y roles

| Herramienta | Rol |
|---|---|
| **Figma + Tokens Studio** | UX define y exporta los tokens de diseño |
| **shadcn/ui** | Dev implementa los componentes en React |
| **Style Dictionary** | Dev convierte el JSON de tokens en CSS variables |
| **Storybook** | UX valida, Dev documenta |

---

## 2. Arquitectura del proyecto

```
src/
├── components/
│   ├── ui/                  ← Componentes base de shadcn (NO modificar directamente)
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   ├── dialog.tsx
│   │   └── ...
│   └── custom/              ← Componentes propios construidos sobre shadcn
│       ├── DataTable/
│       ├── EmployeeCard/
│       └── ...
├── styles/
│   ├── globals.css          ← Punto de entrada de estilos
│   └── tokens.css           ← Generado automáticamente por Style Dictionary
├── tokens/
│   └── tokens.json          ← Entregado por UX desde Figma
├── stories/                 ← Archivos de Storybook por componente
└── lib/
    └── utils.ts             ← Función cn() de shadcn
```

---

## 3. Tokens de diseño

Los tokens son las variables visuales del sistema: colores, tipografías, espaciados y bordes. UX los define en Figma y los exporta como `tokens.json`. Dev los convierte en CSS variables que toda la app consume.

### Estructura del tokens.json

```json
{
  "color": {
    "primary": "#1E40AF",
    "primary-hover": "#1D3FA0",
    "secondary": "#64748B",
    "danger": "#DC2626",
    "success": "#16A34A",
    "warning": "#D97706",
    "background": "#F8FAFC",
    "surface": "#FFFFFF",
    "text-primary": "#0F172A",
    "text-secondary": "#64748B",
    "border": "#E2E8F0"
  },
  "font": {
    "family": "Inter, sans-serif",
    "size": {
      "xs": "12px",
      "sm": "14px",
      "base": "16px",
      "lg": "18px",
      "xl": "24px",
      "2xl": "30px"
    },
    "weight": {
      "normal": "400",
      "medium": "500",
      "semibold": "600",
      "bold": "700"
    }
  },
  "spacing": {
    "xs": "4px",
    "sm": "8px",
    "md": "16px",
    "lg": "24px",
    "xl": "32px",
    "2xl": "48px"
  },
  "border": {
    "radius": {
      "sm": "4px",
      "md": "6px",
      "lg": "8px",
      "full": "9999px"
    }
  }
}
```

### CSS variables generadas

Style Dictionary convierte ese JSON en `src/styles/tokens.css`:

```css
:root {
  --color-primary: #1E40AF;
  --color-primary-hover: #1D3FA0;
  --color-secondary: #64748B;
  --color-danger: #DC2626;
  --color-success: #16A34A;
  --color-background: #F8FAFC;
  --color-text-primary: #0F172A;
  --color-border: #E2E8F0;

  --font-family: Inter, sans-serif;
  --font-size-base: 16px;
  --font-size-lg: 18px;

  --spacing-sm: 8px;
  --spacing-md: 16px;
  --spacing-lg: 24px;

  --border-radius-md: 6px;
  --border-radius-lg: 8px;
}
```

Todos los componentes consumen estas variables. Nunca se hardcodean valores visuales en el código.

---

## 4. Flow UX → Dev

### Flujo general

```
UX: Diseña pantallas en Figma usando el kit de componentes de shadcn
        ↓
UX: Define o actualiza tokens en Tokens Studio (plugin de Figma)
        ↓
UX: Exporta tokens.json y lo sube al repositorio
        ↓
Dev: Corre npm run build:tokens → se regenera src/styles/tokens.css
        ↓
Dev: Implementa las pantallas usando componentes de shadcn
        ↓
Dev: Si necesita un componente custom, lo crea en /components/custom/
        ↓
Dev: Documenta el componente nuevo en Storybook
        ↓
UX: Valida en Storybook que la implementación coincide con el diseño
```

### Flujo de un cambio visual (ejemplo: cambio de color primario)

```
UX: Abre Tokens Studio en Figma
        ↓
UX: Cambia color.primary de #1E40AF a #16A34A
        ↓
UX: Exporta tokens.json actualizado y lo sube al repositorio
        ↓
Dev: Corre npm run build:tokens
        ↓
tokens.css se regenera con --color-primary: #16A34A
        ↓
Todos los componentes que usan --color-primary se actualizan solos
```

---

## 5. Configuración inicial del proyecto

### Prerequisitos

- Node.js 18+
- React 18+
- Tailwind CSS configurado

### Paso 1: Inicializar shadcn

```bash
npx shadcn@latest init
```

Opciones recomendadas:
- Style: **Default**
- Base color: **Slate**
- CSS variables: **Sí**

### Paso 2: Instalar Style Dictionary

```bash
npm install --save-dev style-dictionary
```

Crear `sd.config.js` en la raíz del proyecto:

```javascript
module.exports = {
  source: ['tokens/tokens.json'],
  platforms: {
    css: {
      transformGroup: 'css',
      buildPath: 'src/styles/',
      files: [
        {
          destination: 'tokens.css',
          format: 'css/variables',
          options: {
            selector: ':root'
          }
        }
      ]
    }
  }
}
```

Agregar el script en `package.json`:

```json
{
  "scripts": {
    "build:tokens": "style-dictionary build --config sd.config.js"
  }
}
```

### Paso 3: Importar tokens en los estilos globales

En `src/styles/globals.css`:

```css
@import './tokens.css';
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### Paso 4: Instalar Storybook

```bash
npx storybook@latest init
```

```bash
npm run storybook        # Corre en localhost:6006
npm run build-storybook  # Genera el build estático para desplegar
```

---

## 6. Agregar y usar componentes de shadcn

### Agregar un componente

```bash
npx shadcn@latest add button
npx shadcn@latest add input
npx shadcn@latest add dialog
npx shadcn@latest add table
```

Cada comando copia el código del componente en `src/components/ui/`. Ver todos los disponibles en [ui.shadcn.com](https://ui.shadcn.com/components).

### Uso en pantallas

```tsx
import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"
import { Label } from "@/components/ui/label"

export function FormularioEmpleado() {
  return (
    <div>
      <Label htmlFor="nombre">Nombre</Label>
      <Input id="nombre" placeholder="Ingresá el nombre" />

      <Button>Guardar</Button>
      <Button variant="outline">Cancelar</Button>
      <Button variant="destructive">Eliminar</Button>
    </div>
  )
}
```

### Variantes de Button

```tsx
<Button variant="default">Principal</Button>
<Button variant="outline">Secundario</Button>
<Button variant="ghost">Sutil</Button>
<Button variant="destructive">Peligroso</Button>
<Button size="sm">Chico</Button>
<Button size="lg">Grande</Button>
<Button disabled>Deshabilitado</Button>
```

### Dialog (Modal)

```tsx
import {
  Dialog,
  DialogContent,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog"

<Dialog>
  <DialogTrigger asChild>
    <Button>Abrir</Button>
  </DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Confirmar acción</DialogTitle>
    </DialogHeader>
    <p>¿Estás seguro que querés continuar?</p>
  </DialogContent>
</Dialog>
```

### La función `cn`

Usar `cn` para agregar clases de Tailwind sin romper las del componente:

```tsx
import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"

<Button className={cn("w-full mt-4")}>
  Botón de ancho completo
</Button>
```

> **Regla:** Nunca hardcodear colores como `text-blue-500` o `bg-[#1E40AF]`. Siempre usar las CSS variables del sistema: `var(--color-primary)`.

---

## 7. Crear componentes custom

Cuando se necesita un componente específico del producto que no existe en shadcn, se crea en `/src/components/custom/` usando componentes base de shadcn.

### Estructura

```
src/components/custom/EmployeeCard/
├── EmployeeCard.tsx
├── EmployeeCard.stories.tsx
└── index.ts
```

### Ejemplo: EmployeeCard

```tsx
// src/components/custom/EmployeeCard/EmployeeCard.tsx

import { Card, CardContent, CardHeader } from "@/components/ui/card"
import { Badge } from "@/components/ui/badge"
import { Button } from "@/components/ui/button"
import { cn } from "@/lib/utils"

interface EmployeeCardProps {
  name: string
  position: string
  department: string
  status: "active" | "inactive" | "vacation"
  onViewProfile?: () => void
}

const statusStyles = {
  active: "bg-green-100 text-green-800",
  inactive: "bg-gray-100 text-gray-800",
  vacation: "bg-yellow-100 text-yellow-800",
}

const statusLabels = {
  active: "Activo",
  inactive: "Inactivo",
  vacation: "Vacaciones",
}

export function EmployeeCard({
  name,
  position,
  department,
  status,
  onViewProfile,
}: EmployeeCardProps) {
  return (
    <Card>
      <CardHeader className="flex flex-row items-center justify-between">
        <div>
          <h3 className="font-semibold text-lg">{name}</h3>
          <p className="text-sm text-muted-foreground">{position}</p>
        </div>
        <Badge className={cn(statusStyles[status])}>
          {statusLabels[status]}
        </Badge>
      </CardHeader>
      <CardContent>
        <p className="text-sm mb-4">
          <span className="font-medium">Área:</span> {department}
        </p>
        {onViewProfile && (
          <Button variant="outline" size="sm" onClick={onViewProfile}>
            Ver perfil
          </Button>
        )}
      </CardContent>
    </Card>
  )
}
```

```ts
// src/components/custom/EmployeeCard/index.ts
export { EmployeeCard } from "./EmployeeCard"
```

### Uso

```tsx
import { EmployeeCard } from "@/components/custom/EmployeeCard"

<EmployeeCard
  name="María García"
  position="Analista de RRHH"
  department="Recursos Humanos"
  status="active"
  onViewProfile={() => navigate("/empleados/123")}
/>
```

---

## 8. Documentar en Storybook

Todo componente custom debe tener su archivo `.stories.tsx`. Sin este archivo, el PR no se aprueba.

```tsx
// src/stories/EmployeeCard.stories.tsx

import type { Meta, StoryObj } from "@storybook/react"
import { EmployeeCard } from "@/components/custom/EmployeeCard"

const meta: Meta<typeof EmployeeCard> = {
  title: "Custom/EmployeeCard",
  component: EmployeeCard,
  tags: ["autodocs"],
}

export default meta
type Story = StoryObj<typeof EmployeeCard>

export const Activo: Story = {
  args: {
    name: "María García",
    position: "Analista de RRHH",
    department: "Recursos Humanos",
    status: "active",
  },
}

export const EnVacaciones: Story = {
  args: {
    name: "Juan Pérez",
    position: "Desarrollador",
    department: "Tecnología",
    status: "vacation",
  },
}

export const Inactivo: Story = {
  args: {
    name: "Carlos López",
    position: "Contador",
    department: "Finanzas",
    status: "inactive",
  },
}
```

Storybook desplegado en: **[URL interna — completar]**

Se actualiza automáticamente con cada merge a la rama principal.

---

## 9. Governance

### Roles

| Rol           | Responsabilidad                                                    |
| ------------- | ------------------------------------------------------------------ |
| **Tech Lead** | Revisa y aprueba cambios a componentes.                            |
| **UX**        | Define y actualiza tokens. Aprueba variantes visuales nuevas       |
| **Dev**       | Implementa pantallas. Propone componentes nuevos cuando hace falta |

### Cuándo crear un componente custom

```
¿Existe en shadcn?                   → Usarlo directamente
¿Existe en /components/custom/?      → Usarlo o proponer variante al maintainer
¿Solo se usa en una pantalla?        → Puede vivir en el archivo de esa pantalla
¿Se reutiliza en 2 o más pantallas?  → Crear en /components/custom/ + documentar en Storybook
```

### Cómo proponer un componente nuevo

1. Abrir un ticket en **[Jira/Linear/GitHub Issues — completar]** con el tag `design-system`
2. Incluir: nombre, para qué se usa, qué pantallas lo necesitan
3. UX diseña en Figma
4. Maintainer aprueba y asigna
5. Dev implementa, documenta en Storybook y abre PR
6. Maintainer hace code review y merge

### Reglas

- Nunca hardcodear valores visuales. Siempre usar tokens (`var(--color-primary)`)
- No modificar archivos en `/components/ui/` sin pasar por el maintainer
- Todo componente custom requiere su archivo `.stories.tsx`
- Un componente por archivo

---

## 10. Glosario

| Término | Significado |
|---|---|
| **Token de diseño** | Variable con nombre que representa una decisión visual: `color.primary`, `spacing.md` |
| **CSS Variable** | Variable en CSS generada desde los tokens. Ej: `var(--color-primary)` |
| **shadcn/ui** | Librería de componentes React. El código se copia directamente al proyecto |
| **Storybook** | Catálogo visual interactivo de los componentes del sistema |
| **Tokens Studio** | Plugin de Figma para definir y exportar tokens como JSON |
| **Style Dictionary** | Convierte `tokens.json` en CSS variables |
| **Tailwind CSS** | Framework de CSS por clases utilitarias. Base de shadcn |
| **Radix UI** | Primitivos accesibles sin estilos que usa shadcn internamente |
| **Maintainer** | Responsable de aprobar cambios al design system |
| **PR (Pull Request)** | Propuesta de cambio de código que requiere revisión antes de mergear |
| **Hardcodear** | Escribir un valor directo en el código (`#1E40AF`) en lugar de usar una variable. Práctica prohibida |

---

*Última actualización: Marzo 2026*
*Dudas sobre esta documentación: abrir un ticket con el tag `design-system-docs`*
