# DavoSMP Fantasy Store

Web estática responsive para la tienda de DavoSMP (Java + Bedrock), con tipografía moderna, fondo fantasy luminoso y sidebar de navegación.

## Abrir en local
Abre `index.html` en un navegador. Para publicar, sube toda la carpeta a un hosting estático (por ejemplo GitHub Pages).

## Antes de publicar
Edita `CONFIG` al final de `index.html`:
- `serverAddress`: IP y puerto reales que deben mostrarse a los jugadores.
- `prices`: precios aprobados.
- `checkout`: URLs oficiales de checkout por producto. Vacío = compra desactivada.
- `accountApi`: URL HTTPS de la API de vinculación. Vacío = vinculación desactivada.

Los precios actuales son ejemplos. La tienda no procesa pagos ni vincula cuentas por sí sola.

## Vinculación segura de cuentas
La interfaz espera un backend en HTTPS con:
- `POST /account-link/start` con `{ platform: "Java"|"Bedrock", playerName: "..." }`
- Respuesta JSON con instrucciones de verificación. El backend debe generar un código aleatorio de un solo uso, con expiración breve, vincularlo al jugador y comprobarlo contra un plugin del servidor.
- El backend debe verificar el UUID/XUID real desde el servidor/plugin; no debe confiar únicamente en el nombre escrito en el navegador.
- `POST /account-link/verify` (a implementar en el backend) debe consumir el código una sola vez y crear la asociación de cuenta.
- Sesiones seguras `HttpOnly`, `Secure`, `SameSite`; protección CSRF, rate limiting, validación de entradas y registros sin tokens/códigos.
- Nunca guardar contraseñas de Microsoft, tokens de acceso en `localStorage`, ni claves privadas del plugin en el frontend.

Para Java se puede añadir inicio de sesión Microsoft en el backend con OAuth/OIDC Authorization Code (y PKCE cuando corresponda), usando bibliotecas oficiales. No es correcto pedir la contraseña en la web.
Para Bedrock en un servidor Java con Geyser/Floodgate, revisar la vinculación que ofrece Floodgate. La asociación específica con la tienda requiere un plugin/API propio o una integración compatible con el proveedor de pagos.

## Pagos
Conecta el checkout de cada producto con el proveedor de pagos y configura la entrega del paquete en el panel/plugin oficial. No pongas secretos de proveedor en JavaScript.
Las llaves de pago deben corresponder a recompensas fijas, descritas antes de pagar. No configurar pagos por resultados aleatorios.
