---
title: "Launch Round 1 - Preparación para el Día de Lanzamiento"
date: 2025-12-30T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "preparation", "launch-day"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Preparación para el Día de Lanzamiento'
---

{{< figure src="cover" caption="alt" >}}

Este documento contiene las **instrucciones esenciales de preparación** que todos los participantes deben completar antes del día de lanzamiento de la Ronda 1 de la cadena **Infinite Improbability Drive**.

> 📖 **Contexto:** Este documento es la continuación del proceso de la Ronda 1. Asegúrate de haber completado los pasos anteriores:
> - [Ronda 1 Original](/es/posts/launch-round-1/) - Proceso inicial
> - [Corrección de la Ronda 1](/es/posts/launch-round-1-correction/) - Envío del archivo Génesis editado

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px;">

## ⚠️ IMPORTANTE: Preparación Esencial para el Lanzamiento

**Este documento contiene pasos CRÍTICOS que todos los participantes DEBEN completar antes del día de lanzamiento.**

- ✅ Estos pasos son **obligatorios** para todos los participantes de la Ronda 1
- ⏰ Completa estos pasos **lo antes posible** para asegurar que tu nodo esté listo
- 🔒 La configuración correcta es esencial para el consenso y la estabilidad de la red

</div>

---

## 📅 Disponibilidad del Génesis Final

El equipo publicará el **archivo Génesis final** y el **script de descarga** una vez que:

1. Todos los participantes hayan enviado su información de nodo (IP y subdominio deseado)
2. El equipo haya configurado todos los dominios seguros en Cloudflare
3. El equipo haya compilado el Génesis final con todas las gentx válidas

**📢 El equipo notificará a través de los canales oficiales cuando el Génesis final y los valores de seed nodes y persistent peers estén disponibles.**

---

## 🔧 Pasos de Preparación

### Paso 1: Configuración del Firewall

Antes de continuar con cualquier otra configuración, es **CRÍTICO** configurar correctamente el firewall para proteger tu servidor y permitir las conexiones necesarias.

**🔴 IMPORTANTE 🔴** 
Asegúrate de **permitir primero el puerto SSH (22)** antes de habilitar el firewall. Si no lo haces, perderás el acceso a la consola de tu servidor.

Sigue estas guías en orden:

1. **Configuración General del Firewall**
   - [Guía de Configuración del Firewall](https://docs.infinitedrive.xyz/es/drive/services/ports/firewall-configuration/)

2. **Configuración Específica para Infinite Creative Network (node2-infinite-creative)**
   - [Configuración del Firewall para node2-infinite-creative](https://docs.infinitedrive.xyz/es/drive/services/catalog/node2-infinite-creative/#firewall-configuration)

---

### Paso 2: Obtener tu Node ID

Necesitarás tu Node ID para la configuración de seed nodes. Para obtenerlo:

```bash
# Accede al contenedor
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash

# Obtén tu Node ID
infinited comet show-node-id
```

**Ejemplo de salida:**
```
dd5689375610aaa35b69ed311d69e51ea5557474
```

Anota este valor, lo necesitarás para el siguiente paso.

---

### Paso 3: Enviar Información de tu Nodo

Cada participante debe enviar por **mensaje directo (DM, no en chats públicos)** a **Cypher Xenia (@XeniaCypher88)** la siguiente información:

- **Tu IP del servidor**
- **El nombre que deseas como subdominio** (formato: `server-TU-NOMBRE`)
- **Tu Node ID** (obtenido en el paso anterior)

**Ejemplo:**
```
IP: 192.168.123.123
Subdominio: server-red
Node ID: dd5689375610aaa35b69ed311d69e51ea5557474
```

**💡 Nota:** Intenta usar nombres cortos para el subdominio 😜

El equipo utilizará esta información para:
- Configurar dominios seguros en Cloudflare (en lugar de compartir IPs directamente)
- Compilar la lista de seed nodes y persistent peers
- Preparar el Génesis final

**Formato del dominio que se utilizará:**
```
TU-SUBDOMINIO.infinitedrive.xyz
```

**Ejemplo:** `server-red.infinitedrive.xyz`

---

### Paso 4: Modificar config.toml

Necesitas realizar dos modificaciones importantes en el archivo de configuración.

**💻 En tu máquina Host, asegúrate de estar dentro de la carpeta del servicio infinite-creative:**
```bash
cd drive/services/node2-infinite-creative
```

#### 4.1: Configurar laddr para Acceso RPC

Abre el archivo de configuración con Nano para editarlo:
```bash
nano persistent-data/config/config.toml
```

Esto permitirá ver los datos de tu nodo RPC desde la web, no solo desde dentro de tu servidor. Es muy útil para validaciones esenciales.

Busca la sección `[rpc]` y configura `laddr` para usar `0.0.0.0` como dirección. El puerto `26657` está bien.

**Edita el valor para que quede así:**
```toml
###################################
###     RPC Server Configuration Options    ###
###################################
[rpc]

# TCP or UNIX socket address for the RPC server to listen on
laddr = "tcp://0.0.0.0:26657"
```

#### 4.2: Deshabilitar PEX

En el mismo archivo de configuración, ve a la sección `[p2p]` y busca la opción `pex`. Debe estar configurada en `false`.

Esto evita un problema sobre desconexiones en cadenas pequeñas (como nuestro caso).

**Configura el valor así:**
```toml
############################
###    P2P Configuration Options      ###
##############################
[p2p]
...
pex = false
```

**Para guardar los cambios en Nano:**
- Presiona `Ctrl + O` para guardar
- Presiona `Enter` para confirmar
- Presiona `Ctrl + X` para salir

---

### Paso 5: Configurar P2P External Address

Esta configuración permitirá que otros nodos encuentren tu nodo. Es requerida cuando eres un seed node. Para el lanzamiento inicial, todos debemos ser seed nodes para permitir que otros validadores conozcan nuestros nodos.

**⚠️ IMPORTANTE:** Asegúrate de **detener el servicio infinite-creative** antes de continuar con este paso.

Cada vez que necesitamos editar el archivo docker-compose, es necesario detener el servicio primero:
```bash
./drive.sh down
```

Abre el archivo docker-compose con Nano para editarlo:
```bash
nano docker-compose.yml
```

En `NODE_P2P_EXTERNAL_ADDRESS`, agrega tu nuevo dominio con el puerto usado para el puerto P2P de infinite-creative.

En este caso, el puerto debe ser `26676`.

**Formato:**
```
TU-SUBDOMINIO.infinitedrive.xyz:26676
```

**Ejemplo:**
```yaml
###########################
#      Network P2P Configuration       #
###########################

#NODE_P2P_SEEDS: ""
#NODE_PERSISTENT_PEERS: ""
NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
```

**Para guardar los cambios en Nano:**
- Presiona `Ctrl + O` para guardar
- Presiona `Enter` para confirmar
- Presiona `Ctrl + X` para salir

**Luego, puedes levantar nuevamente tu servicio:**
```bash
./drive.sh up -d
```

---

### Paso 6: Configurar Seed Nodes y Persistent Peers

Los valores de `NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS` serán proporcionados por el equipo una vez que todos los participantes hayan enviado su información requerida.

**📢 El equipo publicará estos valores junto con el Génesis final cuando estén listos.**

**Formato esperado para seed nodes:**
```
node_id@subdominio.infinitedrive.xyz:26656,node_id@subdominio2.infinitedrive.xyz:26656,...
```

**Ejemplo:**
```
dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26656,abc123def456@server-blue.infinitedrive.xyz:26656
```

Una vez que recibas estos valores, deberás agregarlos en tu archivo `docker-compose.yml` en las variables correspondientes:

```yaml
NODE_P2P_SEEDS: "node_id@subdominio.infinitedrive.xyz:26656,node_id2@subdominio2.infinitedrive.xyz:26656"
NODE_PERSISTENT_PEERS: "node_id@subdominio.infinitedrive.xyz:26656,node_id2@subdominio2.infinitedrive.xyz:26656"
```

---

### Paso 7: Descargar el Génesis Final

Una vez que el equipo publique el Génesis final, deberás descargarlo y reemplazar tu archivo Génesis actual.

**El equipo proporcionará:**
- El script de descarga
- Las instrucciones específicas para actualizar el Génesis
- Los valores finales de seed nodes y persistent peers

**📢 Mantente atento a los anuncios oficiales para recibir estas instrucciones.**

---

## 🔐 Seguridad: Uso de Dominios Seguros

Para asegurar que este proceso sea seguro, **no compartiremos direcciones IP directamente**. En su lugar, utilizaremos **dominios seguros a través de Cloudflare**.

**En lugar de usar la dirección IP `123.456.789`, usaremos:**
```
server-red.infinitedrive.xyz
```

De esta manera, al definir los seed nodes para cada uno de nosotros en los archivos de configuración, usaremos un formato como este:

```
node_id@server-red.infinitedrive.xyz:26656
```

Esto proporciona:
- ✅ Mayor seguridad (no exponemos IPs directamente)
- ✅ Flexibilidad (fácil cambio de IP sin actualizar configuraciones)
- ✅ Mejor gestión de red

---

## 📝 Resumen de Pasos

```
PREPARACIÓN PARA EL LANZAMIENTO:
1. Configurar firewall (SSH primero, luego otros puertos)
   ↓
2. Obtener Node ID
   ↓
3. Enviar información del nodo por DM (IP, subdominio, Node ID)
   ↓
4. Modificar config.toml (laddr RPC y pex = false)
   ↓
5. Configurar P2P External Address en docker-compose.yml
   ↓
6. Esperar valores de Seed Nodes y Persistent Peers del equipo
   ↓
7. Configurar Seed Nodes y Persistent Peers cuando estén disponibles
   ↓
8. Descargar Génesis final cuando esté disponible
   ↓
9. ¡Listo para el día de lanzamiento!
```

---

## ❓ Preguntas Frecuentes

### ¿Qué pasa si no completo estos pasos antes del lanzamiento?

Si no completas estos pasos, tu nodo no podrá conectarse correctamente con otros nodos de la red, lo que impedirá el consenso y la participación en el lanzamiento.

### ¿Cuándo estarán disponibles los valores de seed nodes y persistent peers?

Los valores estarán disponibles una vez que todos los participantes hayan enviado su información de nodo y el equipo haya configurado los dominios. El equipo notificará cuando estén listos.

### ¿Puedo usar mi IP directamente en lugar de un subdominio?

No. Por razones de seguridad y flexibilidad, todos los participantes deben usar subdominios configurados por el equipo a través de Cloudflare.

### ¿Qué pasa si cambio mi IP después de enviar la información?

Contacta al equipo de desarrollo para actualizar la configuración del dominio. Los dominios facilitan este proceso sin necesidad de actualizar todas las configuraciones.

### ¿Necesito reiniciar el servicio después de cada cambio?

Sí, después de modificar `docker-compose.yml` necesitas reiniciar el servicio. Sin embargo, los cambios en `config.toml` pueden requerir solo un reinicio del nodo, no del servicio completo.

---

## 📚 Referencias

- [Ronda 1 Original](/es/posts/launch-round-1/) - Documento original de la Ronda 1
- [Corrección de la Ronda 1](/es/posts/launch-round-1-correction/) - Proceso de corrección y envío del Génesis editado
- [Guía Completa: Crear Gentx](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/) - Documentación técnica completa
- [Configuración del Firewall](https://docs.infinitedrive.xyz/es/drive/services/ports/firewall-configuration/) - Guía general de firewall
- [node2-infinite-creative](https://docs.infinitedrive.xyz/es/drive/services/catalog/node2-infinite-creative/) - Documentación del servicio

---

**¡Gracias por tu preparación y colaboración!** Estos pasos son esenciales para el éxito del lanzamiento de la cadena Creative. Completa todos los pasos lo antes posible para asegurar que estemos todos listos para el día de lanzamiento.

