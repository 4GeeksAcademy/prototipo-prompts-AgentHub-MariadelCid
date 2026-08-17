# Especificación Técnica del Prototipo Frontend — AgentHub (`SPECS.md`)

## 1. Descripción del Producto
**AgentHub** es una plataforma SaaS B2B donde las empresas pueden alquilar agentes de inteligencia artificial: asistentes inteligentes preconfigurados que pueden equiparse con distintas *skills* (habilidades como navegar por la web, analizar documentos, gestionar calendarios, etc.) y desplegarse para tareas de negocio específicas.

El **panel de administración** es la interfaz central utilizada por el administrador de la plataforma para supervisar la actividad global, gestionar los usuarios y sus suscripciones, controlar el catálogo y estado de los agentes IA, administrar las *skills* disponibles, consultar el historial de contrataciones y revisar el log de errores de ejecución.

---

## 2. Stack Tecnológico y Restricciones
* **HTML5:** Estructura semántica sin preprocesadores.
* **CSS Framework:** Tailwind CSS integrado exclusivamente mediante CDN (`<script src="https://cdn.tailwindcss.com"></script>`). Se debe habilitar la configuración del modo oscuro mediante clase (`darkMode: 'class'`).
* **JavaScript:** JS Vanilla estricto sin dependencias externas, librerías ni frameworks (sin React, Vue, jQuery, etc.).
* **Arquitectura de Ejecución:** Prototipo meramente cliente/frontend (sin backend, datos *hardcodeados* en el DOM o JS estático).
* **Restricciones de Estilos:** Se deben utilizar las utilidades `dark:` de Tailwind CSS para garantizar que toda la interfaz sea 100% compatible con el modo claro y oscuro.

---

## 3. Especificaciones Detalladas por Sección

### 3.1. Dashboard
1. **Cuatro tarjetas de métricas en cuadrícula responsive 2×2 (o 4×1 en pantallas anchas):** Cada tarjeta debe incluir un icono representativo, una etiqueta descriptiva y un valor *hardcodeado* (Ingresos totales este mes, Pérdida total por descuentos/cupones, Número de agentes activos en todos los clientes, y Número de agentes marcados como fallando).
2. **Estilo visual diferenciado y acentos:** Las tarjetas deben utilizar colores de acento distintos según la métrica (verde para ingresos, amarillo/naranja para pérdidas por cupones, azul para agentes activos, rojo para fallando), sombras sutiles (`shadow-sm` / `shadow-md`) y bordes adaptados al modo claro/oscuro.
3. **Marcador de posición de gráfico:** Debajo de las tarjetas, se debe incluir un `div` de ancho completo con borde discontinuo (`border-dashed`), fondo sutil y una etiqueta centrada que indique *"Área reservada para Gráfico de Actividad Semanal"*.

### 3.2. Gestión de Usuarios
1. **Tabla de usuarios registrados:** Tabla responsive que enumera a todos los usuarios mostrando nombre (con avatar de iniciales), email, plan de suscripción actual (ej. Basic, Pro, Enterprise) y estado de la cuenta representado mediante badges.
2. **Dropdown de acciones por fila:** Cada fila debe incluir un botón con tres puntos verticales (`⋮`) que activa un menú desplegable con al menos las opciones *"Ver detalle"* y *"Eliminar"*.
3. **Modal overlay de detalle de usuario:** Al hacer clic en *"Ver detalle"*, se debe abrir un modal centralizado que muestra la ficha completa del registro del usuario (ID, fecha de alta, método de pago, límites del plan). El modal se cierra al pulsar un botón explícito de cierre (`✕`) o al hacer clic en el backdrop semitransparente.

### 3.3. Gestión de Agentes
1. **Listado interactivo de agentes:** Vista de tabla o tarjetas que enumera todos los agentes registrados indicando el nombre del agente, el propietario/cliente asignado y el estado actual (*Activo*, *Inactivo*, *Fallando*) con badges con código de color.
2. **Lista de skills colapsada e interactiva:** Las *skills* asociadas a cada agente están ocultas por defecto. Un control expandible (botón o flecha accordion) permite revelar la lista de *skills* con una transición suave (`transition-all`).
3. **Dropdown de acciones y modal de configuración:** Cada agente incluye un dropdown con las opciones *"Configurar"* y *"Eliminar"*. Al seleccionar *"Configurar"*, se abre un modal que muestra y permite visualizar/editar el *System Prompt* del agente dentro de un campo de texto multi-línea (`textarea`).

