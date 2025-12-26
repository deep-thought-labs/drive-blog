---
title: "Launch Round 1"
date: 2025-12-25T10:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Creative Network'
---

{{< figure src="cover" caption="alt" >}}

Esta es la **primera ronda de pruebas** para el lanzamiento de la cadena **Infinite Improbability Drive**. En esta ronda, trabajaremos con la **network Creative** de nuestra cadena y te guiaremos paso a paso para crear tu gentx (genesis transaction) que será incluida en el genesis final de la cadena.

**Sobre las networks de Infinite Improbability Drive:**
- **Creative**: Network utilizada para pruebas y desarrollo (esta primera ronda)
- **Testnet**: Network de pruebas más amplia
- **Mainnet**: Network principal de producción

La network Creative será utilizada para nuestra primera ronda de prueba de lanzamiento. Si no puedes participar en esta ronda, no te preocupes: realizaremos esta misma ronda de iteración en otras cadenas de manera repetitiva para garantizar que siempre tengamos resultados estables y que todos tengan oportunidades de participar.

## 📅 Periodo de Participación

**Fecha de publicación:** 25 de diciembre de 2025  
**Periodo de entrega:** Del 25 al 27 de diciembre de 2025 (2 días)

Durante estos dos días, los participantes tendrán la oportunidad de:

1. Descargar el genesis base proporcionado por el equipo
2. Crear su gentx siguiendo las instrucciones específicas
3. Entregar su archivo gentx al equipo de desarrollo

**Después del 27 de diciembre**, el equipo de desarrollo compilará todas las gentxs recibidas en un genesis final que será redistribuido a todos los participantes para proceder con el lanzamiento de la cadena.

---

## 🎯 Objetivo de esta Ronda

El objetivo de esta primera ronda es:

- **Validar el proceso completo** de creación de gentx en un entorno real
- **Probar la infraestructura** de Creative Network antes del lanzamiento oficial
- **Asegurar que todos los participantes** puedan crear y entregar sus gentxs correctamente
- **Compilar un genesis final** con todos los validadores participantes

---

## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener:

- ✅ **Drive instalado y configurado** (consulta [Instalación de Drive](/es/posts/drive-installation/))
- ✅ **Servicio Creative configurado** (`node2-infinite-creative`)
- ✅ **Nodo inicializado** usando el proceso de recuperación (recovery) con tu seed phrase de validador
- ✅ **Llave agregada al keyring** usando la misma seed phrase de validador que usaste para inicializar el nodo
- ✅ **Conocer el nombre de la llave** que agregaste al keyring (este nombre lo elegiste cuando agregaste la llave)
- ✅ **Acceso al servidor** donde está ejecutándose Drive

**⚠️ Importante sobre la llave:**
- Debes haber inicializado tu nodo usando el proceso de recuperación con tu seed phrase de validador
- Debes haber agregado esa misma seed phrase como una llave al keyring con un nombre específico (por ejemplo: `validator`, `my-validator`, etc.)
- **Debes recordar y tener claro cuál es el nombre de esa llave**, ya que lo necesitarás en todos los comandos de este proceso
- Este nombre de llave es el que usarás en los comandos `add-genesis-account` y `genesis gentx`

> 📖 **Documentación completa:** Para entender el proceso completo de creación de gentx, consulta la [guía completa en la documentación](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/).

---

## 🚀 Instrucciones Específicas para Creative

### Paso 1: Acceder al Contenedor de Creative

Navega al directorio del servicio Creative y accede al bash del contenedor:

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

Una vez dentro del contenedor, el binario `infinited` estará disponible directamente.

---

### Paso 2: Descargar el Genesis Base

Descarga el genesis base para Creative ejecutando el siguiente comando dentro del contenedor:

```bash
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-base.json
```

**⚠️ Importante:**
- Este comando descargará el genesis base directamente a la ubicación correcta (`~/.infinited/config/genesis.json`)
- Reemplazará el genesis existente que se generó durante la inicialización
- Asegúrate de estar dentro del contenedor antes de ejecutar el comando

### Verificar el Genesis Descargado

Verifica que el genesis se descargó correctamente y que el Chain ID es el correcto:

```bash
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'
```

**Chain ID esperado para Creative:** `infinite_421018002-1`

**Nota:** Si el comando de descarga cambia, el equipo de desarrollo te notificará a través del canal oficial de comunicación.

### Validar el Genesis Base

Antes de continuar, valida que el genesis base es correcto:

```bash
infinited genesis validate-genesis --home ~/.infinited
```

