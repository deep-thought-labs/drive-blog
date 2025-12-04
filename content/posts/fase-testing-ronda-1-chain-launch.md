---
title: "Fase de Testing - Ronda 1: Chain Launch"
date: 2024-02-01T10:00:00Z
tags: ["testing", "ronda-1", "chain-launch", "genesis", "validadores"]
---

# Fase de Testing - Ronda 1: Chain Launch

La primera ronda de testing se enfoca en el proceso más crítico en el ciclo de vida de una cadena: **el Chain Launch (lanzamiento de la cadena)**.

## Importancia del Chain Launch

Este proceso es **esencial y muy importante** en el ciclo de vida de la cadena porque:

- **Solo se hace una vez** en el ciclo de vida de la cadena
- Tiene que estar **muy bien definido y estandarizado**
- Durante la fase de testing, lo realizaremos varias veces en nuestras cadenas de prueba
- **Para la Mainnet, solo se hará una vez**
- Por eso debemos practicar y tener el procedimiento muy claro para que cuando lo hagamos en la Mainnet, todo funcione perfectamente, como un reloj suizo

## Preparación Requerida

Antes de comenzar, cada participante debe tener:

- ✅ Su nodo inicializado
- ✅ Su validation key correctamente creada en el lugar correcto
- ✅ Haber practicado la gestión de keys y verificado el `priv_validator_key`

## Proceso del Chain Launch

### Paso 1: Genesis Base

Se proporcionará un archivo Genesis base que **aún no contiene ninguna cuenta de validación**.

### Paso 2: Creación de Transacciones

Cada participante usará este archivo Genesis para crear las transacciones necesarias para crear su validador dentro de él.

### Paso 3: Generación de Archivos

Cada participante proporcionará un archivo generado a partir de los datos de su nodo.

### Paso 4: Compilación del Genesis Final

Se recopilarán todos los archivos de cada participante y se combinarán en un único Genesis.

### Paso 5: Lanzamiento de la Cadena

Este nuevo archivo Genesis, creado al compilar todas las transacciones realizadas por separado, será el Genesis de la cadena.

Cuando todos iniciemos nuestros nodos usando este nuevo archivo Genesis, **todos seremos validadores de la cadena recién creada, desde el bloque 1**.

## Flujo del Proceso

```
1. Genesis Base (sin validadores)
   ↓
2. Cada participante crea transacciones para su validador
   ↓
3. Cada participante genera archivo con datos de su nodo
   ↓
4. Compilación de todos los archivos en un Genesis único
   ↓
5. Todos inician nodos con el Genesis final
   ↓
6. Cadena lanzada con todos como validadores desde bloque 1
```

## Instrucciones Específicas

Las instrucciones específicas y pasos detallados se proporcionarán más adelante. Este post sirve como preparación y contexto para el proceso.

## Objetivos de esta Ronda

- Validar el proceso completo de Chain Launch
- Asegurar que todos los participantes puedan crear validadores correctamente
- Verificar que el Genesis compilado funcione correctamente
- Documentar cualquier problema o mejora necesaria en el proceso

## Próximas Rondas

Después de esta primera ronda, continuaremos con:

- Rondas de testing cíclicas (cada jueves)
- Tests de diferentes funcionalidades
- Movimiento de tokens
- Votación en la DAO
- Simulación de escenarios diversos

---

**Esta es la ronda más importante** porque establece el procedimiento estándar que usaremos para el lanzamiento oficial de la Mainnet. La precisión y atención al detalle son cruciales.

