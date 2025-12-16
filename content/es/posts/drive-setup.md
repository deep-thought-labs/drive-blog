---
title: "Preparación con Drive"
date: 2025-11-27T10:00:00Z
tags: ["testing", "drive"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Fase de Testing - Preparación con Drive'
---

{{< figure src="cover" caption="alt" >}}

Antes de comenzar con las rondas de testing, es fundamental preparar correctamente nuestro entorno y entender la importancia crítica de la gestión segura de keys (claves) y seed phrases (frases semilla).

## Instalación de Drive

**Drive** es la herramienta cliente desarrollada por el Lab que permite gestionar múltiples nodos y servicios de manera unificada.

### Recursos de Drive

- **Repositorio**: [github.com/deep-thought-labs/drive](https://github.com/deep-thought-labs/drive)
- **Documentación**: La documentación técnica está disponible en el repositorio y se está refinando continuamente para hacerla más accesible

### Funcionalidades de Drive

Drive permite:
- Gestionar múltiples nodos
- Administrar servicios
- Próximamente: soporte para nodos QL1, permitiendo ejecutar ambos validadores en el mismo servidor

> **Nota**: Actualmente, la demanda operativa para ambas redes es muy baja, por lo que ejecutarlas en el mismo servidor es perfectamente factible, evitando costos duplicados.

## Gestión de Keys: Aspectos Críticos

### ⚠️ Importancia de las Seed Phrases

La gestión correcta de keys es **absolutamente crítica** para la seguridad de tu validador. Presta especial atención a estos puntos:

### Proceso Correcto de Inicialización

1. **Crea tus keys** usando Drive y la utilidad de interfaz gráfica para generación de keys
2. **Aprende a almacenarlas** correctamente
3. **Guarda tu seed phrase offline** — preferiblemente en papel
4. **Practica inicializando el nodo** usando esa key que ya tienes

### Verificación Crítica: priv_validator_key

El valor más importante a verificar es que el archivo `priv_validator_key` generado en tu carpeta de configuración después de inicializar el nodo **siempre sea el mismo valor** cuando inicialices tu nodo con tu recovery key.

**Esto es lo más importante**: Asegúrate de que siempre sea el mismo valor cada vez que uses la misma recovery key.

### Proceso de Verificación Recomendado

Antes de ejecutar la transacción "create-validator", debes:

1. **Inicializa un nodo 2 o 3 veces con `--recover`**, usando la misma seed phrase
2. **Asegúrate de poder generar siempre el mismo `priv_validator_key`**
3. **Conoce tu `priv_validator_key` exacto y correcto** y verifica que esté presente en tu archivo de configuración
4. **Solo entonces** ejecuta la transacción "create-validator"

### ⚠️ Advertencia Importante

**Si creas un validador pero NO usaste una seed phrase en el Init del nodo (con `--recover`), NO hay forma de regenerar el `priv_validator_key` exacto nuevamente.**

Esto representa un riesgo: **Si pierdes el archivo `priv_validator_key`, también pierdes tu validador.**

### Entendiendo priv_validator_key

Es importante entender el propósito de `priv_validator_key`:

- El validador lo usa para **firmar sus bloques**
- **NO tiene el poder** de mover, delegar o realizar cualquier otra operación que pertenezca al titular del token
- Esas operaciones son responsabilidad de las keys que se agregan al Key Ring en un proceso separado

### Mejores Prácticas

**Idealmente**, las keys usadas en el Key Ring y el `priv_validator_key` generado durante el proceso INIT deberían usar **la misma seed phrase**.

## Práctica Recomendada

Te recomendamos que todos practiquen con su nodo usando Drive y la utilidad de interfaz gráfica para generación de keys:

- Crea tus keys
- Aprende cómo almacenarlas
- Guarda tu seed phrase offline en papel
- Practica inicializando el nodo usando esa key

## Próximos Pasos

Una vez que tengas Drive instalado y hayas practicado la gestión de keys, estaremos listos para la primera ronda de testing: **el Chain Launch**.

---

**Recuerda**: La seguridad de tu validador depende completamente de la gestión correcta de tus keys. Tómate el tiempo necesario para practicar y verificar que todo funcione correctamente antes de proceder.