Si la validación es exitosa, puedes proceder con confianza.

---

### Paso 3: Verificar tu Llave en el Keyring

Antes de continuar, verifica que tu llave existe en el keyring y recuerda su nombre:

```bash
infinited keys list --keyring-backend file --home ~/.infinited
```

Este comando mostrará todas las llaves que tienes en el keyring. **Identifica y anota el nombre de la llave** que corresponde a tu validador (la que agregaste usando tu seed phrase de validador).

**Ejemplo de salida:**
```
- name: validator
  type: local
  address: infinite1abc123...
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"..."}'
```

En este ejemplo, el nombre de la llave es `validator`. **Usa este mismo nombre** en los siguientes pasos, reemplazando `validator` por el nombre real de tu llave.

---

### Paso 4: Agregar Fondos a la Cuenta en Genesis

**💡 Sugerencia:** Antes de ejecutar el comando, puedes prepararlo en un editor de texto plano para mayor facilidad. Esto te permitirá revisar y editar el comando completo (incluyendo el nombre de tu llave) antes de copiarlo y pegarlo en la consola.

Agrega tu cuenta al genesis con el saldo inicial. **Para esta primera ronda de Creative**, usa el siguiente monto:

```bash
infinited genesis add-genesis-account <nombre-de-tu-llave> 1000000000000000000000cdrop \
  --keyring-backend file \
  --home ~/.infinited
```

**⚠️ Importante:** Reemplaza `<nombre-de-tu-llave>` con el nombre exacto de tu llave que verificaste en el Paso 3 (por ejemplo: `validator`, `my-validator`, etc.).

**Parámetros específicos para Creative:**
- Denominación: `cdrop` (Creative drop)
- Monto: `1000000000000000000000cdrop` (1000 tokens en unidades atómicas)

### Verificar que la Cuenta fue Agregada Correctamente

Antes de generar la gentx, es recomendable verificar que tu cuenta fue agregada correctamente al genesis. Puedes hacerlo consultando el contenido del genesis:

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.bank.balances'
```

Este comando mostrará todos los balances en el genesis. Busca tu dirección pública (la misma que viste cuando listaste tus llaves) y verifica que tiene el monto correcto.

**Ejemplo de salida esperada para Creative:**
```json
[
  {
    "address": "infinite1rs3s0jx0rvnsjwfjch59lg9ypp6k3vmg2cn68j",
    "coins": [
      {
        "denom": "cdrop",
        "amount": "1000000000000000000000"
      }
    ]
  }
]
```

También puedes verificar la información de la cuenta en la sección de accounts:

```bash
cat ~/.infinited/config/genesis.json | jq '.app_state.auth.accounts'
```

**Ejemplo de salida esperada:**
```json
[
  {
    "@type": "/cosmos.auth.v1beta1.BaseAccount",
    "address": "infinite1rs3s0jx0rvnsjwfjch59lg9ypp6k3vmg2cn68j",
    "pub_key": null,
    "account_number": "0",
    "sequence": "0"
  }
]
```

**Nota:** Los valores mostrados son ejemplos. Para Mainnet la denominación será `drop`, para Testnet será `tdrop`, y para Creative será `cdrop`.

Si ves tu dirección con el monto correcto, puedes proceder con confianza a generar tu gentx.

---

### Paso 5: Generar la Gentx

**💡 Sugerencia:** Antes de ejecutar el comando, puedes prepararlo en un editor de texto plano para mayor facilidad. Esto te permitirá revisar y editar el comando completo antes de copiarlo y pegarlo en la consola.

Genera tu gentx con los parámetros específicos para Creative:

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

**⚠️ Importante:** Reemplaza `<nombre-de-tu-llave>` con el nombre exacto de tu llave que verificaste en el Paso 3.

**Parámetros específicos para esta ronda:**
- **Chain ID:** `infinite_421018002-1` (Creative)
- **Autodelegación:** `10000000000000000000cdrop` (10 tokens)
- **Tasa de comisión inicial:** `0.01` (1%)
- **Tasa de comisión máxima:** `0.05` (5%)
- **Cambio máximo de tasa:** `0.01` (1%)
- **Autodelegación mínima:** `1000000000000000000` (1 token)

---

### Paso 6: Validar tu Gentx

Antes de entregar tu gentx, valida que funciona correctamente:

```bash
# Recopilar gentxs (incluye la tuya)
infinited genesis collect-gentxs --home ~/.infinited

