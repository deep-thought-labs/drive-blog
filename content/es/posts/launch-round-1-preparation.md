---
title: "Launch Round 1 - Preparación para el Día de Lanzamiento"
date: 2025-12-30T00:00:00Z
draft: false
tags: ["testing", "gentx", "creative", "chain-launch", "round-1", "preparation", "launch-day"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Launch Round 1 - Preparación para el Día de Lanzamiento'
---

{{< figure src="cover" caption="alt" >}}

Este documento contiene las **instrucciones finales de preparación** y toda la información necesaria para el día de lanzamiento de la Ronda 1 de la cadena **Infinite Improbability Drive**.

> 📖 **Contexto:** Este documento es la continuación del proceso de la Ronda 1. Asegúrate de haber completado los pasos anteriores:
> - [Ronda 1 Original](/es/posts/launch-round-1/) - Proceso inicial
> - [Corrección de la Ronda 1](/es/posts/launch-round-1-correction/) - Envío del archivo Génesis editado

---

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 200, 0, 0.1);">

## ✅ Estado: Todo Listo para el Lanzamiento

**Todos los participantes han completado exitosamente la fase de preparación:**

- ✅ Todos los participantes enviaron su información de nodo (IP, subdominio, Node ID)
- ✅ El equipo configuró todos los dominios seguros en Cloudflare
- ✅ El equipo compiló el Génesis final con todas las gentx válidas
- ✅ Los valores de seed nodes y persistent peers están listos
- ✅ El script de descarga del Génesis está disponible

**🎯 Lo único que queda es completar la configuración final en tu nodo y estar listo para iniciar en el momento acordado del lanzamiento sincronizado.**

</div>

---

## 📋 Información Solicitada a los Participantes (Completada)

Como parte del proceso de preparación, se solicitó a todos los participantes proporcionar la siguiente información y completar ciertas configuraciones:

### Información Enviada por los Participantes

1. **IP del servidor** - Dirección IP del servidor donde está alojado el nodo
2. **Nombre de subdominio deseado** - Formato: `server-TU-NOMBRE` (ejemplo: `server-red`)
3. **Node ID del nodo** - Obtenido mediante el comando `infinited comet show-node-id`

**✅ Todos los participantes completaron exitosamente el envío de esta información.**

### Configuraciones Solicitadas

1. **Configuración del firewall** - Para proteger los servidores y permitir las conexiones necesarias
2. **Obtener Node ID** - Identificador único del nodo para la configuración de red

**✅ Todos los participantes completaron exitosamente estas configuraciones.**

---

## 🔧 Configuraciones que Debes Realizar en tu Nodo

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(255, 193, 7, 0.1);">

### ⚠️ IMPORTANTE: Revisa tu Configuración

**Los participantes deberían tener estas configuraciones completadas antes del día de lanzamiento.**

Si no lo has hecho hasta ahora, **asegúrate de tenerlo realizado y correcto**. Revisa por favor y asegúrate de que todos los valores configurados estén como se describen en esta guía.

</div>

### 🔴 Reglas Importantes para Modificar Archivos

**⚠️ CRÍTICO - Lee esto antes de continuar:**

1. **Para modificar `config.toml`:**
   - ✅ **NO necesitas detener el servicio** - Puedes editar el archivo mientras el servicio está en ejecución
   - ✅ El servicio puede seguir corriendo durante la edición

2. **Para modificar `docker-compose.yml`:**
   - ⛔ **DEBES detener el servicio ANTES** de editar el archivo
   - ✅ Edita el archivo y guarda los cambios
   - ✅ **DEBES levantar el servicio DESPUÉS** de guardar los cambios

3. **Para operaciones dentro del contenedor (bash):**
   - ✅ **El servicio DEBE estar en ejecución** - No lo detengas antes de hacer operaciones dentro del contenedor
   - ✅ Usa `./drive.sh exec infinite-creative bash` para acceder al contenedor

---

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

### Paso 2: Modificar config.toml

**✅ IMPORTANTE:** Para modificar `config.toml`, **NO necesitas detener el servicio**. Puedes editar el archivo mientras el servicio está en ejecución.

