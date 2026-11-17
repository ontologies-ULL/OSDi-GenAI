# OSDi → Excel con interpretación por IA

Este proyecto forma parte de mi TFG y permite generar árboles de decisión en
Excel a partir de casos descritos en Turtle (TTL) con la ontología OSDi.

He separado el proceso en dos partes: **la IA interpreta el caso y propone el
árbol; Python comprueba esa propuesta y construye el Excel**. El resultado se
guarda en formato `.xlsm`, utilizando la plantilla incluida en el paquete.

**Autora:** Laura Ramallo Pérez.

## Descargar

- [OSDi_Project_20261113_v3_1_1.zip](OSDi_Project_20261113_v3_1_1.zip): paquete
  completo con el código, la plantilla Excel, los esquemas OSDi, la documentación
  y las pruebas.
- [INSTRUCCIONES_PROYECTO.txt](INSTRUCCIONES_PROYECTO.txt): instrucciones que hay
  que copiar en la configuración del proyecto de ChatGPT.

El ZIP contiene los 35 archivos del proyecto, organizados en sus carpetas.
La versión 3.1.1 actualiza las instrucciones de uso y mantiene el código del
motor 3.1. Estas versiones son independientes de las versiones de OSDi.

## Configurar el proyecto en ChatGPT

1. **Crea un proyecto nuevo con memoria solo del proyecto.** Selecciona esta
   modalidad al crearlo.
2. **Configura las instrucciones del proyecto.** Abre
   `INSTRUCCIONES_PROYECTO.txt`, copia su contenido completo y pégalo en
   **Instrucciones del proyecto**, dentro de la configuración.
3. **Adjunta el ZIP en Fuentes.** Añade
   `OSDi_Project_20261113_v3_1_1.zip` completo, sin descomprimir, a las
   **Fuentes del proyecto**. Mantén únicamente esta versión del paquete en
   ese contexto.
4. **Abre una conversación dentro del proyecto y adjunta el TTL del caso.**
   No hace falta añadir una petición: las instrucciones indican que se
   procesen todos los modelos de árbol de decisión admitidos que contenga.

El entorno de ChatGPT debe poder leer archivos y ejecutar Python. Los recursos
ya están incluidos y no hace falta una clave API adicional.

## Cómo se genera el Excel

### 1. Leer el caso y preparar los datos

El proceso identifica los `DecisionTreeModel` del TTL y selecciona el esquema
local de OSDi que corresponda. El paquete incluye OSDi 1.0 y 1.1, con controles
de compatibilidad para los casos que admite.

Python organiza los elementos, relaciones, parámetros y valores en
`contexto.json`. Este archivo facilita la consulta de los datos del caso;
en esta etapa todavía no se ha construido el árbol.

### 2. Interpretar el modelo

La IA lee el TTL, el esquema y las reglas de interpretación. A partir de ellos
redacta un `plan.json` por modelo, donde define los parámetros, las
intervenciones, las ramas, las probabilidades y las fórmulas de costes y
resultados en salud. También registra las evidencias del TTL que justifican
sus decisiones.

El usuario no tiene que escribir el plan. Los programas Python lo reciben y
no llaman por sí solos a un modelo de IA.

### 3. Comprobar y revisar el plan

Python contrasta la propuesta con los datos del TTL y comprueba las referencias,
los parámetros y las reglas admitidas. También revisa las probabilidades:
sus rangos, la suma de las ramas que salen de un mismo nodo y el total de los
desenlaces de cada intervención.

Si hay errores corregibles, la IA revisa el plan y repite la generación en una
carpeta nueva. Se conservan los intentos para poder revisar qué ha ocurrido.
Los datos que faltan no se completan con valores de otros casos ni se toman
automáticamente como cero.

### 4. Construir el libro Excel

Una vez superadas las comprobaciones previas, Python transforma el plan en
celdas y fórmulas, distribuye las ramas y aplica el formato sobre
`PLANTILLA_BASE.xlsm`.