# Validar el genesis resultante
infinited genesis validate-genesis --home ~/.infinited
```

Si la validación es exitosa, tu gentx está lista para entregar.

---

### Paso 7: Localizar tu Archivo Gentx

Tu gentx se generó en el siguiente directorio dentro del contenedor:

```bash
~/.infinited/config/gentx/
```

El archivo gentx tiene un formato con un hash único, similar a: `gentx-adba573456c82908c3221163185703c421a2dd1f.json`

**⚠️ Importante:** El nombre del archivo NO incluye tu moniker, sino un hash único generado automáticamente. **NO debes renombrar este archivo JSON**.

Para ver el nombre exacto de tu archivo:

```bash
ls -la ~/.infinited/config/gentx/
```

Verás un archivo con formato `gentx-<hash>.json`. Anota el nombre completo de este archivo para los siguientes pasos.

---

### Paso 8: Extraer el Archivo Gentx del Servidor

El archivo gentx está almacenado en el volumen persistente de Docker, por lo que es accesible desde el sistema host sin necesidad de copiarlo manualmente.

**Desde el sistema host (fuera del contenedor):**

1. **Navega al directorio del servicio Creative:**
   ```bash
   cd services/node2-infinite-creative
   ```

2. **Localiza tu archivo gentx:**
   ```bash
   ls -la persistent-data/.infinited/config/gentx/
   ```

3. **Copia el archivo gentx manteniendo su nombre original:**
   
   **⚠️ Importante:** El archivo gentx tiene un nombre con formato hash (ejemplo: `gentx-adba573456c82908c3221163185703c421a2dd1f.json`). **NO debes renombrar este archivo JSON**. Mantén su nombre original tal como fue generado.
   
   ```bash
   # Crear un directorio específico para la gentx
   mkdir -p ~/gentx-round-1
   
   # Copiar manteniendo el nombre original (reemplaza <hash> con el hash real de tu archivo)
   cp persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/gentx-round-1/
   ```
   
   **Ejemplo:** Si tu archivo se llama `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
   ```bash
   mkdir -p ~/gentx-round-1
   cp persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
   ```

**Si estás en un servidor remoto**, puedes usar `scp` para descargarlo a tu computadora local manteniendo el nombre original:

```bash
# Desde tu computadora local (reemplaza <hash> con el hash real de tu archivo)
scp usuario@servidor:/ruta/a/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json ~/
```

**Explicación del comando `scp`:**
- `usuario`: Es el nombre de usuario con el que inicias sesión en tu servidor
- `@servidor`: Se refiere a la dirección IP o el nombre de dominio de tu servidor (por ejemplo: `@192.168.1.100` o `@mi-servidor.com`)
- La ruta después de los dos puntos (`:`) es la ruta completa al archivo en el servidor
- `~/` es el directorio destino en tu computadora local (tu directorio home)

**⚠️ Importante:** Al ejecutar este comando, es muy probable que el sistema te solicite credenciales o autorización para realizar la transferencia. Estas son las mismas credenciales que usas cuando inicias sesión en tu servidor (contraseña o clave SSH).

**Ejemplo completo:** Si tu usuario es `ubuntu`, tu servidor tiene la IP `192.168.1.100`, y tu archivo se llama `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
```bash
mkdir -p ~/gentx-round-1
scp ubuntu@192.168.1.100:/home/ubuntu/drive/services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-adba573456c82908c3221163185703c421a2dd1f.json ~/gentx-round-1/
```

---

### Paso 9: Comprimir y Enviar tu Gentx

**Comprimir el archivo:**

**⚠️ Importante:** 
- El archivo JSON gentx debe mantener su nombre original (con el hash, no lo renombres)
- El archivo comprimido SÍ debe incluir tu moniker en su nombre para facilitar la identificación

```bash
# Crear un archivo comprimido con tu moniker (reemplaza <moniker> con tu moniker real y <hash> con el hash de tu archivo)
tar -czf gentx-<moniker>-round-1.tar.gz gentx-<hash>.json

