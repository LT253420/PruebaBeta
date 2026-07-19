# DOCUMENTACION DEL PROYECTO - NOVACELL DIAGNOSTICO

**Sistema de Diagnostico Profesional para Tecnicos de Celulares**

---

## 1. INFORMACION GENERAL

### Que es este proyecto?
Este es un sistema de diagnostico para tecnicos de reparacion de celulares. Permite buscar fallas comunes y ver sus posibles soluciones.

### Tecnologias utilizadas
- **React** - Biblioteca para crear interfaces de usuario
- **TypeScript** - JavaScript con tipos (ayuda a evitar errores)
- **Vite** - Herramienta para desarrollar y construir la aplicacion
- **Tailwind CSS** - Framework de estilos (colores, espacios, etc.)
- **Firebase** - Autenticacion con Google
- **Lucide React** - Iconos

### Version
v2.0

---

## 2. ESTRUCTURA DE CARPETAS

```
project/
├── public/                     # Archivos publicos (accesibles desde el navegador)
│   ├── favicon.svg            # Icono de la pestana
│   ├── footer-logo-dark.svg   # Logo para modo oscuro
│   └── footer-logo-light.svg  # Logo para modo claro
│
├── src/                        # CODIGO FUENTE (aqui esta toda la logica)
│   │
│   ├── config/                 # Configuraciones
│   │   └── firebase.ts         # Configuracion de Firebase (login Google)
│   │
│   ├── contexts/               # Contexts de React (datos globales)
│   │   ├── AuthContext.tsx     # Maneja login/logout y usuario
│   │   └── ThemeContext.tsx    # Maneja tema oscuro/claro
│   │
│   ├── components/             # Componentes reutilizables
│   │   ├── Button.tsx          # Boton con estilos
│   │   ├── Card.tsx            # Tarjeta contenedora
│   │   ├── Input.tsx           # Campo de texto
│   │   ├── Layout.tsx          # Header, Footer, Boton Home
│   │   ├── Preloader.tsx       # Pantalla de carga
│   │   ├── SearchBar.tsx       # Barra de busqueda
│   │   └── index.ts           # Exportaciones centralizadas
│   │
│   ├── screens/                # Pantallas completas
│   │   ├── HomeScreen.tsx      # Menu principal + Como Usar + Contacto
│   │   ├── LoginScreen.tsx     # Login con Google + Verificacion 2FA
│   │   └── SearchScreen.tsx    # Busqueda y detalle de fallas
│   │
│   ├── data/                   # Datos estaticos
│   │   └── fallas.ts           # Lista de 36+ fallas y soluciones
│   │
│   ├── utils/                  # Funciones auxiliares
│   │   └── helpers.ts          # Hash, storage, debounce, etc.
│   │
│   ├── App.tsx                 # COMPONENTE PRINCIPAL
│   ├── main.tsx                # PUNTO DE ENTRADA
│   ├── index.css               # TODOS LOS ESTILOS
│   └── vite-env.d.ts          # Tipos de Vite (no editar)
│
├── index.html                  # Archivo HTML base
├── package.json               # Dependencias y scripts
├── tailwind.config.js         # Configuracion de Tailwind
├── tsconfig.json              # Configuracion de TypeScript
└── vite.config.ts            # Configuracion de Vite
```

---

## 3. FLUJO DE LA APLICACION

### Como funciona desde que el usuario abre la app?

```
1. El navegador carga index.html
   |
   v
2. index.html tiene: <script src="/src/main.tsx">
   |
   v
3. main.tsx ejecuta: createRoot().render(<App />)
   |
   v
4. App.tsx se renderiza:
   |
   +-- [Muestra Preloader por 2 segundos]
   |        |
   |        v
   +-- [Verifica si hay sesion guardada]
            |
            +-- Si NO hay usuario --> LoginScreen
            |        |
            |        v
            |        [Usuario hace clic en Acceder con Google]
            |        |
            |        v
            |        [Se abre popup de Google]
            |        |
            |        v
            |        [Usuario selecciona cuenta]
            |        |
            |        v
            |        [Firebase confirma identidad]
            |
            +-- Si hay usuario pero NO paso 2FA --> PasswordVerification
            |        |
            |        v
            |        [Usuario ingresa contrasena]
            |        |
            |        v
            |        [Si es correcta --> HomeScreen]
            |
            +-- Si hay usuario Y paso 2FA --> HomeScreen
                     |
                     v
                     [Menu principal: Buscar Fallas, Como Usar, Soporte]
                     |
                     v
                     [Usuario hace clic en "Buscar Fallas"]
                     |
                     v
                     SearchScreen
                     |
                     v
                     [Lista de 36+ fallas]
                     |
                     v
                     [Usuario hace clic en una falla]
                     |
                     v
                     FallaDetailScreen
                     |
                     v
                     [Lista de soluciones expandibles]
```

