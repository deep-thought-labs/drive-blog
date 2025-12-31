---
title: "Launch Round 1 - Corrección"
date: 2025-12-29T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "correction"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Corrección'
---

{{< figure src="cover" caption="alt" >}}

Este documento describe la **corrección del flujo** y los **pasos actualizados** para continuar con la Ronda 1 del lanzamiento de la cadena **Infinite Improbability Drive**.

> 📖 **Contexto:** Este es un documento de continuación de la [Ronda 1 original](/es/posts/launch-round-1/). Si no participaste en el periodo original de la Ronda 1 (25-28 de diciembre de 2025), este documento no aplica para ti, ya que el periodo de participación original ha finalizado.

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ IMPORTANTE: Solo para Participantes de la Ronda 1 Original

**Este documento y los pasos descritos aquí son SOLO para participantes que ya participaron en el periodo original de la Ronda 1 (25-28 de diciembre de 2025).**

- ✅ Si ya enviaste tu gentx durante el periodo original, debes seguir estos pasos
- ❌ Si NO participaste en el periodo original, este documento no aplica para ti
- ⏰ No hay un tiempo límite estricto para completar estos pasos, pero es importante hacerlo **lo antes posible** para que el equipo pueda proceder con la corrección

</div>

---

## 🔍 Error Detectado en el Flujo Original

Durante el proceso de la Ronda 1, se identificó un **error fundamental** en el flujo inicial que requiere corrección antes de poder continuar con el lanzamiento de la cadena.

### ¿Qué ocurrió?

El archivo Génesis base proporcionado inicialmente tenía un problema crítico: **no contenía las cuentas y saldos pre-agregados** que debería haber incluido desde el principio.

**Flujo que se siguió (incorrecto):**
1. Se proporcionó un Génesis base vacío (sin cuentas)
2. Cada participante agregó su propia cuenta individualmente en su archivo Génesis local
3. Cada participante creó su gentx basándose en su Génesis editado
4. Al intentar compilar todas las gentxs, el proceso fallaría porque el Génesis base original no tenía las cuentas desde el inicio

**Flujo correcto que debió seguirse:**
1. Los participantes proporcionan sus cuentas públicas al equipo
2. El equipo pre-llena el Génesis base con todas las cuentas y saldos
3. Se proporciona el Génesis base completo a todos los participantes
4. Los participantes solo crean sus gentx basándose en el Génesis base completo
5. El equipo compila todas las gentxs exitosamente

### Plan de Corrección

Para corregir este error, el proceso se dividirá en dos fases:

**Fase 1: Recopilación de Información**
- Los participantes envían sus archivos Génesis editados (que contienen sus cuentas agregadas)
- El equipo compila todos los Génesis individuales para extraer todas las cuentas y saldos
- El equipo crea un nuevo Génesis base v2 que incluye todas las cuentas desde el inicio

**Fase 2: Proceso Corregido**
- Se proporciona el nuevo Génesis base v2 a todos los participantes
- El equipo verifica que las gentx generadas previamente son válidas con el nuevo Génesis base v2
- El equipo procede directamente con la fase de lanzamiento
- Se publica un nuevo documento de preparación específico para el evento de lanzamiento

---

## 📋 Fase 1: Enviar tu Archivo Génesis Editado

En esta fase, necesitamos que envíes el archivo Génesis que editaste durante el proceso original (el que contiene tu cuenta agregada).

### Paso 1: Localizar tu Archivo Génesis Editado

Tu archivo Génesis editado se encuentra en el mismo lugar donde trabajaste durante el proceso original:

**Dentro del contenedor:**
```bash
~/.infinited/config/genesis.json
```

**Desde el sistema host (fuera del contenedor):**
```bash
services/node2-infinite-creative/persistent-data/config/genesis.json
```

O desde el directorio del servicio:
```bash
cd services/node2-infinite-creative
ls -la persistent-data/config/genesis.json
```

### Paso 2: Verificar que es el Archivo Correcto

Antes de extraer y enviar, verifica que el archivo contiene tu cuenta:

**Desde el contenedor:**
```bash
# Accede al contenedor
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Lista tus llaves para obtener tu dirección
infinited keys list --keyring-backend file --home ~/.infinited

# Verifica que tu dirección está en el Génesis (reemplaza TU_DIRECCION con tu dirección real)
cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "TU_DIRECCION")'
```

**Desde el host:**
```bash
# Reemplaza TU_DIRECCION con tu dirección real
cat services/node2-infinite-creative/persistent-data/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "TU_DIRECCION")'
```

Si ves tu dirección con el saldo correcto, ese es el archivo que necesitas enviar.

### Paso 3: Extraer el Archivo Génesis del Servidor

El archivo Génesis está almacenado en el volumen persistente de Docker, por lo que es accesible desde el sistema host sin necesidad de copiarlo manualmente.

**Si estás trabajando directamente en el servidor:**

1. **Navega al directorio del servicio Creative:**
   ```bash
   cd services/node2-infinite-creative
   ```

2. **Crea un directorio para el archivo:**
   ```bash
   mkdir -p ~/genesis-correction
   ```

3. **Copia el archivo Génesis:**
   ```bash
   cp persistent-data/config/genesis.json ~/genesis-correction/
   ```

**Si estás en un servidor remoto**, necesitas descargar el archivo a tu computadora local usando `scp`:

```bash
# Desde tu computadora local
# Reemplaza <usuario>, <servidor> y la ruta según tu configuración
scp <usuario>@<servidor>:/ruta/a/drive/services/node2-infinite-creative/persistent-data/config/genesis.json ~/genesis-correction/
```

**Explicación del comando `scp`:**
- `<usuario>`: Es el nombre de usuario con el que inicias sesión en tu servidor
- `<servidor>`: Se refiere a la dirección IP o el nombre de dominio de tu servidor (por ejemplo: `192.168.1.100` o `mi-servidor.com`)
- La ruta después de los dos puntos (`:`) es la ruta completa al archivo en el servidor
- `~/genesis-correction/` es el directorio destino en tu computadora local

**⚠️ Importante:** Al ejecutar este comando, es muy probable que el sistema te solicite credenciales o autorización para realizar la transferencia. Estas son las mismas credenciales que usas cuando inicias sesión en tu servidor (contraseña o clave SSH).

**Ejemplo completo:** Si tu usuario es `ubuntu`, tu servidor tiene la IP `192.168.1.100`, y tu directorio de trabajo es `/home/ubuntu/drive`:
```bash
# Desde tu computadora local
mkdir -p ~/genesis-correction
scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/config/genesis.json ~/genesis-correction/
```

### Paso 4: Comprimir tu Archivo Génesis

Una vez que tengas el archivo Génesis en tu computadora local (ya sea copiado directamente en el servidor o descargado con `scp`), comprímelo con un nombre que incluya tu moniker para facilitar la identificación:

1. **Navega al directorio donde está el archivo:**
   ```bash
   cd ~/genesis-correction
   ```

2. **Comprime el archivo con tu moniker:**
   ```bash
   tar -czf genesis-<tu-moniker>-round-1-correction.tar.gz genesis.json
   
   # O usando zip
   zip genesis-<tu-moniker>-round-1-correction.zip genesis.json
   ```

   **Ejemplo:** Si tu moniker es `mi-validador`:
   ```bash
   tar -czf genesis-mi-validador-round-1-correction.tar.gz genesis.json
   ```

   **⚠️ Importante:**
   - El archivo comprimido debe incluir tu moniker en el nombre para facilitar la identificación
   - El archivo original `genesis.json` dentro del comprimido mantendrá su nombre original

### Paso 5: Enviar tu Archivo Génesis

Envía el archivo comprimido por Telegram a **Cypher Xenia** con el nombre de usuario **@XeniaCypher88**.

1. Abre Telegram
2. Busca el usuario **@XeniaCypher88** (Cypher Xenia)
3. Envía el archivo comprimido (por ejemplo: `genesis-<tu-moniker>-round-1-correction.tar.gz`)
4. **Incluye en el mensaje:** "Archivo Génesis editado para corrección de Ronda 1 - Moniker: [tu-moniker]"

