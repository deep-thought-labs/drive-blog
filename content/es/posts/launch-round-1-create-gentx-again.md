---
title: "Launch Round 1 - Crear Gentx Otra Vez"
date: 2025-12-29T05:00:00Z
expiryDate: 2020-01-01T00:00:00Z
draft: true
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "v2"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Crear Gentx Otra Vez'
---

{{< figure src="cover" caption="alt" >}}

Este documento contiene las **instrucciones para crear tu gentx nuevamente** usando el Génesis base v2, como parte del proceso corregido de la Ronda 1 del lanzamiento de la cadena **Infinite Improbability Drive**.

> 📖 **Contexto:** Este documento es la continuación de la [corrección de la Ronda 1](/es/posts/launch-round-1-correction/). Asegúrate de haber completado la Fase 1 (enviar tu archivo Génesis editado) antes de seguir estas instrucciones.

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ IMPORTANTE: Solo para Participantes de la Ronda 1 Original

**Este documento y los pasos descritos aquí son SOLO para participantes que ya participaron en el periodo original de la Ronda 1 (25-28 de diciembre de 2025) y que ya completaron la Fase 1 de corrección (envío del archivo Génesis editado).**

- ✅ Si ya enviaste tu archivo Génesis editado en la Fase 1, debes seguir estos pasos
- ❌ Si NO participaste en el periodo original o NO completaste la Fase 1, este documento no aplica para ti

</div>

---

## 📋 Crear tu Gentx Nuevamente con Génesis Base v2

El equipo ha compilado todos los archivos Génesis individuales recibidos y ha creado el **Génesis base v2** que incluye todas las cuentas y saldos de todos los participantes desde el inicio.

Ahora debes seguir estos pasos para crear tu nueva gentx basándote en el Génesis base v2.

### Paso 1: Eliminar tu Gentx Anterior

Antes de descargar el nuevo Génesis base v2, necesitas eliminar la gentx anterior que creaste. Esta gentx anterior no se utilizará porque fue creada con el Génesis anterior, y ahora necesitamos crear una nueva gentx utilizando el nuevo archivo Génesis corregido (Génesis base v2).

Para asegurarnos de que realmente eliminamos todos los archivos relacionados, **recomendamos eliminar toda la carpeta de gentx** en lugar de buscar y eliminar archivos individuales. Esto garantiza un estado limpio para crear tu nueva gentx.

**⚠️ ADVERTENCIA IMPORTANTE:** Ten mucho cuidado al ejecutar estos comandos. Asegúrate de eliminar **SOLO** la carpeta `gentx/` y **NO** borres por accidente otras carpetas importantes como `config/` completa u otras carpetas del sistema. Verifica cuidadosamente la ruta antes de ejecutar el comando.

**Dentro del contenedor:**
```bash
# Accede al contenedor
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Verifica el contenido de la carpeta gentx antes de eliminarla
ls -la ~/.infinited/config/gentx/

# Elimina toda la carpeta de gentx
rm -rf ~/.infinited/config/gentx/

# Verifica que la carpeta fue eliminada (debería mostrar un error indicando que no existe)
ls -la ~/.infinited/config/gentx/
```

**Desde el sistema host:**
```bash
# Verifica el contenido de la carpeta gentx antes de eliminarla
ls -la services/node2-infinite-creative/persistent-data/config/gentx/

# Elimina toda la carpeta de gentx
rm -rf services/node2-infinite-creative/persistent-data/config/gentx/

# Verifica que la carpeta fue eliminada (debería mostrar un error indicando que no existe)
ls -la services/node2-infinite-creative/persistent-data/config/gentx/
```

### Paso 2: Descargar el Génesis Base v2

El equipo ha compilado el Génesis base v2 que incluye todas las cuentas y saldos de todos los participantes desde el inicio.

**Dentro del contenedor:**
```bash
# Accede al contenedor
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Descarga el Génesis base v2
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base-v2.json
```

**⚠️ Importante:**
- Este Génesis base v2 incluye todas las cuentas y saldos de todos los participantes
- Reemplazará cualquier Génesis existente en tu configuración

