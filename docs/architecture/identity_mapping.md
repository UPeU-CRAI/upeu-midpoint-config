# Mapeo de Identidad e Interoperabilidad

Esta tabla describe la transformación de atributos entre el esquema institucional y los sistemas conectados.

| Atributo Origen (Esquema UPeU v2.2) | Atributo Destino (Koha ILS) | Descripción | Transformación |
| :--- | :--- | :--- | :--- |
| `institutionalIdCard` | `userid` | Identificador único del usuario | Copia directa (Direct mapping) |
| `givenName` | `firstname` | Nombres | - |
| `familyName` | `surname` | Apellidos | - |
| `emailAddress` | `email` | Correo institucional | - |

## Notas
*   El `userid` en Koha es crítico para la autenticación y debe coincidir exactamente con el ID usado en la tarjeta institucional.