### 3.4. Skills (Catálogo de Habilidades)
1. **Bloque explicativo contextual:** La sección debe comenzar con una tarjeta informativa o cabecera que defina claramente qué es una *"skill"* en el contexto de AgentHub (capacidades ejecutables que se equipan a los agentes como navegación web, lectura de PDFs o gestión de calendarios).
2. **Grid/Tabla de catálogo de skills:** Listado de todas las *skills* disponibles en la plataforma, mostrando para cada una su nombre, una breve descripción funcional y un indicador numérico de cuántos agentes la tienen habilitada actualmente.
3. **Dropdown de acciones en cada skill:** Cada entrada incluye un botón `⋮` con dropdown de acciones (*"Ver detalle"* y *"Eliminar"*). La opción *"Ver detalle"* abre un modal con la información técnica y permisos de la *skill*.

### 3.5. Contrataciones de Agentes
1. **Tabla de contratos de alquiler:** Registro histórico de contrataciones que muestra el nombre del cliente, el agente alquilado, las *skills* contratadas, el rango de fechas del contrato (inicio/fin) y el importe total pagado.
2. **Dropdown de acciones por contratación:** Cada fila cuenta con un menú flotante de acciones con la opción *"Ver detalle"*.
3. **Modal de desglose completo de contrato:** Al hacer clic en *"Ver detalle"*, un modal despliega el desglose financiero e individualizado del contrato, mostrando el precio base de alquiler del agente más la lista desglosada de *skills* adicionales contratadas con sus respectivos precios.

### 3.6. Log de Errores
1. **Tabla de registro de ejecuciones fallidas:** Muestra una lista cronológica con timestamp (fecha y hora), nombre del agente afectado, tipo de error y una breve descripción del problema.
2. **Categorización visual por gravedad:** Los errores se deben identificar visualmente mediante badges con código de color según el tipo/gravedad (*Crítico* en rojo, *Advertencia* en amarillo, *Timeout* en naranja).
3. **Dropdown de acciones y marcado de resolución:** Cada registro de error dispone de un dropdown con las opciones *"Ver detalle"* (que abre un modal con la traza de código/stack trace completa del error) y *"Marcar como resuelto"* (que actualiza visualmente la fila indicando el estado resuelto).

---

## 4. Inventario de Componentes UI Reutilizables

* **Sidebar (Navegación Lateral Persistente):** Menú vertical con estado activo para la sección seleccionada y enlaces a las 6 vistas.
* **Top Header con Toggle de Modo Oscuro:** Barra superior con interruptor (switch/botón) para cambiar entre tema claro y oscuro mediante la clase `dark` en el tag `<html>`.
* **Tarjeta de Métrica (Metric Card):** Contenedor de datos con icono, valor numérico destacado, etiqueta y color de acento.
* **Tabla de Datos (Data Table):** Formato homogéneo para listados con cabeceras estilizadas, estados *hover* en filas y soporte para modo oscuro.
* **Dropdown de Acciones:** Menú flotante activado por un botón `⋮` posicionado en la celda de acciones de las tablas/tarjetas.
* **Modal Overlay:** Ventana emergente con backdrop semitransparente, cabecera con botón de cierre `✕`, cuerpo de contenido y pie con acciones.
* **Badge (Etiqueta de Estado/Gravedad):** Elemento compacto con esquinas redondeadas y colores de contraste (`bg-*-100 text-*-800 dark:bg-*-950 dark:text-*-300`) para estados (*Activo*, *Inactivo*, *Fallando*, *Resuelto*, etc.).
* **Lista de Skills Colapsable (Accordion):** Componente desplegable con animación para ocultar/mostrar elementos anidados.

---

## 5. Criterios de Aceptación