---

## 4. ARCHIVOS IMPORTANTES

### Archivos que probablemente quieras editar:

| Archivo | Para que sirve | Dificultad |
|---------|---------------|------------|
| `src/data/fallas.ts` | Agregar/editar fallas y soluciones | Facil |
| `src/index.css` | Cambiar colores y estilos | Intermedio |
| `public/footer-logo-dark.svg` | Logo del footer (modo oscuro) | Facil |
| `public/footer-logo-light.svg` | Logo del footer (modo claro) | Facil |

### Archivos que NO deberias modificar (a menos que sepas que haces):

| Archivo | Por que no tocarlo |
|---------|-------------------|
| `src/main.tsx` | Solo arranca la app. Lo necesario ya esta. |
| `src/vite-env.d.ts` | Tipos de TypeScript, no editar. |
| `vite.config.ts` | Configuracion de build, peligroso. |
| `tsconfig.json` | Configuracion de TypeScript. |
| `tailwind.config.js` | Configuracion de estilos. |

---

## 5. DONDE AGREGAR NUEVAS FUNCIONALIDADES

### Quiero agregar una nueva falla:
Ir a `src/data/fallas.ts` y agregar al array `fallasData`:

```typescript
{
  id: 'mi-nueva-falla',
  nombre: 'Mi nueva falla',
  soluciones: [
    { titulo: 'Solucion 1', descripcion: 'Descripcion...' }
  ]
},
```

### Quiero cambiar el color verde a otro color:
Ir a `src/index.css`, buscar `--color-primary` y cambiar los valores:

```css
--color-primary: #ff0000; /* Rojo en vez de verde */
```

### Quiero agregar un nuevo boton en el menu principal:
Ir a `src/screens/HomeScreen.tsx`, buscar `menuItems` y agregar:

```typescript
{
  id: 'mi-nueva-opcion',
  title: 'Mi nueva opcion',
  description: 'Descripcion corta',
  icon: <MiIcono size={24} />,
  action: () => onNavigate('miNuevaPantalla'),
  variant: 'secondary',
},
```

### Quiero cambiar el logo del footer:
1. Crea tu imagen en formato SVG o PNG
2. Guarda una version oscura como `public/footer-logo-dark.svg`
3. Guarda una version clara como `public/footer-logo-light.svg`
4. Si usas PNG, edita `src/components/Layout.tsx` y cambia `.svg` por `.png`

### Quiero agregar una nueva pantalla:
1. Crea el archivo `src/screens/MiPantalla.tsx`
2. Agrega la pantalla en `App.tsx`:
   - Agrega el tipo: `type Screen = ... | 'miPantalla'`
   - Agrega el case en el switch de `renderScreen()`
3. Agrega navegacion desde donde quieras

### Quiero cambiar la contrasena del 2FA:
1. Abre la consola del navegador (F12)
2. Ejecuta este codigo para obtener el hash:

```javascript
// Primero, define tu nueva contrasena
const nuevaPassword = 'TU_NUEVA_PASSWORD';

// Convierte a hash SHA-256
async function obtenerHash(texto) {
  const encoder = new TextEncoder();
  const data = encoder.encode(texto);
  const hashBuffer = await crypto.subtle.digest('SHA-256', data);
  const hashArray = Array.from(new Uint8Array(hashBuffer));
  return hashArray.map(b => b.toString(16).padStart(2, '0')).join('');
}

obtenerHash(nuevaPassword).then(hash => console.log(hash));
```

3. Copia el hash resultante
4. Ve a `src/contexts/AuthContext.tsx`
5. Busca `PASSWORD_HASH` y reemplaza el valor

---

## 6. GLOSARIO DE TERMINOS TECNICOS

