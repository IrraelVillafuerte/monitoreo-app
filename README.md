# 📡 Sistema de Monitoreo de Sensores en Tiempo Real

> **Asignatura:** Aplicaciones Telemáticas Basadas en la Web  
> **Institución:** Universidad Técnica Estatal de Quevedo (UTEQ)  
> **Escenario:** Campus "La María"  
> **Tecnologías:** React + Vite, Firebase Realtime Database, React Router DOM v6  

---

## 📋 Descripción del Proyecto

Este proyecto consiste en una aplicación web interactiva desarrollada para la visualización y monitoreo en tiempo real de variables ambientales (temperatura, humedad y presión) provenientes de redes de sensores desplegadas en el Campus "La María" de la UTEQ.

La plataforma utiliza la arquitectura de actualización en tiempo real de **Firebase Realtime Database** combinada con **React** y **Vite**, garantizando que las métricas (Tarjetas KPI y listas) se actualicen de manera reactiva e instantánea sin necesidad de recargar la página web.

---

## 🚀 Características Principales

- ⚡ **Sincronización en Tiempo Real:** Uso de WebSockets a través del SDK de Firebase (`onValue`) para reflejar los cambios en tiempo real.
- 📊 **Tarjetas KPI Dinámicas:** Visualización rápida de Temperatura (°C), Humedad (%) y Presión (hPa).
- 🔀 **Enrutamiento Dinámico:** Navegación por identificador de sensor (`/sensor/:sensorId`) utilizando `react-router-dom` v6.
- 📍 **Directorio de Ubicaciones:** Navegación centralizada para explorar sensores instalados en distintas zonas del campus.
- 🎨 **Diseño Adaptativo (Responsive):** Malla CSS Grid autorregulable mediante `auto-fit` para dispositivos móviles, tablets y escritorio.
- 🔒 **Manejo Seguro de Variables:** Configuración aislada de las credenciales de Firebase mediante variables de entorno `.env`.

---

## 🛠️ Tecnologías Utilizadas

| Tecnología | Versión / Función |
| :--- | :--- |
| **React** | Biblioteca principal para la interfaz de usuario basada en componentes |
| **Vite** | Tooling de desarrollo rápido y empaquetador |
| **Firebase RTDB** | Base de datos NoSQL en tiempo real para almacenamiento e ingestión |
| **React Router** | Enrutamiento dinámico SPA (Single Page Application) |
| **CSS3 (Grid/Flex)** | Estilizado moderno y maquetación responsive |

---

## 📁 Estructura del Proyecto

```text
monitoreo-app/
├── node_modules/
├── public/
├── src/
│   ├── components/
│   │   ├── Navbar.jsx         # Barra de navegación principal
│   │   └── SensorCard.jsx     # Componente reutilizable para tarjetas KPI
│   ├── hooks/
│   │   └── useSensorData.js   # Custom Hook para escucha en tiempo real de RTDB
│   ├── pages/
│   │   ├── Dashboard.jsx      # Vista principal de monitoreo e indicadores
│   │   └── Ubicaciones.jsx    # Vista de directorio/lista de sensores
│   ├── services/
│   │   └── firebase.js        # Configuración e inicialización del SDK de Firebase
│   ├── App.jsx                # Configuración de rutas principales
│   ├── main.jsx               # Punto de entrada de la aplicación
│   └── index.css              # Estilos globales y grid dinámico
├── .env                       # Variables de entorno (Firebase Config)
├── .gitignore                 # Archivos e ignorados por Git
├── index.html                 # Plantilla HTML base
├── package.json               # Dependencias y scripts del proyecto
├── vite.config.js             # Configuración de Vite
└── README.md                  # Documentación del proyecto
```

---

## 🗄️ Estructura de Datos en Firebase (JSON Schema)

La base de datos NoSQL en Firebase Realtime Database está estructurada de forma modular para optimizar las lecturas:

```json
{
  "valorActual": {
    "sensor_001": {
      "temperatura": 24.5,
      "humedad": 60,
      "presion": 1012,
      "timestamp": "2026-08-13T09:00:00Z"
    },
    "sensor_002": {
      "temperatura": 28.1,
      "humedad": 52,
      "presion": 1009,
      "timestamp": "2026-08-13T09:00:00Z"
    }
  },
  "valoresHistoricos": {
    "sensor_001": {
      "rec_001": { "temperatura": 24.2, "humedad": 61, "presion": 1012, "timestamp": "2026-08-13T08:55:00Z" }
    }
  },
  "ubicacionesSensores": {
    "sensor_001": { "nombre": "Sensor Campus A", "zona": "Laboratorio" },
    "sensor_002": { "nombre": "Sensor Campus B", "zona": "Invernadero" }
  }
}
```

