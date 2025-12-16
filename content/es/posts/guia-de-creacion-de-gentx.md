---
title: "Cómo crear una Gentx"
date: 2025-12-11T10:00:00Z
tags: ["guide", "genesis"]
cover: 'https://raw.githubusercontent.com/foxihd/hugo-et-hd/master/static/svg/flowlines/22.svg'
alt: 'Guía - Cómo crear una Gentx'
---

# Cómo crear una Gentx

{{< figure src="cover" caption="alt" >}}

## PASO 0. Requisitos previos

### 0-1. Verificar la instalación de `Go` y `jq`

```bash
# Verificar Go (requerido: 1.21+)
go version

# Verificar jq (requerido para scripts)
jq --version
```

Si no están instalados, instálelos primero:
- **Go**: [Guía de instalación de Go](https://go.dev/doc/install)
- **jq**:
- macOS: `brew install jq`
- Linux: `sudo apt-get install jq` o `sudo yum install jq`

---

### 0-2. Clonar el Repositorio Infinito

```bash
clon de git https://github.com/deep-thought-labs/infinite.git

cd infinite

git switch migration
```

---

## PASO 1. Compilar el binario

### 1-1. Construir e instalar el binario

```bash
make install
```

**Propósito:** Compila e instala el binario `infinited`.

**Tiempo estimado:** 2–5 minutos (dependiendo de las dependencias)

Si ocurre un error durante la compilación, instale los paquetes necesarios y reconstruya:

```bash
sudo apt update
sudo apt install -y build-essential pkg-config libssl-dev

cd ~/infinite
make clean
make install
```

---

### Ubicación binaria

El binario se instalará en: `$HOME/go/bin/infinited`

Asegúrese de que `$HOME/go/bin` esté incluido en `PATH` de su sistema.

```bash
# Agregar a PATH (agregar esto a ~/.bashrc o ~/.zshrc)
export PATH=$PATH:$HOME/go/bin

# Verificar
which infinited
infinited version

# Resultado esperado:
# infinito
# versión infinita 0.1.9-...
```

---

## PASO 2. Crear el archivo Génesis

### 2-1. Inicialice la cadena y restaure su cuenta

Para **Red principal**:
```bash
infinited init my-moniker --recover --chain-id infinite_421018-1 --home ~/.infinited
```

> Nota: El ID de la cadena puede actualizarse automáticamente mediante `customize_genesis.sh`, pero aún así debe usar el ID de la cadena inicial correcto para mantener la coherencia.

Reemplace el ID de cadena a continuación si está configurando para Testnet o Creative:

```bash
# Testnet
--chain-id infinite_421018001-1

# Creative
--chain-id infinite_421018002-1
```

Este comando crea un archivo génesis base utilizando los valores predeterminados del SDK de Cosmos (que luego se personalizarán en el siguiente paso).

---

### 2-2. Aplicar personalización de Infinite Drive

**⚠️ Obligatorio:** El génesis generado utiliza los valores predeterminados del SDK de Cosmos.
Debes aplicar la personalización específica de Infinite Drive.

**Red principal:**
```bash
# Ejecutar desde el directorio raíz infinito
./scripts/customize_genesis.sh ~/.infinited/config/genesis.json --network mainnet
```

Para Testnet o Creative, reemplace `--network` según corresponda:

```bash
# Testnet 
--network testnet 

# Creative 
--network creative
```

**Este script realiza:**
- ✅ Establece la denominación correcta de la red para todos los módulos.
- ✅ Agrega metadatos completos del token.
- ✅ Configura precompilaciones de EVM y pares ERC20.
- ✅ Establece parámetros de consenso, staking, acuñación, gobernanza, slashing, mercado de comisiones y distribución.
- ✅ Realiza copias de seguridad automáticas de Génesis.

**Rutas de los archivos de configuración:**
- Red principal: `scripts/genesis-configs/mainnet.json`
- Red de pruebas: `scripts/genesis-configs/testnet.json`
- Red creativa: `scripts/genesis-configs/creative.json`

Estos archivos definen todos los parámetros de la red, como denominaciones, metadatos de tokens, staking, acuñación, gobernanza, slashing y configuraciones de consenso.

---

### 2-3. Crea o recupera tu cuenta y añade fondos

⚠️ Continúe solo si ya tiene un mnemónico almacenado de forma segura.

#### 2-3-1. Recupera tu cuenta desde mnemonic

```bash
infinited keys add validator --recover --keyring-backend file --home ~/.infinited
```

Puede reemplazar "validador" con cualquier nombre de cuenta preferido.

---

#### 2-3-2. Agregar fondos a la cuenta

```bash
# Agregar la cuenta a Génesis con saldo inicial (ejemplo: 100 tokens)
infinited genesis add-genesis-account validator 1000000000000000000000drop \
--keyring-backend file \
--home ~/.infinited
```

**Cantidad:** `10000000000000000000000drop` = 100 Improbabilidad (100 × 10¹⁸)

> Utilice siempre unidades atómicas (10¹⁸) e incluya el sufijo de denominación correcto (drop, tdrop o cdrop).

```bash
# Testnet
tdrop

# Creative
cdrop
```

---

### 2-4. Crear el validador

2-4-1. Generar el validador Gentx (transacción Genesis)

**Red principal:**
```bash
# Crear un gentx para el validador (ejemplo: autodelegar 1 token)
infinited genesis gentx validator 10000000000000000000drop \
--chain-id infinite_421018-1 \
--commission-rate "0.10" \
--commission-max-rate "0.20" \
--commission-max-change-rate "0.01" \
--min-self-delegation "1000000000000000000" \
--keyring-backend file \
--home ~/.infinited
```

Para Testnet y Creative, reemplace las denominaciones y los ID de cadena de la siguiente manera:

```bash
# Testnet
1000000000000000000tdrop
--chain-id infinite_421018001-1

# Creative
1000000000000000000cdrop
--chain-id infinite_421018002-1
--commission-rate "0.01" \
--commission-max-rate "0.05" \
```

**Parámetros:**
- `validator`: Nombre de la cuenta (debe existir en el llavero y tener fondos en Génesis)
- **Importe:** Importe de autodelegación
	- Mainnet: `10000000000000000000drop`
	- Testnet: `10000000000000000000tdrop`
	- Creative: `10000000000000000000cdrop`
- `--chain-id`: Debe coincidir con el ID de la cadena utilizado durante la inicialización
- `--commission-rate`: Tasa de comisión inicial (p. ej., 10% = "0.10")
- `--commission-max-rate`: Tasa de comisión máxima (p. ej., 20% = "0.20")
- `--commission-max-change-rate`: Cambio máximo de la tasa por actualización (p. ej., 1% = "0.01")
- `--min-self-delegation`: Autodelegación mínima (unidades atómicas)

---

#### 2-4-2. Recopilar transacciones de Génesis

```bash
infinited genesis collect-gentxs --home ~/.infinited
```

**⚠️ Importante:**
- La cadena **requiere al menos un validador** en Génesis para empezar a producir bloques.
- Si omites este paso, la cadena comenzará, pero no producirá bloques.
- Todos los validadores deben usar el mismo `--chain-id`.

---

### 2-5. Validar el archivo Génesis

```bash
infinited genesis validate-genesis --home ~/.infinited
```

**Esto verifica:**
- ✅ Consistencia de las denominaciones
- ✅ El suministro total coincide con la suma de todos los saldos
- ✅ La configuración del validador es válida
- ✅ La estructura JSON es correcta

---

### 2-6. Distribuir el archivo Génesis

El `genesis.json` finalizado debe distribuirse a **todos los nodos** antes del lanzamiento de la red.

```bash
scp ~/.infinited/config/genesis.json user@validator1:/path/to/.infinited/config/

scp ~/.infinited/config/genesis.json user@validator2:/path/to/.infinited/config/
```

**⚠️ Importante:**
Todos los nodos deben usar **exactamente el mismo** archivo `genesis.json`.
Incluso una pequeña diferencia puede provocar una bifurcación de la red.

---

## PASO 3. Iniciar la red

**Mainnet**
```bash
infinited start \
  --chain-id infinite_421018-1 \
  --evm.evm-chain-id 421018 \
  --home ~/.infinited
```

**Testnet**
```bash
infinited start \
  --chain-id infinite_421018001-1 \
  --evm.evm-chain-id 421018001 \
  --home ~/.infinited
```

**Creative**
```bash
infinited start \
  --chain-id infinite_421018002-1 \
  --evm.evm-chain-id 421018002 \
  --home ~/.infinited
```

Especifique siempre **ambos** ID:
- `--chain-id`: ID de la cadena Cosmos (formato: `{nombre}_{número}-{versión}`)
- `--evm.evm-chain-id`: ID de la cadena EVM (entero, estándar EIP-155)