| Termino | Definicion simple |
|---------|-------------------|
| **Componente** | Pieza de UI reutilizable. Como un bloque LEGO. Se crea una vez y se usa muchas veces. |
| **Props** | Datos que un componente padre pasa a un hijo. Como argumentos de una funcion. |
| **State (Estado)** | Datos que pueden cambiar y causan que la UI se actualice. |
| **Hook** | Funciones especiales de React que empiezan con `use`. Ej: useState, useEffect. |
| **Context** | "Bolsa global" para compartir datos entre componentes sin pasar props. |
| **useEffect** | Hook para ejecutar codigo cuando algo cambia o al montar componente. |
| **useState** | Hook para crear variables que pueden cambiar. Ej: `const [user, setUser] = useState(null)` |
| **useCallback** | Hook para memorizar funciones y evitar recalcularlas. |
| **useMemo** | Hook para memorizar valores calculados y evitar recalcularlos. |
| **Render** | Proceso de convertir componente en HTML visible. |
| **Re-render** | Cuando React vuelve a dibujar un componente (porque cambio el estado). |
| **DOM** | Estructura del HTML en memoria. React lo manipula para mostrar cambios. |
| **Virtual DOM** | Copia del DOM en memoria que React usa para calcular cambios eficientes. |
| **JSX** | Sintaxis que permite escribir HTML dentro de JavaScript. |
| **TypeScript** | JavaScript con tipos. Ayuda a detectar errores antes de ejecutar. |
| **Interface** | Define la forma de un objeto. Como un contrato que dice que campos debe tener. |
| **Type** | Similar a interface. Define que valores son validos. |
| **npm** | Manejador de paquetes de Node. Instala bibliotecas de otros. |
| **Vite** | Herramienta de desarrollo rapida. Ejecuta `npm run dev` para probar la app. |
| **Build** | Proceso de convertir todo el codigo en archivos optimizados para produccion. |
| **CSS Variables** | Variables en CSS que se definen con `--nombre` y se usan con `var(--nombre)`. |
| **Tailwind** | Framework CSS que usa clases como `p-4`, `bg-red-500`, etc. |
| **Firebase** | Plataforma de Google para autenticacion, base de datos, hosting. |
| **Auth** | Abreviacion de "autenticacion". Verificar identidad del usuario. |
| **2FA** | Two-Factor Authentication. Segundo factor de seguridad (contrasena). |
| **Session** | Datos guardados temporalmente mientras el navegador esta abierto. |
| **Local Storage** | Datos guardados permanentemente en el navegador. |
| **Hash** | Transformacion irreversible de texto. Se usa para contrasenas. |
| **SHA-256** | Algoritmo de hash seguro. Convierte cualquier texto en codigo de 64 caracteres. |
| **API** | Interfaz para comunicarse con otros programas. Firebase es una API. |
| **Provider** | Componente que "provee" datos a sus hijos. Se usa con Context. |
| **Callback** | Funcion que se pasa como argumento y se ejecuta despues. |
| **Event** | Accion del usuario: clic, escribir, mover mouse. |
| **Event Listener** | Codigo que escucha eventos y ejecuta una funcion cuando ocurren. |

---

## 7. COMANDOS UTILES

### Ejecutar en desarrollo:
```bash
npm run dev
```
Abre la app en el navegador (generalmente en http://localhost:5173)

### Construir para produccion:
```bash
npm run build
```
Crea carpeta `dist/` con archivos optimizados.

### Verificar errores de TypeScript:
```bash
npm run typecheck
```

### Ver errores de linting:
```bash
npm run lint
```

---

## 8. SOLUCION DE PROBLEMAS

### La app no carga (pantalla en blanco):
1. Abre la consola (F12) y mira los errores
2. Verifica que todas las importaciones son correctas
3. Ejecuta `npm install` por si falta alguna dependencia

### Error: "useAuth must be used within an AuthProvider":
Significa que intentaste usar `useAuth()` fuera de `<AuthProvider>`.
Ve a App.tsx y asegurate de que tu componente esta dentro del provider.

### Error: "useTheme must be used within a ThemeProvider":
Igual que arriba pero con el tema. Asegurate de estar dentro de `<ThemeProvider>`.

### El login con Google no funciona:
1. Verifica que Firebase este configurado correctamente en `src/config/firebase.ts`
2. Verifica que el dominio este autorizado en Firebase Console
3. Mira la consola para ver el error especifico

### Los estilos no se aplican:
1. Verifica que Tailwind CSS este funcionando
2. Verifica que `src/index.css` este importado en `main.tsx`
3. Recarga la pagina con Ctrl+Shift+R (forzar recarga)

---

## 9. INFORMACION DE CONTACTO

**Powered by Novacell Repairs**

**Developed by Lucas M. Tuillier**

WhatsApp: +54 9 11 2722-2169

---

## 10. LICENCIA

Este proyecto es privado y de uso exclusivo para Novacell Repairs.

---

*Documentacion generada automaticamente - Julio 2026*