**⚠️ Importante:**
- Solo envía el archivo comprimido que contiene `genesis.json`, **NO** el genesis completo con gentx
- Verifica que estás enviando el archivo correcto antes de enviarlo
- Mantén una copia de seguridad de tu archivo Génesis

### Paso 6: Confirmación

Una vez que envíes tu archivo Génesis, el equipo lo recibirá y lo procesará. El equipo compilará todos los archivos Génesis recibidos para crear el Génesis base v2.

**No hay un tiempo límite estricto**, pero es importante enviar tu archivo **lo antes posible** para que el equipo pueda proceder con la corrección.

---

## 📝 Resumen del Proceso (Fase 1)

```
FASE 1: Corrección
1. Localizar archivo Génesis editado
   ↓
2. Verificar que contiene tu cuenta
   ↓
3. Extraer/descargar archivo Génesis del servidor
   ↓
4. Comprimir archivo Génesis con moniker
   ↓
5. Enviar archivo Génesis comprimido por Telegram
   ↓
6. Esperar confirmación y Génesis base v2
```

---

## ❓ Preguntas Frecuentes

### ¿Qué pasa si no envío mi archivo Génesis editado?

Si no envías tu archivo Génesis editado, el equipo no podrá incluir tu cuenta en el Génesis base v2, y no podrás participar en el lanzamiento de la cadena.

### ¿Hay un tiempo límite para enviar mi archivo Génesis?

No hay un tiempo límite estricto, pero es importante enviarlo **lo antes posible** para que el equipo pueda proceder con la corrección y crear el Génesis base v2.

### ¿Puedo modificar mi archivo Génesis antes de enviarlo?

No, debes enviar exactamente el mismo archivo Génesis que usaste para crear tu gentx original. No hagas modificaciones adicionales.

### ¿Qué pasa si ya no tengo mi archivo Génesis editado?

Si ya no tienes tu archivo Génesis editado, contacta al equipo de desarrollo para encontrar una solución alternativa.

### ¿Cuándo estará disponible el Génesis base v2?

El Génesis base v2 estará disponible una vez que el equipo haya recibido y procesado todos los archivos Génesis individuales de los participantes. El equipo notificará cuando esté listo.

---

## 📢 Actualización: Proceso de Lanzamiento

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 123, 255, 0.1);">

### 🔄 Actualización del Proceso

Una vez que el equipo recibió y procesó todos los archivos Génesis individuales:

1. **El equipo extrajo** todas las cuentas y saldos de todos los archivos Génesis recibidos
2. **El equipo creó** el nuevo Génesis base v2 que incluye todas las cuentas desde el inicio
3. **El equipo verificó** que las gentx generadas previamente por los participantes son válidas con el nuevo Génesis base v2

**✅ Resultado de la verificación:** Las gentx que los participantes ya generaron previamente son exactamente las mismas que si las hubieran generado nuevamente con el Génesis base v2. Por lo tanto, **no es necesario que los participantes vuelvan a generar nuevas gentx**.

**👉 El equipo procederá directamente con la preparación para el evento de lanzamiento. Consulta el siguiente documento para los pasos esenciales de preparación:**
**→ [Launch Round 1 - Preparación para el Día de Lanzamiento](/es/posts/launch-round-1-preparation/)**

</div>

---

## 📚 Referencias Técnicas

- [Guía Completa: Crear Gentx](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/) - Documentación técnica completa

---

## 🔗 Documentos Relacionados - Round 1

Esta sección contiene todos los documentos relacionados con Round 1, en orden cronológico:

- **[Guía de Fase de Testing](/es/posts/testing-phase-guide/)** - Guía general de la fase de testing (punto de inicio)
- **[Launch Round 1](/es/posts/launch-round-1/)** - Documento original de la Ronda 1 con el flujo inicial
- **[Launch Round 1 - Preparación para el Día de Lanzamiento](/es/posts/launch-round-1-preparation/)** - Pasos esenciales de preparación para el lanzamiento

---

**Gracias por tu paciencia y colaboración en este proceso de corrección.** Tu participación es esencial para el éxito del lanzamiento de la cadena Creative.

