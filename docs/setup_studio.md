# Configuración de midPoint Studio

Pasos para conectar midPoint Studio a la instancia de desarrollo de la UPeU.

## Conexión al Servidor

1.  Abrir IntelliJ IDEA y navegar a la pestaña **MidPoint**.
2.  Hacer clic en el icono **+** (Add Environment).
3.  Configurar los parámetros:
    *   **Name**: `UPeU Dev 192.168.15.230`
    *   **URL**: `https://192.168.15.230/midpoint`
    *   **Username**: `administrator`
    *   **Password**: *(Solicitar credenciales al administrador de sistemas)*
4.  Activar la opción **Ignore SSL errors** si el certificado es autofirmado.
5.  Hacer clic en **Test Connection**. Si es exitoso, guardar con **OK**.

## Buenas Prácticas

*   Al trabajar, siempre descargar ("Download") el objeto antes de editar para evitar conflictos.
*   Utilizar "Cleanup File" antes de comitear para remover metadatos operativos (id, version, operationalState).