El contenido generado se organiza en dos hojas:

- **Parameters:** parámetros del modelo, valores, distribuciones y procedencia.
- **Model:** árbol de decisión, probabilidades, resultados de cada ruta y
  resultados agrupados por intervención.

El proceso conserva las demás hojas, macros y componentes de la plantilla.
Además de las dos hojas, solo ajusta los metadatos mínimos necesarios del libro.
Las macros no se ejecutan durante la generación.

### 5. Entregar los resultados

Tras superar los controles, se entregan los enlaces a los `.xlsm` válidos.
Contextos, planes e informes quedan como registros internos. Si un modelo se
bloquea, se explica el motivo y se continúa con los que puedan completarse.

## Archivos principales del paquete

Todos estos archivos están **dentro del ZIP**.

| Archivo o carpeta | Para qué sirve |
| --- | --- |
| `00_EMPEZAR.txt` e `INSTRUCCIONES_PROYECTO.txt` | Explican la configuración y establecen cómo debe trabajar la IA. |
| `OSDi.ttl` y `schemas/OSDi-1.0.ttl` | Esquemas locales OSDi 1.1 y 1.0. |
| `PLANTILLA_BASE.xlsm` | Libro base con la estructura y el formato del Excel. |
| `preparar_contexto.py` | Lee la entrada y prepara los datos que consultará la IA. |
| `generar_desde_plan.py` | Coordina la validación del plan y la generación del Excel. |
| `engine/ontology.py` y `engine/schema_selection.py` | Leen las relaciones del TTL y seleccionan el esquema local adecuado. |
| `engine/ai_contract.py`, `engine/semantic_guards.py` y `engine/evaluate.py` | Comprueban el plan, las reglas admitidas y las probabilidades. |
| `engine/plan_renderer.py` | Convierte el árbol y sus fórmulas en celdas y referencias de Excel. |
| `engine/presentation.py` y `engine/style_profile.json` | Aplican los estilos, las anchuras y la distribución visual del árbol. |
| `engine/ooxml.py` | Escribe las partes autorizadas del libro y conserva el resto de sus componentes. |
| `comparar_dos_hojas.py` | Compara después las hojas Parameters y Model con un Excel de referencia. |
| `docs/` | Flujo, formato del plan, reglas, compatibilidad y límites. |
| `tests/` | Pruebas de las herramientas con datos artificiales. |

## Alcance y evaluación

El soporte actual está centrado en árboles finitos con una enfermedad y una
población, dentro de las reglas documentadas. No hay soporte general para
modelos de Markov ni para cualquier estructura que pueda expresarse en OSDi.

Las comprobaciones detectan errores concretos, pero **superarlas no garantiza
que toda la interpretación clínica o económica sea correcta**. Tampoco se
ejecuta Excel para verificar su recálculo nativo o el funcionamiento de las
macros. La interpretación de la IA puede variar entre ejecuciones.

La evaluación frente a modelos de referencia se realiza por separado, después
de generar el resultado. `comparar_dos_hojas.py` permite revisar diferencias de
contenido, fórmulas, estilos y distribución en Parameters y Model. Los Excel
esperados y las respuestas de evaluación se mantienen fuera del contexto
generador.

## Ejecución manual (opcional)

Para utilizar los programas directamente, extrae el ZIP en una carpeta nueva
conservando sus subcarpetas. Se requiere Python 3.10 o superior y su biblioteca
estándar. Desde la carpeta extraída, prepara primero el contexto:

```bash
python preparar_contexto.py --ttl CASO.ttl --out CONTEXTO_NUEVO
```

Después de que la IA interprete el caso y redacte `PLAN.json`, genera el Excel:

```bash
python generar_desde_plan.py --ttl CASO.ttl --plan PLAN.json --out SALIDA_NUEVA
```

El detalle del flujo se encuentra en `docs/FLUJO_IA.txt` y el formato del plan en
`docs/FORMATO_PLAN_IA.txt`.
