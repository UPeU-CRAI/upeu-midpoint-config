# Repositorio de Gobierno de Identidad de la UPeU

Este repositorio contiene la configuración ("midPoint overlay") para la plataforma de Gobierno de Identidad y Acceso de la Universidad Peruana Unión (UPeU), basada en Evolveum midPoint 4.9.5 LTS.

## Integraciones Clave

Este proyecto gestiona el ciclo de vida de identidades y sus accesos, destacando la integración entre:

*   **Lamb Academic**: Sistema académico (Fuente de autoridad para estudiantes y docentes).
*   **Koha ILS**: Sistema integrado de gestión de bibliotecas (Recurso destino para aprovisionamiento de cuentas).

## Estructura del Proyecto

La estructura sigue las recomendaciones para el uso de midPoint Studio:

*   `/objects`: Objetos XML de midPoint (Recursos, Roles, Arquetipos, etc.)
*   `/docs`: Documentación de arquitectura y guías operativas.

## Requisitos

*   Java 17+
*   Maven 3.8+
*   IntelliJ IDEA con plugin midPoint Studio

## Seguridad

**Importante**: Las credenciales no deben ser almacenadas en este repositorio. Utilice "Encrypted Properties" en midPoint Studio o variables de entorno.