### Paso 3: Verificar el Génesis Base v2

Una vez descargado el Génesis base v2:

1. **Verifica el Chain ID:**
   ```bash
   cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
   ```
   
   **Chain ID esperado para Creative:** `infinite_421018002-1`

2. **Verifica que tu cuenta está incluida:**
   ```bash
   # Lista tus llaves para obtener tu dirección
   infinited keys list --keyring-backend file --home ~/.infinited
   
   # Verifica que tu dirección está en el Génesis (reemplaza TU_DIRECCION con tu dirección real)
   cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances[] | select(.address == "TU_DIRECCION")'
   ```

3. **Valida el Génesis base v2:**
   ```bash
   infinited genesis validate-genesis --home ~/.infinited
   ```
   
   Si la validación es exitosa, puedes proceder con confianza.

### Paso 4: Generar tu Nueva Gentx

**IMPORTANTE:** En este proceso corregido, **NO necesitas agregar tu cuenta al Génesis**, ya que ya está incluida en el Génesis base v2.

Solo necesitas generar tu gentx directamente:

```bash
infinited genesis gentx <nombre-de-tu-llave> 10000000000000000000cdrop \
  --chain-id infinite_421018002-1 \
  --commission-rate "0.01" \
  --commission-max-rate "0.05" \
  --commission-max-change-rate "0.01" \
  --min-self-delegation "1000000000000000000" \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ Importante:** Reemplaza `<nombre-de-tu-llave>` con el nombre exacto de tu llave que verificaste anteriormente. Sugerimos editar o hacer la edición correspondiente en un archivo de texto plano para posteriormente solo copiar y pegar el comando en tu terminal para un proceso más sencillo.

**Parámetros específicos para Creative:**
- **Chain ID:** `infinite_421018002-1` (Creative)
- **Autodelegación:** `10000000000000000000cdrop` (10 tokens)
- **Tasa de comisión inicial:** `0.01` (1%)
- **Tasa de comisión máxima:** `0.05` (5%)
- **Cambio máximo de tasa:** `0.01` (1%)
- **Autodelegación mínima:** `1000000000000000000` (1 token)

### Paso 5: Validar tu Nueva Gentx

Antes de entregar tu gentx, valida que funciona correctamente:

```bash
# Recopilar gentxs (incluye la tuya)
infinited genesis collect-gentxs --home ~/.infinited

# Validar el genesis resultante
infinited genesis validate-genesis --home ~/.infinited
```

Si la validación es exitosa, tu gentx está lista para entregar.

### Paso 6: Extraer y Enviar tu Nueva Gentx

1. **Localizar tu archivo gentx:**
   ```bash
   ls -la ~/.infinited/config/gentx/
   ```
   
   Verás un archivo con formato `gentx-<hash>.json`. Anota el nombre completo de este archivo.

2. **Extraer el archivo gentx del servidor:**
   
   El archivo gentx está almacenado en el volumen persistente de Docker, por lo que es accesible desde el sistema host sin necesidad de copiarlo manualmente.
   
   **Si estás trabajando directamente en el servidor:**
   ```bash
   cd services/node2-infinite-creative
   ls -la persistent-data/config/gentx/
   
   # Copiar manteniendo el nombre original (reemplaza <hash> con el hash real de tu archivo)
   mkdir -p ~/gentx-round-1-v2
   cp persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **Si estás en un servidor remoto**, necesitas descargar el archivo a tu computadora local usando `scp`:
   ```bash
   # Desde tu computadora local (reemplaza <usuario>, <servidor>, <hash> y la ruta según tu configuración)
   mkdir -p ~/gentx-round-1-v2
   scp <usuario>@<servidor>:/ruta/a/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-<hash>.json ~/gentx-round-1-v2/
   ```
   
   **Explicación del comando `scp`:**
   - `<usuario>`: Es el nombre de usuario con el que inicias sesión en tu servidor
   - `<servidor>`: Se refiere a la dirección IP o el nombre de dominio de tu servidor (por ejemplo: `192.168.1.100` o `mi-servidor.com`)
   - La ruta después de los dos puntos (`:`) es la ruta completa al archivo en el servidor
   - `~/gentx-round-1-v2/` es el directorio destino en tu computadora local
   
   **⚠️ Importante:** Al ejecutar este comando, es muy probable que el sistema te solicite credenciales o autorización para realizar la transferencia. Estas son las mismas credenciales que usas cuando inicias sesión en tu servidor (contraseña o clave SSH).
   
   **Ejemplo completo:** Si tu usuario es `ubuntu`, tu servidor tiene la IP `192.168.1.100`, tu directorio de trabajo es `/home/ubuntu/drive`, y tu archivo se llama `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   mkdir -p ~/gentx-round-1-v2
   scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1-v2/
   ```

