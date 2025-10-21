KipuBankV2 – OpenZeppelin Edition

Evolucioné el KipuBank original a una versión más cercana a producción: soporte multi-token (ETH + ERC-20), roles con AccessControl, valuación en USD(6) via Chainlink y cap global del banco en USD. Agregué límites por retiro por token, eventos y errores personalizados, y protecciones (ReentrancyGuard, SafeERC20, patrón checks-effects-interactions).

Mejoras clave

Control de acceso: DEFAULT_ADMIN_ROLE y ROLE_ADMIN para configurar tokens y parámetros sin exponer llaves.

Multi-token: address(0) representa ETH; para ERC-20 se guardan decimals, withdrawLimit y priceFeed.

Oracle & contabilidad: _toUsd() convierte montos a USD con 6 decimales usando Chainlink (TOKEN/USD) y controla bankCapUsd6 (immutable).

Seguridad: ReentrancyGuard, SafeERC20, errores custom (gas-efficient) y eventos trazables (Deposit, Withdraw, TokenConfigured).

Decisiones de diseño:

Normalizo todo a USD(6) (estilo USDC) para simplificar contabilidad multi-decimales.

bankCapUsd6 inmutable: coherencia y menos superficie de error.

address(0) como ETH evita código duplicado.

Prioricé claridad + seguridad antes que micro-optimizaciones.


Parámetros típicos del constructor:

const bankCapUsd6 = 10000000000; // Cap global = 10 000 USD
const ethPriceFeed = "0x694AA1769357215DE4FAC081bf1f309aDC325306"; // Chainlink ETH/USD Sepolia
const ethWithdrawLimit = 50000000000000000


Uso rápido

Depositar ETH: depositEth() enviando msg.value > 0.

Depositar ERC-20: approve al contrato y luego depositToken(token, amount).

Retirar: withdrawEth(amount) / withdrawToken(token, amount) (respeta withdrawLimit).

Consultar saldo: getBalance(token) y getUsdBalance(token, user).

Dirección desplegada

Red: Sepolia

Contrato: 0x81032740b2d5f6805ccff49896b97dfdb29f3561

Etherscan: buscá la dirección arriba (código verificado cuando corras el paso de verify).
