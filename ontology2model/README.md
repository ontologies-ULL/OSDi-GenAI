# OSDi → Excel con interpretación por IA

Proyecto de Laura Ramallo Pérez para generar modelos Excel a partir de casos
Turtle (TTL) descritos con OSDi. Se distribuye como **paquete completo 3.1.1**:
los **35 archivos originales están dentro del ZIP**, con su estructura de
carpetas. La interfaz e instrucciones corresponden a 3.1.1; el motor y el
contrato semántico siguen en 3.1.

## Descargar

- [OSDi_Project_20261113_v3_1_1.zip](OSDi_Project_20261113_v3_1_1.zip): paquete
  original con código, plantilla XLSM, esquemas, documentación y pruebas.
- [INSTRUCCIONES_PROYECTO.txt](INSTRUCCIONES_PROYECTO.txt): instrucciones activas
  3.1.1, idénticas byte por byte a las incluidas en el ZIP.

## Configurar el proyecto en ChatGPT

El uso habitual se realiza dentro de un proyecto de ChatGPT preparado así:

1. **Crea un proyecto nuevo con memoria solo del proyecto.** Selecciona esta
   modalidad al crearlo para mantener el contexto de este trabajo dentro del
   propio proyecto.
2. **Configura las instrucciones del proyecto.** Abre
   `INSTRUCCIONES_PROYECTO.txt`, copia su contenido completo y pégalo en el
   apartado **Instrucciones del proyecto** de la configuración.
3. **Adjunta el ZIP en Fuentes.** Añade
   `OSDi_Project_20261113_v3_1_1.zip` completo, sin descomprimir, a las
   **Fuentes del proyecto**. Mantén este ZIP como única versión activa del
   paquete y conserva las versiones anteriores fuera de ese contexto.
4. **Abre una conversación dentro del proyecto y adjunta el TTL del caso.**
   Con el proyecto ya configurado, basta adjuntar ese archivo para iniciar
   el flujo documentado.

El entorno de IA debe poder leer archivos y ejecutar Python. El contenido de
`INSTRUCCIONES_PROYECTO.txt` constituye las instrucciones canónicas de la
versión activa.

## Cómo funciona

En el flujo de IA documentado, basta adjuntar el TTL del caso: la IA procesa
todos sus `DecisionTreeModel` y redacta internamente un plan por modelo, con
parámetros, intervenciones, ramas, probabilidades y fórmulas respaldadas por
el caso. Python prepara hechos, comprueba el plan y escribe el XLSM.

**La CLI no llama a un LLM ni deduce el árbol automáticamente.** El plan lo
redacta la IA; el usuario no tiene que proporcionarlo. La modalidad documentada
de proyecto ChatGPT no requiere una clave API adicional.

### Comandos técnicos

Para ejecutar directamente los comandos de Python, descarga el ZIP y extráelo
en una carpeta nueva, **conservando todas sus subcarpetas**. Empieza por
`00_EMPEZAR.txt`. Se requiere Python 3.10 o superior y su biblioteca estándar.

Los comandos se ejecutan desde la carpeta extraída:

```bash
python preparar_contexto.py --ttl CASO.ttl --out CONTEXTO_NUEVO
```

Después de que la IA lea el caso y redacte `PLAN.json`:

```bash
python generar_desde_plan.py --ttl CASO.ttl --plan PLAN.json --out SALIDA_NUEVA
```

Cada intento utiliza una carpeta nueva. La entrega habitual contiene enlaces
a los XLSM válidos; los contextos, planes e informes se conservan internamente
para trazabilidad. Los bloqueos se comunican sin inventar datos ni entregar
libros inválidos.

## Evaluación y límites

El paquete no incluye casos reservados ni sus Excel esperados. Las referencias
de evaluación deben permanecer fuera del contexto generador; las pruebas
sintéticas incluidas verifican herramientas.

El alcance es un perfil documentado de árboles finitos. Solo se interpretan y
modifican `Parameters` y `Model`; los demás componentes se conservan opacos.
La validación estructural no demuestra toda la semántica, corrección clínica
o económica, recálculo nativo de Excel ni funcionamiento de macros.

Dentro del ZIP se encuentran `docs/FLUJO_IA.txt`, `docs/FORMATO_PLAN_IA.txt`,
`docs/CONTRATO_SEMANTICO.txt`, `docs/COMPATIBILIDAD_VERSIONES.txt`,
`docs/EVALUACION_INDEPENDIENTE.txt` y `docs/LIMITES.txt`.

## Integridad

El ZIP se conserva sin modificaciones. Su SHA-256 es:

```text
c8ca06defe3ebf8d744a6af1c06bb5435d42355c6c87427c5a384b85123b90d7
```

El `MANIFEST.json` incluido enumera los 34 archivos que acompañan al propio
manifiesto, con tamaños y huellas SHA-256.