**💻 En tu máquina Host, asegúrate de estar dentro de la carpeta del servicio infinite-creative:**
```bash
cd drive/services/node2-infinite-creative
```

**Abre el archivo de configuración con Nano para editarlo:**
```bash
nano persistent-data/config/config.toml
```

Necesitas realizar dos modificaciones importantes en el archivo de configuración.

#### 2.1: Configurar laddr para Acceso RPC

Esta configuración permitirá ver los datos de tu nodo RPC desde la web, no solo desde dentro de tu servidor. Es muy útil para validaciones esenciales.

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

#### 2.2: Deshabilitar PEX

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

**✅ Una vez que están estos cambios guardados, surtirán efecto cuando el nodo se inicie (o cuando se detenga y vuelva a iniciar si el nodo ya estaba en ejecución).** Es importante distinguir entre el **nodo** (el binario que se ejecuta dentro del contenedor) y el **contenedor** (el servicio Docker). En este caso, todas las modificaciones que estamos haciendo en el archivo `config.toml` se están haciendo con el nodo detenido, ya que este no se iniciará hasta el momento acordado del lanzamiento. Sin embargo, si en el futuro quisieras modificar tu archivo `config.toml` por algún motivo, **necesitas detener y volver a iniciar el nodo** para que los cambios surtan efecto.

---

## 📋 Valores de Configuración Listos

<div style="border: 3px solid currentColor; border-left: 8px solid currentColor; padding: 20px; margin: 30px 0; border-radius: 4px; background-color: rgba(0, 123, 255, 0.1);">

### 🔗 Seed Nodes y Persistent Peers

Los valores de `NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS` están listos y disponibles en la **documentación oficial del servicio**. Todos los participantes han proporcionado su información y estos valores están configurados correctamente.

**📍 Ubicación en la documentación:**

Los valores completos están disponibles en la sección **"Network P2P Configuration"** de la documentación oficial:

**[Configuración de Red P2P - Infinite Creative Network](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)**

En esta sección encontrarás:
- Una tabla completa con todos los nodos confiables (Node ID, dirección, puerto)
- Los valores completos de `NODE_P2P_SEEDS` listos para copiar
- Los valores completos de `NODE_PERSISTENT_PEERS` listos para copiar
- Información detallada sobre cada variable de configuración

**💡 Cómo obtener los valores:**

1. Accede a la [documentación oficial](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)
2. Busca la sección **"Network P2P Configuration"**
3. Expande la sección **"Network P2P Configuration Variables"**
4. Copia los valores completos de `NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS`
5. Pega estos valores en tu archivo `docker-compose.yml`

**⚠️ NOTA:** Los valores de `NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS` son idénticos. Copia el mismo valor para ambas variables.

### 📥 URL del Génesis Final

El Génesis final está disponible en la siguiente URL:

```
https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

**Chain ID esperado:** `infinite_421018002-1`

</div>

---

### Paso 3: Configurar docker-compose.yml

**⛔ IMPORTANTE:** Para modificar `docker-compose.yml`, **DEBES detener el servicio ANTES** de editar el archivo.

#### 3.1: Detener el Servicio

**Primero, detén el servicio infinite-creative:**

```bash
cd drive/services/node2-infinite-creative
./drive.sh down
```

#### 3.2: Abrir el Archivo docker-compose.yml

**Abre el archivo docker-compose con Nano para editarlo:**
```bash
nano docker-compose.yml
```

#### 3.3: Configurar NODE_P2P_EXTERNAL_ADDRESS

Esta es la dirección que previamente se les fue entregada a cada participante basándose en el subdominio que ellos eligieron. El puerto es el correspondiente a esta red creativa (26676).

Busca la variable `NODE_P2P_EXTERNAL_ADDRESS` en la sección de configuración de red P2P y configura el valor con tu subdominio y el puerto:

**Debe tener el formato:**
```
TU-SUBDOMINIO.infinitedrive.xyz:26676
```

**⚠️ IMPORTANTE:** Asegúrate de que la línea de `NODE_P2P_EXTERNAL_ADDRESS` **NO esté comentada** en tu archivo docker-compose.yml. Si la línea está comentada (con `#` al inicio), descoméntala antes de modificar el valor. Si solo modificas el valor pero la línea sigue comentada, no surtirá ningún efecto.

