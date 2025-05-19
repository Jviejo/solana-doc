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

---

Esta guía cubre los aspectos esenciales para empezar a trabajar con Solana, tanto desde la terminal como desde código y desarrollo de smart contracts.
