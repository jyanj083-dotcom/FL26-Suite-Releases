# FL26 Suite: Manual completo de usuario

| | |
|---|---|
| **Aplicación** | FL26 Suite / FL26 Suite Desktop |
| **Plataforma** | Windows (distribución compilada, carpeta portable) |
| **Idiomas** | Español · English · Français · Português |
| **Módulos** | Escudos · Plantillas · Batch · Kits · Fútbol Manager |

Las instrucciones describen las funciones conocidas de la versión de desarrollo validada y la familia beta. Los nombres o posiciones de algunos botones pueden variar entre versiones.

> [!WARNING]
> Antes de modificar el juego o una receta, haz un respaldo.

## Contenido

1. [¿Para qué sirve FL26 Suite?](#1-para-qué-sirve-fl26-suite)
2. [Instalación e inicio](#2-instalación-e-inicio)
3. [Flujo resumido de los cinco módulos](#3-flujo-resumido-de-los-cinco-módulos)
4. [Fútbol Manager: descargar logos y plantillas](#4-fútbol-manager-descargar-logos-y-plantillas)
5. [Escudos: vincular PNG con los clubes correctos](#5-escudos-vincular-png-con-los-clubes-correctos)
6. [Plantillas: preparar jugadores y tácticas de una liga](#6-plantillas-preparar-jugadores-y-tácticas-de-una-liga)
7. [Batch: aplicar múltiples ligas automáticamente](#7-batch-aplicar-múltiples-ligas-automáticamente)
8. [Kits: generar uniformes](#8-kits-generar-uniformes)
9. [Ejemplo completo: LigaPro Serie A de Ecuador](#9-ejemplo-completo-ligapro-serie-a-de-ecuador)
10. [Errores frecuentes y recomendaciones](#10-errores-frecuentes-y-recomendaciones)
11. [Seguridad, compatibilidad y privacidad](#11-seguridad-compatibilidad-y-privacidad)

---

## 1. ¿Para qué sirve FL26 Suite?

Es una herramienta de apoyo al modding de **SP Football Life 2026**, diseñada para que puedas obtener datos reales de fútbol y preparar escudos, jugadores, formaciones, tácticas y kits para ligas y clubes añadidos a un mundo personalizado. Reúne en una sola ventana tareas que antes se realizaban por separado.

> [!IMPORTANT]
> No confundas estos tres elementos:
>
> - **Mundo del juego:** carpeta con archivos BIN que FL26 Mod Studio construye, por ejemplo `_FL26Pack/common/etc/pesdb/`.
> - **Receta JSON:** archivo de configuración del mundo para FL26 Mod Studio. FL26 Suite Plantillas y Batch modifican una copia de esta receta; no actualizan automáticamente el mundo ya construido.
> - **TXT/PNG:** descargas obtenidas por Fútbol Manager. Son las entradas para Plantillas, Batch y Escudos, no archivos BIN del juego.

## 2. Instalación e inicio

1. Obtén la versión oficial desde **GitHub Releases** del proyecto público `FL26-Suite-Releases`.
2. Extrae el ZIP completo. La distribución debe contener `FL26 Suite.exe` y `_internal` (además de otros recursos cuando corresponda); no muevas solo el EXE.
3. Ejecuta `FL26 Suite.exe`. En el EXE compilado no hace falta instalar Python.
4. Selecciona tu idioma arriba. En **Configuración**, revisa la carpeta del juego, el espacio de trabajo, las rutas Sider y de los datos; evita utilizar como espacio de trabajo la raíz de instalación del juego.
5. Para consultar actualizaciones utiliza **Actualizar**. El programa valida la autenticidad de las versiones distribuidas mediante manifiesto y firma; no instales ZIP de procedencia dudosa.

> [!NOTE]
> En desarrollo (no para usuarios de la versión pública), el programa se inicia desde la raíz del código con `tools/launcher.py` usando el entorno virtual local.

## 3. Flujo resumido de los cinco módulos

| Módulo | Entrada | ¿Qué hace? | Resultado |
|---|---|---|---|
| **Fútbol Manager** | País, temporada, competición, clubes y fuente | Busca información en internet; obtiene logos y datos de jugadores | Carpetas PNG y archivos TXT |
| **Escudos** | PNG, país/liga y mundo BIN | Vincula logos con nombres/IDs reales de clubes | Imágenes identificadas para el juego |
| **Plantillas** | Un TXT de liga y receta JSON | Genera las ediciones de 30 jugadores por club y asigna formación/táctica | Una receta JSON nueva o actualizada |
| **Batch** | Varios TXT y receta JSON | Ejecuta el trabajo de Plantillas para varias ligas y registra los resultados | Receta JSON, respaldos e informes |
| **Kits** | Mundo, escudos, imágenes de uniformes y recursos gráficos | Genera, adapta y configura uniformes por club | Archivos y mapas de kits |

> [!NOTE]
> **Regla de clubes:** Escudos, Plantillas, Batch y Kits están orientados a los **clubes nuevos** del mundo. Fútbol Manager deja elegir también clubes originales, porque solo descarga información.

## 4. Fútbol Manager: descargar logos y plantillas

**¿Para qué sirve?** Busca datos por país y temporada; permite seleccionar competiciones y clubes; descarga logos PNG y plantillas TXT. Es el punto de partida recomendado antes de utilizar Escudos y Plantillas.

**Procedimiento**

1. Abre **Fútbol Manager**.
2. Introduce un país (por ejemplo `Ecuador`) y una temporada (`2026` o `2026/27` según la competición).
3. Pulsa **Buscar competiciones**, revisa los resultados y selecciona las ligas deseadas.
4. En la pestaña **Plantillas/TXT**, elige las competiciones y clubes cuyos jugadores quieras descargar. Puedes seleccionar Todos/Ninguno, cargar clubes y revisar nombres.
5. En **Logos/PNG**, elige competiciones, clubes y fuente(s) preferidas. Dependiendo del modo y la fuente puedes elegir **Normal** o **mejora con IA** en cada descarga, incluso si la anterior fue Normal.
6. Ejecuta la descarga y revisa el registro, los clubes omitidos y los archivos realmente generados.

**Carpetas de salida correctas**

```text
FootballManager/
├── Logos/
│   └── Ecuador/
│       ├── Liga Pro Serie A/
│       │   ├── Libertad FC.png
│       │   └── Emelec.png
│       └── Liga Pro Serie B/
│           └── Club.png
└── Plantillas/
    └── Ecuador/
        ├── Liga Pro Serie A.txt
        └── Liga Pro Serie B.txt
```

La versión mejorada **no** crea `2026` entre país y liga. El año elegido sigue siendo relevante para la búsqueda. Las carpetas antiguas `2026` pueden seguir existiendo porque el cambio no las borró. Descargar otra temporada con el mismo nombre de liga podría reemplazar su TXT: guarda copias por tu cuenta si necesitas varias temporadas simultáneas.

> [!IMPORTANT]
> Descargar un TXT no cambia los jugadores de Football Life hasta importarlo mediante Plantillas/Batch, recargar la receta y reconstruir con FL26 Mod Studio.

## 5. Escudos: vincular PNG con los clubes correctos

**¿Para qué sirve?** Toma imágenes con nombres de clubes, las empareja con los IDs de club del mundo de Football Life y prepara los tamaños y nombres de archivo usados por el juego.

**Procedimiento**

1. Abre **Escudos** y despliega el panel **Carpetas**.
2. En **Logos (raíz)** elige la carpeta `FootballManager/Logos` u otra que respete `País/Liga/Club.png`.
3. Selecciona **Mundo**, la carpeta que contiene `common/etc/pesdb` de tu mundo; pulsa **Cargar ligas** si corresponde.
4. En el panel **Clubes**, selecciona la liga del mundo. Pulsa **Usar los clubes de esta liga**: la versión corregida sincroniza automáticamente país, liga y carpeta PNG.
5. Comprueba que la carpeta y la liga identificada sean correctas. La detección ya distingue mejor LigaPro Serie A, LigaPro Serie B, ligas genéricas Liga 2, países distintos, tildes y nombres con patrocinadores. Cuando no hay coincidencia única, requiere revisión o elección manual.
6. Utiliza **Auto-emparejar por nombre** y revisa la columna de confianza (**Score**). Corrige manualmente filas dudosas. Emparejar por orden solo es seguro si los órdenes de ambas listas coinciden realmente.
7. Puedes **Guardar mapeo** y **Cargar mapeo** para reutilizar las asociaciones. Comprueba **Destino**, realiza tu respaldo y finalmente pulsa **Instalar**.

El módulo puede crear tres tamaños por club según la salida requerida: **128×128**, **256×256** y **512×512**, con nombres basados en el ID.

> [!WARNING]
> No pulses **Instalar** si está seleccionada una liga incorrecta: se podrían asignar escudos a clubes equivocados.

## 6. Plantillas: preparar jugadores y tácticas de una liga

**¿Para qué sirve?** Importa los jugadores de un TXT, los relaciona con clubes nuevos de una receta JSON y genera las entradas que necesita FL26 Mod Studio. No edita el BIN del juego directamente.

**Procedimiento**

1. Abre **Plantillas**.
2. Selecciona una receta JSON (mejor una copia de prueba) y el TXT de una liga.
3. Elige la liga de destino y verifica la correspondencia de país y clubes. Los clubes originales del juego deben quedar fuera del proceso de edición.
4. Configura una escala de valoración: manual o **Auto-escala**, con región/país/división cuando proceda. Las GRL calculadas son estimaciones de simulación, no necesariamente valoraciones oficiales de EA FC/PES.
5. Ejecuta **Preview/Vista previa**. Revisa clubes, cantidad de jugadores, valoraciones, posiciones y formación propuesta.
6. Cuando un club tenga menos de 30 jugadores reales, se completa con jugadores ficticios de características coherentes. La distribución se ajusta a la formación final elegida por el sistema táctico, no solo a una preliminar. Se conserva a los jugadores reales dentro de las reglas aplicables; si hay más de 30 se selecciona una plantilla ajustada.
7. Selecciona clubes y pulsa **Aplicar**. Prefiere **Guardar como nuevo** para producir una receta nueva y conservar la original.
8. Abre **FL26 Mod Studio**, vuelve a cargar expresamente la receta JSON nueva y, después de revisar sus clubes/jugadores, utiliza su función **Construir/Build** si deseas llevar cambios al mundo del juego.

> [!WARNING]
> Nunca des por hecho que FL26 Mod Studio recarga automáticamente el JSON anterior. Debes seleccionar el archivo resultante. Un mundo ya construido seguirá usando datos anteriores hasta reconstruirse.

## 7. Batch: aplicar múltiples ligas automáticamente

**¿Para qué sirve?** Es Plantillas en lote. Trabaja sobre varios TXT en una sola operación, con opciones de formación automática, informes, respaldo y cancelación.

**Procedimiento**

1. Selecciona **Carpeta de plantillas**, que puede contener subcarpetas de país, como `Plantillas/Ecuador/`.
2. Selecciona la receta JSON y la carpeta de respaldos/reportes.
3. Pulsa **Recargar** para detectar competiciones y revisar las ligas de destino. La carpeta `Ecuador` ayuda a distinguir ligas aunque el TXT no incluya `(Ecuador)` en el nombre.
4. Marca los TXT que deseas procesar y revisa las correspondencias. Evita procesar dos archivos hacia la misma liga simultáneamente.
5. Configura **Auto-formación**, respaldo y tratamiento de errores. El algoritmo utiliza formaciones donantes y distribuciones adecuadas para mantener plantillas de 30 jugadores por club con valoraciones ajustadas.
6. Pulsa **Procesar seleccionadas**, confirma y revisa las estadísticas, errores y reportes. Si vas a modificar un mundo real, trabaja primero con una copia de la receta, no sobre el original.
7. Carga el JSON resultante en FL26 Mod Studio y reconstruye el mundo solo después de validar.

> [!NOTE]
> **Prueba real reportada:** en una liga de Ecuador se procesaron 12 clubes nuevos y 360 jugadores (30 por club); las formaciones y tácticas quedaron correctas dentro de Football Life. La prueba no garantiza automáticamente todas las ligas del mundo: revisa nombres y plantillas nuevas.

## 8. Kits: generar uniformes

**¿Para qué sirve?** Convierte, asigna y genera equipaciones para clubes nuevos, integrando logos y recursos gráficos.

**Procedimiento**

1. Abre **Kits**. Pulsa **Seleccionar mundo** y elige la carpeta del mundo construido, por ejemplo `.../SiderAddons/livecpk/_FL26Pack`. Debe contener las tablas BIN necesarias; el programa rechaza carpetas incompletas.
2. Pulsa **Recargar ligas** cuando corresponda. Revisa que se muestren únicamente los clubes nuevos.
3. Elige una carpeta de imágenes de kits y comprueba **Recursos HD**, si utilizas conversión.
4. Selecciona el tipo: **PES listo** (copiar), **FM26** (convertir) o **Auto** (detección automática).
5. Selecciona variantes: `p1` local, `p2` visitante, `p3` tercero y `g1` portero.
6. Ajusta cuello, posición/tamaño del escudo, nombre y opciones automáticas. Revisa la previsualización.
7. Haz respaldo del destino/kit-server y genera primero un club. Comprueba el resultado antes de pulsar **Generar todo** para muchos equipos.

La aplicación puede recordar la última ruta válida del mundo. No conviene sobrescribir kits existentes sin revisar explícitamente la opción de reemplazo.

## 9. Ejemplo completo: LigaPro Serie A de Ecuador

1. Descarga con **Fútbol Manager** logos y TXT de `Ecuador / LigaPro Serie A`.
2. En **Escudos**, selecciona la carpeta raíz `Logos`, luego la liga del mundo y pulsa **Usar los clubes de esta liga**. Verifica las imágenes, empareja PNG con IDs y, tras respaldo, instala los escudos.
3. En **Plantillas**, carga `Plantillas/Ecuador/Liga Pro Serie A.txt` y una copia de `_FL26Pack.json`; revisa **Preview** y aplica solo a los clubes nuevos. O usa **Batch** si tienes más ligas.
4. Confirma en la receta resultante 30 jugadores por club, formaciones y tácticas seleccionadas.
5. Abre **FL26 Mod Studio**, elige explícitamente la nueva receta, construye/actualiza el mundo `_FL26Pack` y verifica sus datos en el juego. Para ciertas modificaciones puede ser necesaria una Liga Máster nueva.
6. En **Kits**, selecciona el mundo, configura `p1`/`p2`/`p3`/`g1` y genera uniformes para esos clubes.

## 10. Errores frecuentes y recomendaciones

| Problema | Qué revisar |
|---|---|
| No aparece una liga en Escudos | Carpeta raíz `Logos/País/Liga`, reescaneo, mundo BIN, filtros y nombre de liga |
| La carpeta PNG es de otra competición | Detén la instalación; vuelve a seleccionar liga y comprueba sincronización; nunca confíes solo en el nombre genérico «Liga 2» |
| Fútbol Manager guardó un TXT en otra carpeta | Comprueba la salida personalizada y busca `Plantillas/País/Liga.txt`; carpetas antiguas `2026` pueden permanecer |
| Plantillas o Batch no modifican el juego | Actualizan una receta JSON; hay que recargarla y reconstruir en FL26 Mod Studio |
| Algunos jugadores no tienen GRL oficial | La escala OVR/GRL es estimativa; no confundirla con una ficha oficial |
| Kits no encuentra un mundo | Elegir carpeta de mundo con `common/etc/pesdb` válido; comprobar archivos BIN |
| La IA de logos no se ofrece | Iniciar una operación nueva de logos; debería permitir Normal/IA cada vez; revisar fuentes, configuración y registro |
| La actualización no abre | Conservar respaldo, revisar log de instalación y versión; no borrar manualmente el original antes de comprobar la nueva |

## 11. Seguridad, compatibilidad y privacidad

- Respaldar recetas JSON, archivos de Sider, kits y logos antes de modificar.
- No compartir el mundo original, bases BIN o archivos de terceros sin autorización; el público solo necesita su distribución FL26 Suite y sus propios recursos/instalación del juego.
- El repositorio público `FL26-Suite-Releases` proporciona el paquete compilado, no el código fuente privado. El EXE distribuible incluye `_internal`; ambos son necesarios.
- Las opciones de IA, OCR, navegador y conversión HD dependen de sus herramientas y recursos instalados; revisar mensajes de error.
- La versión de desarrollo puede contener nuevas funciones que aún no están publicadas en GitHub. Comprobar siempre la versión visible y las notas de lanzamiento.

---

## Resumen

**Fútbol Manager** obtiene TXT/PNG; **Escudos** asigna logos a IDs; **Plantillas** prepara una liga con 30 jugadores y táctica en JSON; **Batch** hace lo mismo con varias ligas; **Kits** genera uniformes. Después, **FL26 Mod Studio** construye el mundo para que Football Life utilice las plantillas y formaciones nuevas.