**Ejemplo de cómo debería verse en tu docker-compose.yml:**
```yaml
###########################
#      Network P2P Configuration       #
###########################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
# NODE_P2P_SEEDS: ""
# NODE_PERSISTENT_PEERS: ""
```

#### 3.4: Configurar NODE_P2P_SEEDS y NODE_PERSISTENT_PEERS

Ambas variables (`NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS`) se encuentran en la misma ubicación de la documentación oficial. En ese lugar podrás copiar y pegar cada uno de los campos para reemplazarlos en tu archivo docker-compose.yml.

**Primero, obtén los valores de la documentación:**

1. Accede a la [documentación oficial - Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration)
2. Expande la sección **"Network P2P Configuration Variables"**
3. Copia el valor completo de `NODE_P2P_SEEDS`
4. Copia el valor completo de `NODE_PERSISTENT_PEERS`

**⚠️ IMPORTANTE:** Aunque ambas variables tienen los mismos valores, **debes copiar y pegar cada una de manera individual** con el objetivo de que cada una tenga su correcta clave-valor en el archivo docker-compose.yml.

**Luego, en tu archivo docker-compose.yml:**

1. Busca la variable `NODE_P2P_SEEDS` y pega el valor copiado de la documentación
2. Busca la variable `NODE_PERSISTENT_PEERS` y pega el valor copiado de la documentación

**⚠️ IMPORTANTE:** Asegúrate de que las líneas de `NODE_P2P_SEEDS` y `NODE_PERSISTENT_PEERS` **NO estén comentadas** en tu archivo docker-compose.yml. Si las líneas están comentadas (con `#` al inicio), descoméntalas antes de modificar los valores. Si solo modificas los valores pero las líneas siguen comentadas, no surtirá ningún efecto.

**Ejemplo de cómo debería verse en tu docker-compose.yml:**
```yaml
###########################
#      Network P2P Configuration       #
###########################

NODE_P2P_EXTERNAL_ADDRESS: "server-red.infinitedrive.xyz:26676"
NODE_P2P_SEEDS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
NODE_PERSISTENT_PEERS: "dd5689375610aaa35b69ed311d69e51ea5557474@server-red.infinitedrive.xyz:26676,e5c1b7423d098c660bb82b7f44f86e333cb6af9e@server-farmer.infinitedrive.xyz:26676,..."
```

**Nota:** Los valores en el ejemplo están truncados con `...` para mostrar el formato. Los valores reales completos los encontrarás en la documentación oficial.

**Para guardar los cambios en Nano:**
- Presiona `Ctrl + O` para guardar
- Presiona `Enter` para confirmar
- Presiona `Ctrl + X` para salir

#### 3.6: Levantar el Servicio

**Después de guardar los cambios, levanta nuevamente el servicio:**

```bash
./drive.sh up -d
```

**✅ El servicio ahora está configurado con los valores correctos de seed nodes y persistent peers.**

---

### Paso 4: Descargar y Reemplazar el Génesis Final

El Génesis final está listo y disponible. Debes descargarlo y reemplazar tu archivo Génesis actual antes del lanzamiento.

**✅ IMPORTANTE:** El servicio debe estar en ejecución para acceder al bash del contenedor. No necesitas detener el servicio.

#### 4.1: Acceder al Bash del Contenedor

Accede al bash del contenedor:

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative bash
```

#### 4.2: Descargar el Génesis Final

Dentro del contenedor, descarga el Génesis final:

```bash
# Descarga el Génesis final (usa la URL de la sección "Valores de Configuración Listos")
curl -o ~/.infinited/config/genesis.json https://assets.infinitedrive.xyz/tests-round1/genesis-final.json
```

#### 4.3: Verificar el Génesis Descargado

Dentro del contenedor, verifica que el Génesis se descargó correctamente:

```bash
# Verifica el Chain ID
cat ~/.infinited/config/genesis.json | jq -r '.chain_id'