3. **Comprimir el archivo gentx:**
   Una vez que tengas el archivo gentx en tu computadora local, comprímelo con un nombre que incluya tu moniker para facilitar la identificación:
   
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-<tu-moniker>-round-1-v2.tar.gz gentx-<hash>.json
   
   # O usando zip
   zip gentx-<tu-moniker>-round-1-v2.zip gentx-<hash>.json
   ```
   
   **Ejemplo:** Si tu moniker es `mi-validador` y tu archivo se llama `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   cd ~/gentx-round-1-v2
   tar -czf gentx-mi-validador-round-1-v2.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
   ```
   
   **⚠️ Importante:**
   - El archivo comprimido debe incluir tu moniker en el nombre para facilitar la identificación
   - El archivo JSON gentx dentro del comprimido debe mantener su nombre original (con el hash, no lo renombres)

4. **Enviar por Telegram:**
   - Envía el archivo comprimido por Telegram a **Cypher Xenia** con el nombre de usuario **@XeniaCypher88**
   - **Incluye en el mensaje:** "Gentx v2 para corrección de Ronda 1 - Moniker: [tu-moniker]"

---

## 📝 Resumen del Proceso

```
1. Eliminar gentx anterior
   ↓
2. Descargar Génesis base v2
   ↓
3. Verificar Génesis base v2 (Chain ID y tu cuenta)
   ↓
4. Validar Génesis base v2
   ↓
5. Generar nueva gentx (sin agregar cuenta)
   ↓
6. Validar nueva gentx
   ↓
7. Extraer nueva gentx del servidor
   ↓
8. Comprimir gentx con moniker
   ↓
9. Enviar gentx comprimida por Telegram
```

---

## ❓ Preguntas Frecuentes

### ¿Qué pasa si no elimino la carpeta de gentx anterior?

Si no eliminas la carpeta de gentx antes de descargar el Génesis base v2, puede haber conflictos. Es importante eliminar toda la carpeta de gentx para empezar con un estado limpio.

### ¿Puedo usar la misma gentx que creé anteriormente?

No, debes crear una nueva gentx basándote en el Génesis base v2. La gentx anterior fue creada con un Génesis diferente y no será válida con el nuevo Génesis base v2.

### ¿Qué pasa si mi cuenta no está en el Génesis base v2?

Si tu cuenta no está en el Génesis base v2, significa que no enviaste tu archivo Génesis editado en la Fase 1, o hubo un problema al procesarlo. Contacta al equipo de desarrollo para resolver este problema.

### ¿Cuándo debo enviar mi nueva gentx?

Debes enviar tu nueva gentx lo antes posible ahora que el Génesis base v2 está disponible. El equipo compilará todas las gentxs recibidas para crear el Génesis final.

---

## 📚 Referencias

- [Ronda 1 Original - Primer Flujo](/es/posts/launch-round-1/) - Documento original de la Ronda 1
- [Corrección de la Ronda 1](/es/posts/launch-round-1-correction/) - Fase 1: Envío del archivo Génesis editado
- [Guía Completa: Crear Gentx](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/) - Documentación técnica completa

---

**Gracias por tu paciencia y colaboración en este proceso de corrección.** Tu participación es esencial para el éxito del lanzamiento de la cadena Creative.