# O usando zip
zip gentx-<moniker>-round-1.zip gentx-<hash>.json
```

**Ejemplo:** Si tu moniker es `mi-validador` y tu archivo se llama `gentx-adba573456c82908c3221163185703c421a2dd1f.json`:
```bash
cd ~/gentx-round-1
tar -czf gentx-mi-validador-round-1.tar.gz gentx-adba573456c82908c3221163185703c421a2dd1f.json
```

**Estructura del archivo comprimido:**
- **Nombre del archivo comprimido:** `gentx-<tu-moniker>-round-1.tar.gz` (incluye tu moniker para identificación)
- **Contenido del archivo comprimido:** `gentx-<hash>.json` (el archivo JSON original con su nombre original)

**Enviar por Telegram:**

Envía el archivo comprimido directamente por Telegram a **RED** con el nombre de usuario **@teh_woman_in_red**.

1. Abre Telegram
2. Busca el usuario **@teh_woman_in_red** (RED)
3. Envía el archivo comprimido directamente

**⚠️ Importante:**
- Solo envía el archivo gentx, **NO** el genesis completo
- Verifica que estás enviando el archivo correcto antes de enviarlo
- Mantén una copia de seguridad de tu gentx
- Incluye tu moniker o identificador en el mensaje si el equipo lo solicita

---

## 📝 Información Adicional

### Verificar tu Gentx Antes de Enviar

Puedes verificar el contenido de tu gentx para asegurarte de que está correcta:

```bash
# Desde el contenedor (reemplaza <hash> con el hash de tu archivo)
cat ~/.infinited/config/gentx/gentx-<hash>.json | jq .
```

O desde el host:

```bash
# Reemplaza <hash> con el hash de tu archivo
cat services/node2-infinite-creative/persistent-data/.infinited/config/gentx/gentx-<hash>.json | jq .
```

Verifica que:
- El `chain_id` sea `infinite_421018002-1`
- El `moniker` dentro del contenido JSON sea el correcto
- Los montos y parámetros sean los especificados

---

## 🔄 Proceso Post-Entrega

Una vez que el equipo de desarrollo reciba todas las gentxs (hasta el 27 de diciembre):

1. **Compilación del Genesis Final:**
   - El equipo compilará todas las gentxs recibidas en un genesis final
   - Se validará que el genesis final sea correcto y completo

2. **Redistribución del Genesis Final:**
   - Todos los participantes recibirán el genesis final compilado
   - Se proporcionarán instrucciones para reemplazar el genesis local

3. **Lanzamiento de la Cadena:**
   - Una vez que todos tengan el genesis final, se procederá con el lanzamiento
   - Tu validador estará activo desde el bloque 1

---

## ❓ Preguntas Frecuentes

### ¿Qué pasa si no entrego mi gentx a tiempo?

Si no entregas tu gentx antes del 27 de diciembre, no podrás participar como validador en el lanzamiento inicial de Creative. Sin embargo, realizaremos esta misma ronda de iteración en otras cadenas de manera repetitiva, por lo que tendrás oportunidades futuras de participar.

### ¿Puedo modificar mi gentx después de enviarla?

No, una vez que envíes tu gentx, no podrás modificarla. Asegúrate de validarla correctamente antes de enviarla.

### ¿Qué información debo incluir al enviar la gentx?

Solo necesitas enviar el archivo gentx comprimido. El equipo de desarrollo puede solicitar información adicional si es necesario.

### ¿Cómo sé que mi gentx fue recibida correctamente?

El equipo de desarrollo confirmará la recepción de tu gentx. Si no recibes confirmación, contacta al equipo.

---

## 📚 Referencias

- [Guía Completa: Crear Gentx](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/) - Documentación técnica completa
- [Instalación de Drive](/es/posts/drive-installation/) - Cómo instalar Drive
- [Preparación como Validador](/es/posts/validator-preparation/) - Gestión de claves y seguridad

---

## 🎯 Resumen del Proceso

```
1. Acceder al contenedor de Creative
   ↓
2. Descargar genesis base
   ↓
3. Validar genesis base
   ↓
4. Verificar que tu llave existe en el keyring y recordar su nombre
   ↓
5. Agregar cuenta con fondos al genesis usando el nombre de tu llave (montos especificados)
   ↓
6. Generar gentx usando el nombre de tu llave con parámetros específicos de Creative
   ↓
7. Validar gentx y genesis
   ↓
8. Extraer gentx del servidor a tu computadora (manteniendo el nombre original)
   ↓
9. Comprimir manteniendo el nombre original y enviar por Telegram
   ↓
10. Esperar confirmación y genesis final
```

---

**¡Estamos emocionados de comenzar esta primera ronda de pruebas contigo!** Si tienes alguna pregunta o encuentras algún problema durante el proceso, no dudes en contactar al equipo de desarrollo.

**Periodo de entrega:** 25 - 27 de diciembre de 2025  
**Servicio:** Creative (node2-infinite-creative)  
**Chain ID:** `infinite_421018002-1`