# Chain ID esperado para Creative: infinite_421018002-1

# Valida el Génesis
infinited genesis validate-genesis --home ~/.infinited
```

Si la validación es exitosa, el Génesis está listo.

**Sal del contenedor:**
```bash
exit
```

**✅ Tu nodo ahora está configurado con el Génesis final oficial.**

---

## 📝 Resumen de Pasos de Configuración

```
CONFIGURACIÓN FINAL PARA EL LANZAMIENTO:
1. Verificar configuración de firewall ✅
   ↓
2. Modificar config.toml ⬅️ NO requiere detener servicio
   (laddr RPC y pex = false)
   ↓
3. Detener servicio ⛔
   ↓
4. Modificar docker-compose.yml ⬅️ REQUIERE servicio detenido
   (P2P External Address, Seed Nodes, Persistent Peers)
   ↓
5. Levantar servicio ✅
   ↓
6. Acceder al bash del contenedor ✅
   (servicio en ejecución)
   ↓
7. Descargar Génesis final dentro del contenedor ⬅️ REQUIERE servicio en ejecución
   ↓
8. Verificar Génesis descargado dentro del contenedor
   ↓
9. Salir del contenedor
   ↓
10. ¡Esperar el momento acordado para iniciar el nodo! 🚀
```

---

## ⏰ Sincronización del Lanzamiento

### 🎉 Lanzamiento Especial de Round 1

Para este lanzamiento inicial de la Ronda 1, que es una cadena de prueba, hemos decidido hacerlo de manera especial para conmemorar este momento histórico.

**📅 Fecha y Hora del Lanzamiento:**

**1 de enero de 2026 a las 0:00 UTC-0** - El primer minuto del nuevo año, el primer jueves del año.

**⏰ Información Importante sobre el Lanzamiento:**

- **No es crítico ni obligatorio** que el 100% de los participantes inicien su nodo exactamente al mismo tiempo
- Los participantes que tengan la oportunidad de hacerlo en ese momento deben hacerlo
- Los participantes que no puedan hacerlo en ese momento exacto pueden hacerlo en cualquier momento después de esa hora
- Tu nodo se sincronizará con la red y comenzará a validar normalmente

**🚀 Para iniciar tu nodo:**

Tienes dos opciones para iniciar tu nodo:

**Opción 1: Usando la Interfaz Gráfica (Recomendado)**

1. Abre la interfaz gráfica (ver [Interfaz Gráfica](https://docs.infinitedrive.xyz/es/drive/guides/blockchain-nodes/graphical-interface/))
2. Navega: Menú Principal → **"Node Operations"** → **"Start Node"**
3. La interfaz mostrará el proceso de inicio y confirmará cuando el nodo esté en ejecución

![Iniciar Nodo](https://docs.infinitedrive.xyz/images/node-ui-operations-op1-start.png)

**Opción 2: Usando Línea de Comandos**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-start
```