1. **Navegación entre Secciones:** Hacer clic en cualquier elemento de la navegación lateral cambia la vista visible a la sección correspondiente sin recargar la página.
2. **Modo Claro / Oscuro:** Al accionar el toggle de modo oscuro en la barra superior, la interfaz alterna globalmente entre los modos claro y oscuro afectando a fondos, textos, bordes, modales y tarjetas mediante utilidades `dark:`.
3. **Comportamiento del Dropdown de Acciones:** Al hacer clic en el botón `⋮` de cualquier fila/tarjeta, se despliega el menú de acciones correspondiente y se cierra automáticamente al hacer clic fuera de él.
4. **Comportamiento del Modal:** Al hacer clic en *"Ver detalle"* (o *"Configurar"* en agentes), el modal correspondiente aparece sobre la interfaz. Al hacer clic en el botón de cierre (`✕`) o fuera del contenedor del modal (en el backdrop), el modal se oculta completamente.
5. **Comportamiento del Acordeón/Colapsable:** En la sección de Gestión de Agentes, al hacer clic en el control desplegable de *skills*, la lista de habilidades del agente se expande o colapsa mediante una transición visual fluida.
6. **Marcado de Errores Resueltos:** En la sección de Log de Errores, al hacer clic en *"Marcar como resuelto"*, el estado visual de esa fila se actualiza reflejando la resolución del error.
7. **Diseño Responsive:** La interfaz se adapta correctamente a diferentes tamaños de pantalla (móvil, tablet y desktop), manteniendo la usabilidad de tablas, modales y tarjetas.

---
## 6. Guía de Estilos y Paleta de Colores (Tailwind CSS)

### 6.1. Colores de la Interfaz Base
* **Sidebar (Barra Lateral Persistente):**
  * Fondo: `bg-[#1e293b]` (Slate 800) o `bg-[#0f172a]` (Slate 900) para un tono azul oscuro/grisáceo.
  * Texto e Iconos de enlaces: `text-slate-400` (Inactivos) y `text-white` (Activo / Hover).
  * Enlace Activo: `bg-slate-800/60 text-white` con indicador/borde lateral activo en azul.
  * Botón principal `+ Deploy Agent`: `bg-indigo-600 hover:bg-indigo-700 text-white`.
* **Fondo Principal de la Aplicación:**
  * Modo Claro: `bg-[#f8fafc]` (Slate 50) o `bg-[#f1f5f9]` (Slate 100).
  * Modo Oscuro: `bg-[#0f172a]` (Slate 900).
* **Tarjetas y Contenedores:**
  * Modo Claro: `bg-white border border-slate-200/80 shadow-sm`.
  * Modo Oscuro: `bg-slate-800 border border-slate-700 shadow-sm`.

### 6.2. Tipografía y Colores de Texto
* **Títulos Principales (`Dashboard`, etc.):** `text-slate-800 dark:text-slate-100 font-bold`.
* **Subtítulos y Fechas:** `text-slate-500 dark:text-slate-400 font-normal`.
* **Etiquetas de Tarjetas (en Mayúsculas):** `text-xs font-semibold tracking-wider text-slate-500 dark:text-slate-400`.
* **Valores Métricos Destacados:** `text-2xl font-bold text-slate-900 dark:text-white`.

### 6.3. Indicadores de Tendencia y Badges de Estado
* **Tendencia Positiva / Incremento / Éxito:** `text-emerald-600 dark:text-emerald-400` (ej. `↗12%`, `↗24`).
* **Tendencia Negativa / Reducción / Costo:** `text-rose-600 dark:text-rose-400` (ej. `↘5%`).
* **Badges de Estado:**
  * **Activo / Exitoso:** `bg-emerald-100 text-emerald-800 dark:bg-emerald-950/60 dark:text-emerald-300`.
  * **Inactivo / Neutro:** `bg-slate-100 text-slate-700 dark:bg-slate-800 dark:text-slate-300`.
  * **Fallando / Crítico:** `bg-rose-100 text-rose-800 dark:bg-rose-950/60 dark:text-rose-300`.
  * **Advertencia / Timeout:** `bg-amber-100 text-amber-800 dark:bg-amber-950/60 dark:text-amber-300`.

### 6.4. Área de Gráficos (Placeholder)
* Contenedor interno: `bg-slate-50/80 dark:bg-slate-900/50 border border-slate-200 dark:border-slate-700/60 rounded-lg`.
* Texto e Icono del Placeholder: `text-slate-400 dark:text-slate-500`.