> **Nota sobre la importación:** Si el archivo comprimido exportado originalmente posee la extensión `.jsonux`, debe cambiarse o descomprimirse como `.zip` para extraer el archivo `.json` de texto plano antes de subirlo desde la consola de Firebase.

---

## ⚙️ Guía de Instalación y Configuración Paso a Paso

### 1. Requisitos Previos
Asegúrate de tener instalado:
- **Node.js** (Versión 18.x o superior)
- **npm** (Incluido con Node.js)
- Cuenta activa en **Google Firebase**

### 2. Clonar o Crear el Proyecto
Desde la terminal (CMD o PowerShell):
```bash
# Crear proyecto con Vite y React
npm create vite@latest monitoreo-app -- --template react

# Entrar al directorio
cd monitoreo-app
```

### 3. Instalación de Dependencias
```bash
# Instalar dependencias base
npm install

# Instalar Firebase y React Router DOM
npm install firebase react-router-dom
```

### 4. Crear la Estructura de Directorios
```bash
mkdir src/services src/hooks src/components src/pages
```

### 5. Configurar Variables de Entorno
Crea un archivo denominado `.env` en la raíz del proyecto con la configuración provista por la consola de Firebase:

```env
VITE_FIREBASE_API_KEY=tu_api_key_aqui
VITE_FIREBASE_AUTH_DOMAIN=tu_proyecto.firebaseapp.com
VITE_FIREBASE_DATABASE_URL=https://tu_proyecto-default-rtdb.firebaseio.com
VITE_FIREBASE_PROJECT_ID=tu_proyecto
```

---

## 💻 Resumen del Código Implementado

### `src/services/firebase.js`
Inicializa la conexión con Firebase utilizando las variables del entorno `.env`:
```javascript
import { initializeApp } from "firebase/app";
import { getDatabase } from "firebase/database";

const firebaseConfig = {
  apiKey: import.meta.env.VITE_FIREBASE_API_KEY,
  authDomain: import.meta.env.VITE_FIREBASE_AUTH_DOMAIN,
  databaseURL: import.meta.env.VITE_FIREBASE_DATABASE_URL,
  projectId: import.meta.env.VITE_FIREBASE_PROJECT_ID,
};

const app = initializeApp(firebaseConfig);
export const db = getDatabase(app);
```

### `src/hooks/useSensorData.js`
Custom Hook que suscribe al componente a los cambios en vivo del nodo `/valorActual/{sensorId}` y limpia el listener al desmontar:
```javascript
import { useState, useEffect } from 'react';
import { ref, onValue } from 'firebase/database';
import { db } from '../services/firebase';

export const useSensorData = (sensorId) => {
  const [data, setData] = useState(null);

  useEffect(() => {
    if (!sensorId) return;
    const sensorRef = ref(db, `valorActual/${sensorId}`);
    const unsubscribe = onValue(sensorRef, (snapshot) => {
      setData(snapshot.val());
    });
    return () => unsubscribe(); // Cleanup vital
  }, [sensorId]);

  return data;
};
```

---

## 🚀 Ejecución del Proyecto en Desarrollo

Para iniciar el servidor local de desarrollo, ejecuta:

```bash
npm run dev
```

La consola mostrará una URL local, generalmente:
```text
  VITE v5.x.x  ready in 300 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
```

Abre **`http://localhost:5173/`** en tu navegador para interactuar con la aplicación.

---

## 🧪 Validación del Flujo en Tiempo Real

1. Abre la aplicación en el navegador (`http://localhost:5173/`).
2. Abre simultáneamente la **Consola de Firebase** en la pestaña **Realtime Database > Datos**.
3. Navega hacia `valorActual/sensor_001/temperatura` en Firebase y modifica su valor.
4. Observa cómo la tarjeta KPI en la pantalla de React **se actualiza instantáneamente en milisegundos** sin requerir recarga de la página.

---

## 👥 Créditos e Institución

- **Universidad:** Universidad Técnica Estatal de Quevedo (UTEQ)
- **Facultad / Carrera:** Ingeniería en Telemática / Ciencias de la Ingeniería
- **Asignatura:** Aplicaciones Telemáticas Basadas en la Web
- **Escenario:** Campus "La María"
# React + Vite
<img width="1190" height="937" alt="image" src="https://github.com/user-attachments/assets/b39c5957-0453-48df-9048-0a47b27a491e" />

This template provides a minimal setup to get React working in Vite with HMR and some Oxlint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Oxc](https://oxc.rs)
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/)

## React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

## Expanding the Oxlint configuration

If you are developing a production application, we recommend using TypeScript with type-aware lint rules enabled. Check out the [TS template](https://github.com/vitejs/vite/tree/main/packages/create-vite/template-react-ts) for information on how to integrate TypeScript and Oxlint's TypeScript related rules in your project.