**📖 Para más información sobre cómo iniciar y detener nodos, consulta la [guía completa de Iniciar/Detener Nodo](https://docs.infinitedrive.xyz/es/drive/guides/blockchain-nodes/start-stop-node/).**

---

### 📊 Ver Logs del Nodo

Después de iniciar tu nodo, es importante monitorear los logs para verificar que todo esté funcionando correctamente y observar el progreso de sincronización.

**Tienes dos formas de ver los logs:**

1. **Ver logs en tiempo real** - Para monitorear la actividad del nodo mientras está ejecutándose
2. **Ver las últimas N líneas de logs** - Útil cuando hubo algún error y quieres ver qué ocurrió desde el inicio del nodo

**Acceso a las opciones de logs mediante Interfaz Gráfica:**

1. Abre la interfaz gráfica (ver [Interfaz Gráfica](https://docs.infinitedrive.xyz/es/drive/guides/blockchain-nodes/graphical-interface/))
2. Navega: Menú Principal → **"Node Monitoring"**

   ![Menú de Monitoreo del Nodo](https://docs.infinitedrive.xyz/images/node-ui-monitoring.png)

3. En el submenú **"Node Monitoring"**, encontrarás las opciones para ver los logs

---

#### Ver Logs en Tiempo Real

**Opción 1: Usando la Interfaz Gráfica (Recomendado)**

En el submenú **"Node Monitoring"**, selecciona **"Follow Logs"**. Los logs comenzarán a mostrarse en tiempo real. Para detener el seguimiento, presiona `Ctrl+C`.

**Opción 2: Usando Línea de Comandos**

```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs -f
```

O usando la sintaxis alternativa:
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs --follow
```

**Qué hace:** Transmite las entradas de logs en tiempo real a medida que se escriben en el archivo de logs (similar a `tail -f`).

**Salida esperada:** Muestra un flujo continuo de entradas de logs. Presiona `Ctrl+C` para detener.

**Cuándo usar:** Monitorear la actividad del nodo mientras está ejecutándose, observar el progreso de sincronización en tiempo real, o depurar problemas mientras ocurren.

---

#### Ver las Últimas N Líneas de Logs

Esta opción es útil cuando por alguna razón hubo algún error y quieres ver qué fue lo que ocurrió o cuáles fueron los logs del nodo desde el inicio. Para ver desde el inicio de la operación del nodo, especialmente si hubo algún error en la ejecución, es recomendable ver las últimas 200 líneas, ya que en las primeras 200 líneas podrías ver qué es lo que ocurrió.

**Tienes dos opciones para ver las últimas N líneas de logs:**

**Opción 1: Usando la Interfaz Gráfica**

En el submenú **"Node Monitoring"**, selecciona **"View Logs"**. La interfaz mostrará las últimas 50 líneas de logs por defecto.

**Nota:** Si necesitas ver una cantidad específica de líneas (por ejemplo, 100 o 200 líneas), debes usar la línea de comandos (Opción 2).

**Opción 2: Usando Línea de Comandos**

Para ver las últimas 50 líneas (por defecto):
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs
```

Para ver las últimas 100 líneas:
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 100
```

Para ver las últimas 200 líneas (recomendado para ver desde el inicio del nodo):
```bash
cd services/node2-infinite-creative
./drive.sh exec infinite-creative node-logs 200
```

**Qué hace:** Muestra las últimas N líneas del archivo de logs del nodo (`/var/log/node/node.log`).

**Salida esperada:** Entradas de logs recientes mostrando:
- Mensajes de inicio del nodo
- Progreso de sincronización
- Procesamiento de bloques
- Errores o advertencias
- Estado de conexión

**Cuándo usar:** Para revisar actividad reciente del nodo, verificar errores desde el inicio de la ejecución, o revisar el progreso de sincronización cuando no necesitas ver los logs en tiempo real.

**📖 Para más información sobre monitoreo del nodo, consulta la [guía completa de Monitoreo del Nodo](https://docs.infinitedrive.xyz/es/drive/guides/blockchain-nodes/node-monitoring/).**

### 📅 Lanzamientos Futuros

Para lanzamientos futuros (Testnet y Mainnet), se utilizará el horario recomendado de **4:00 PM - 4:10 PM (UTC-0)**, que es el momento de máxima disponibilidad de participantes según el análisis realizado.

**✅ Asegúrate de haber completado todos los pasos de configuración antes del momento del lanzamiento.**

---

## 🔐 Seguridad: Uso de Dominios Seguros

Para asegurar que este proceso sea seguro, **no compartimos direcciones IP directamente**. En su lugar, utilizamos **dominios seguros a través de Cloudflare**.

**En lugar de usar la dirección IP `123.456.789`, usamos:**
```
server-red.infinitedrive.xyz
```

De esta manera, al definir los seed nodes para cada uno de nosotros en los archivos de configuración, usamos un formato como este:

```
node_id@server-red.infinitedrive.xyz:26656
```

Esto proporciona:
- ✅ Mayor seguridad (no exponemos IPs directamente)
- ✅ Flexibilidad (fácil cambio de IP sin actualizar configuraciones)
- ✅ Mejor gestión de red

---

## ❓ Preguntas Frecuentes

### ¿Qué pasa si no completo estos pasos antes del lanzamiento?

Si no completas estos pasos, tu nodo no podrá conectarse correctamente con otros nodos de la red, lo que impedirá el consenso y la participación en el lanzamiento. Es **crítico** completar la configuración de seed nodes, persistent peers y el Génesis final antes del momento acordado.

### ¿Puedo iniciar mi nodo antes del momento acordado?

No. Es esencial que todos los nodos se inicien simultáneamente en el momento acordado para asegurar un lanzamiento exitoso y sincronizado.

### ¿Puedo iniciar mi nodo después del momento acordado del lanzamiento?

Sí. Si no puedes iniciar tu nodo exactamente en el momento acordado (1 de enero de 2026 a las 0:00 UTC-0), puedes iniciarlo en cualquier momento después de esa hora. Tu nodo se sincronizará automáticamente con la red y comenzará a validar normalmente una vez que complete la sincronización.

### ¿Necesito detener el servicio para modificar config.toml?

No. Puedes modificar `config.toml` mientras el servicio está en ejecución. Sin embargo, es importante distinguir entre el **nodo** (el binario que se ejecuta dentro del contenedor) y el **contenedor** (el servicio Docker). Los cambios en `config.toml` surten efecto cuando el nodo se inicia o cuando se detiene y vuelve a iniciar si el nodo ya estaba en ejecución. En este caso, todas las modificaciones se están haciendo con el nodo detenido, ya que este no se iniciará hasta el momento acordado del lanzamiento. Si en el futuro quisieras modificar tu archivo `config.toml` por algún motivo, **necesitas detener y volver a iniciar el nodo** para que los cambios surtan efecto.

### ¿Necesito detener el servicio para modificar docker-compose.yml?

Sí. **DEBES detener el servicio antes** de modificar `docker-compose.yml` y **DEBES levantarlo después** de guardar los cambios.

### ¿Puedo hacer operaciones dentro del contenedor con el servicio detenido?

No. Para acceder al bash del contenedor y hacer operaciones dentro de él, **el servicio DEBE estar en ejecución**. Usa `./drive.sh exec infinite-creative bash` cuando el servicio esté corriendo.

### ¿Puedo usar mi IP directamente en lugar de un subdominio?

Sí, técnicamente es posible usar una dirección IP directamente. Sin embargo, **nosotros recomendamos no mostrarlas** por razones de seguridad y flexibilidad. Por esta razón, todos los participantes deben usar subdominios configurados por el equipo a través de Cloudflare.

---

## 📚 Referencias

- [Ronda 1 Original](/es/posts/launch-round-1/) - Documento original de la Ronda 1
- [Corrección de la Ronda 1](/es/posts/launch-round-1-correction/) - Proceso de corrección y envío del Génesis editado
- [Guía Completa: Crear Gentx](https://docs.infinitedrive.xyz/es/blockchain/genesis/create-gentx/) - Documentación técnica completa
- [Configuración del Firewall](https://docs.infinitedrive.xyz/es/drive/services/ports/firewall-configuration/) - Guía general de firewall
- [Infinite Creative Network - Documentación del Servicio](https://docs.infinitedrive.xyz/es/drive/services/catalog/node2-infinite-creative/) - Documentación completa del servicio
- [Network P2P Configuration](https://docs.infinitedrive.xyz/en/drive/services/catalog/node2-infinite-creative/#network-p2p-configuration) - Valores de seed nodes y persistent peers (documentación oficial)

---

**¡Gracias por tu preparación y colaboración!** Todos los participantes han completado exitosamente la fase de preparación. Ahora solo queda completar la configuración final (seed nodes, persistent peers y Génesis final) y estar listos para iniciar nuestros nodos en el momento acordado del lanzamiento sincronizado.

**🎯 Recuerda:** El éxito del lanzamiento depende de que todos iniciemos nuestros nodos simultáneamente en el momento acordado. ¡Estemos todos listos!
