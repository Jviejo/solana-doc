# Guía Básica de Solana

## 1. Comandos básicos de Solana CLI

Instalación:
```bash
sh -c "$(curl -sSfL https://release.solana.com/v1.18.14/install)"
```

Verifica la instalación:
```bash
solana --version
```

Comandos útiles:
```bash
solana config get                # Ver configuración actual
solana config set --url https://api.devnet.solana.com   # Cambiar red
solana balance                   # Ver balance de la wallet configurada
solana airdrop 1                 # Solicitar 1 SOL (solo en devnet/testnet)
solana address                   # Ver dirección pública
solana keygen new                # Crear nueva wallet
solana transfer <DESTINO> <CANTIDAD>   # Enviar SOL
solana transaction-history       # Ver historial de transacciones
```

## 2. Comandos básicos de SPL Token CLI

Instalación:
```bash
cargo install spl-token-cli
```

Comandos útiles:
```bash
spl-token create-token           # Crear un nuevo token
spl-token create-account <TOKEN> # Crear cuenta asociada a un token
spl-token mint <TOKEN> <CANTIDAD> <CUENTA> # Mintear tokens
spl-token transfer <TOKEN> <CANTIDAD> <DESTINO> # Transferir tokens
spl-token accounts               # Ver cuentas de tokens
spl-token balance <TOKEN>        # Ver balance de un token
```

## 3. Acceso básico a Solana con TypeScript

Instala dependencias:
```bash
npm install @solana/web3.js
```

Ejemplo mínimo:
```typescript
import { Connection, PublicKey, clusterApiUrl } from "@solana/web3.js";

const connection = new Connection(clusterApiUrl('devnet'));
const publicKey = new PublicKey('TU_DIRECCION_PUBLICA');

(async () => {
  const balance = await connection.getBalance(publicKey);
  console.log(`Balance: ${balance / 1e9} SOL`);
})();
```

## 4. Solana Test Validator

Para correr un nodo local de Solana para pruebas:
```bash
solana-test-validator
```
Esto inicia una red local en tu máquina para desarrollo y testing.

## 5. Programa mínimo en Anchor

Instala Anchor:
```bash
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install latest
avm use latest
```

Ejemplo mínimo de programa en `programs/mi_programa/src/lib.rs`:
```rust
use anchor_lang::prelude::*;

#[program]
pub mod mi_programa {
    use super::*;
    pub fn initialize(ctx: Context<Initialize>) -> Result<()> {
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize {}
```

## 6. Programa mínimo en Rust nativo (sin Anchor)

Ejemplo mínimo:
```rust
use solana_program::{account_info::AccountInfo, entrypoint, entrypoint::ProgramResult, pubkey::Pubkey};

entrypoint!(process_instruction);

fn process_instruction(
    _program_id: &Pubkey,
    _accounts: &[AccountInfo],
    _instruction_data: &[u8],
) -> ProgramResult {
    Ok(())
}
```

## 7. Wallets de Solana

- **Sollet**: Extensión web, fácil de usar para desarrolladores.
- **Phantom**: Muy popular, interfaz amigable, integración con dApps.
- **Solflare**: Web, móvil y extensión, soporte para staking.
- **Backpack**: Wallet avanzada con soporte para xNFTs.
- **CLI Wallet**: Usando `solana-keygen` y archivos de clave.

**Recomendación:** Para desarrollo, puedes usar la wallet CLI (`solana-keygen new`) o Phantom para pruebas en devnet.

## 8. Explorers de Solana

Puedes visualizar transacciones, cuentas y programas en los explorers oficiales de Solana:

- **Mainnet Beta:** [https://explorer.solana.com](https://explorer.solana.com)
- **Devnet:** [https://explorer.solana.com?cluster=devnet](https://explorer.solana.com?cluster=devnet)
- **Testnet:** [https://explorer.solana.com?cluster=testnet](https://explorer.solana.com?cluster=testnet)

Solo debes pegar la dirección de una cuenta, transacción o programa en la barra de búsqueda del explorer.

## 9. Cambiar configuración de red en Solana CLI

Para cambiar la red a la que apuntas con la CLI:

```bash
solana config set --url https://api.devnet.solana.com   # Devnet
solana config set --url https://api.testnet.solana.com  # Testnet
solana config set --url https://api.mainnet-beta.solana.com # Mainnet Beta
```

Verifica la configuración actual:
```bash
solana config get
```

Esto te mostrará la URL de la red, la wallet activa y otros parámetros importantes.

## 10. Anchor: Framework para Solana

### Instalación de Anchor

Anchor es el framework más popular para desarrollar smart contracts en Solana.

```bash
cargo install --git https://github.com/coral-xyz/anchor avm --locked --force
avm install latest
avm use latest
npm install -g @coral-xyz/anchor-cli
```

### Crear un nuevo proyecto Anchor

```bash
anchor init mi_programa
cd mi_programa
```

### Ejemplo mínimo de programa en Anchor (`programs/mi_programa/src/lib.rs`)

```rust
use anchor_lang::prelude::*;

#[program]
pub mod mi_programa {
    use super::*;
    pub fn initialize(ctx: Context<Initialize>) -> Result<()> {
        Ok(())
    }
}

#[derive(Accounts)]
pub struct Initialize {}
```

### Build y deploy local

Compila el programa y lanza el test-validator:

```bash
anchor build
anchor test --skip-local-validator
```

Para desplegar en local:
```bash
anchor deploy
```

### Invocación desde TypeScript

Instala dependencias:
```bash
npm install @coral-xyz/anchor @solana/web3.js
```

Ejemplo mínimo de invocación:
```typescript
import * as anchor from "@coral-xyz/anchor";

const provider = anchor.AnchorProvider.env();
anchor.setProvider(provider);

const program = await anchor.Program.at(
  require("../target/idl/mi_programa.json"),
  new anchor.web3.PublicKey("<DIRECCION_DEL_PROGRAMA>"),
  provider
);

await program.methods.initialize().rpc();
console.log("Programa invocado correctamente");
```

- Cambia `<DIRECCION_DEL_PROGRAMA>` por la dirección real del programa desplegado.
- El archivo `mi_programa.json` se genera en `target/idl/` tras compilar con Anchor.

## 11. Explorer Custom y Configuración Custom en Solana CLI

### Usar un Explorer Custom

Puedes usar cualquier explorer compatible con Solana, por ejemplo:

- **Solscan:** [https://solscan.io/](https://solscan.io/)
- **SolanaFM:** [https://solana.fm/](https://solana.fm/)
- **Solana Beach:** [https://solanabeach.io/](https://solanabeach.io/)

Para ver una cuenta, transacción o programa, solo pega la dirección en la barra de búsqueda del explorer que prefieras. Muchos explorers permiten elegir la red (mainnet, devnet, testnet) desde su interfaz.

### Configuración Custom en Solana CLI

Puedes apuntar la CLI a un endpoint RPC personalizado (por ejemplo, un nodo privado o un RPC de pago):

```bash
solana config set --url https://mi-rpc-personalizado.com
```

Para cambiar la wallet por defecto:
```bash
solana config set --keypair /ruta/a/mi/keypair.json
```

Para ver toda la configuración actual:
```bash
solana config get
```

**Nota:** La CLI de Solana no permite configurar un explorer custom para los comandos, pero puedes copiar cualquier dirección o hash y pegarlo en el explorer de tu preferencia.

## 12. Estructura de una Cuenta, PDA, Bump, Transacción e Instrucción en Solana

### Estructura de una Cuenta en Solana

Una cuenta en Solana contiene:
- **Public Key:** Identificador único de la cuenta.
- **Lamports:** Balance de SOL (1 SOL = 1e9 lamports).
- **Owner:** Programa que controla la cuenta.
- **Data:** Espacio de almacenamiento arbitrario (usado por programas).
- **Executable:** Si la cuenta es un programa.
- **Rent Epoch:** Epoch hasta el que la cuenta está rent-exempt.

Ejemplo en TypeScript:
```typescript
const accountInfo = await connection.getAccountInfo(publicKey);
console.log(accountInfo);
```

### PDA (Program Derived Address) y Bump

- **PDA:** Dirección especial generada por un programa, que no tiene clave privada y solo puede ser firmada por el programa propietario.
- **Bump:** Número (u8) que se usa para encontrar una dirección válida (no colisiona con ninguna clave privada real).

Ejemplo de derivación de PDA en Anchor:
```rust
let (pda, bump) = Pubkey::find_program_address(&[b"seed", user.key().as_ref()], program_id);
```
En Anchor, se usa así en la macro:
```rust
#[account(
    seeds = [b"seed", user.key().as_ref()],
    bump
)]
pub pda_account: Account<'info, MiCuenta>,
```

### Estructura de una Transacción

Una transacción en Solana contiene:
- **Signatures:** Firmas de los participantes.
- **Message:**
  - **Account Keys:** Cuentas involucradas.
  - **Recent Blockhash:** Hash reciente para evitar replay.
  - **Instructions:** Lista de instrucciones a ejecutar.

Ejemplo en TypeScript:
```typescript
import { Transaction, SystemProgram } from "@solana/web3.js";

const tx = new Transaction().add(
  SystemProgram.transfer({
    fromPubkey: sender,
    toPubkey: receiver,
    lamports: 1_000_000,
  })
);
```

### Estructura de una Instrucción

Una instrucción contiene:
- **Program ID:** Programa a invocar.
- **Accounts:** Cuentas que participan (con flags de lectura/escritura/firma).
- **Data:** Datos serializados (parámetros de la instrucción).

Ejemplo en TypeScript:
```typescript
import { TransactionInstruction } from "@solana/web3.js";

const instruction = new TransactionInstruction({
  keys: [
    { pubkey: account1, isSigner: false, isWritable: true },
    { pubkey: account2, isSigner: true, isWritable: false },
  ],
  programId: myProgramId,
  data: Buffer.from([]), // datos serializados
});
```

### Formato de la Transacción (Serialización)

Las transacciones se serializan en base64 para ser enviadas a la red. El formato incluye:
- Número de firmas
- Firmas
- Cuentas
- Blockhash
- Instrucciones (programId, cuentas, datos)

Puedes ver el formato serializado usando:
```typescript
const serialized = tx.serialize().toString('base64');
console.log(serialized);
```

## 13. Logs y Events en Solana

### Logs en Solana

Los programas pueden emitir logs para depuración usando macros especiales:

- En Rust nativo:
```rust
msg!("Hola desde el programa");
```
- En Anchor:
```rust
msg!("Valor recibido: {}", valor);
```
Estos logs se pueden ver al ejecutar transacciones (por ejemplo, usando `solana logs` o en el output de Anchor CLI).

### Events en Anchor

Anchor permite definir eventos que se emiten durante la ejecución del programa y pueden ser leídos por clientes:

#### Definir un evento en Anchor
```rust
#[event]
pub struct MiEvento {
    pub user: Pubkey,
    pub cantidad: u64,
}

// Emitir el evento en una instrucción:
emit!(MiEvento {
    user: *ctx.accounts.user.key,
    cantidad: 42,
});
```

#### Leer logs y eventos desde TypeScript

Puedes obtener los logs de una transacción así:
```typescript
const tx = await program.methods.miMetodo().rpc();
const txInfo = await connection.getTransaction(tx, { commitment: "confirmed" });
console.log(txInfo.meta.logMessages);
```

Para parsear eventos de Anchor:
```typescript
const events = program._events.parseLogs(txInfo.meta.logMessages);
console.log(events);
```

- Los eventos aparecen en los logs como líneas con el prefijo `Program log: EVENT_JSON`.
- Anchor provee utilidades para parsear estos eventos automáticamente.

---

Esta guía cubre los aspectos esenciales para empezar a trabajar con Solana, tanto desde la terminal como desde código y desarrollo de smart contracts.
