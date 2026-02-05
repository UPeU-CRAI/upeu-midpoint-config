# 📘 Diccionario Técnico: Esquema de Persona UPeU v2.3 (v89)

Este documento es la referencia oficial de identidad para la **Universidad Peruana Unión**. Define el contrato de datos entre Lamb Academic, Microsoft Azure, Koha y midPoint.

---

## 🏗️ 1. Metadatos del Esquema
* **Nombre:** Esquema de Extensión para Personas UPeU v2.3
* **Namespace:** `urn:upeu:midpoint:person`
* **OID:** `b7d55017-599f-4f2f-9493-9f64bba62c5b`
* **Versión Actual:** 89 (Actualizado Feb 2026)
* **Estado:** Activo y Verificado.

---

## 🏛️ 2. Atributos Nativos (midPoint Core)
*Principio: Priorizar campos nativos para optimizar el rendimiento y la compatibilidad.*

| Atributo | Concepto UPeU | Notas |
| :--- | :--- | :--- |
| `name` | **Identificador Único** | UPN de Azure (@upeu.edu.pe). |
| `givenName` | **Nombres** | Fuente oficial: Lamb Academic. |
| `familyName` | **Apellidos** | Paterno y Materno. |
| `emailAddress` | **Correo Oficial** | Sincronizado desde Microsoft Entra ID. |
| `employeeNumber`| **Código RRHH** | Usado para personal y docentes. |
| `employeeType` | **Vínculo** | Estudiante, Docente, Administrativo. |
| `locality` | **Sede / Campus** | Lima, Juliaca, Tarapoto. |

---

## 🧬 3. Atributos de Extensión (`up:`)
Campos personalizados diseñados bajo el principio de **"Una Identidad, Múltiples Roles"**.

### I. AffiliationDataType (Facultad / Unidad)
Esta sección define la "casa" o contrato principal del usuario.

| Elemento | Etiqueta | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `primaryAffiliationCode`| **Siglas Afiliación** | `string` | Código corto indexado para lógica de grupos. | `FIA`, `CRAI` |
| `primaryAffiliationName`| **Nombre Afiliación** | `string` | Nombre oficial completo de la facultad o área. | `Facultad de Ingeniería...` |
| `languageSkills` | **Idiomas** | `string` | Idiomas dominados por el usuario. | `Español, Inglés` |

### II. AcademicStatusType (Estatus y Carrera)
Define el estado del alumno y su programa académico específico.

| Elemento | Etiqueta | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `academicProgram` | **Nombre Carrera** | `string` | Nombre oficial completo del programa. | `Educación Lingüística...` |
| `academicProgramCode`| **Código Programa** | `string` | Sigla o codificación interna (Indexado). | `P23` |
| `studentCycle` | **Ciclo Académico** | `int` | Ciclo actual (1 al 12). | `5` |
| `alumniStatus` | **Estado de Egreso** | `string` | Situación (Ej: Bachiller, Titulado). | `Egresado` |

### III. UniqueIdentifiersType (Identificadores)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `taxId` | **DNI / CE** | `string` | Documento legal peruano. **Indexado**. |
| `universityIdCard`| **Carnet Minedu** | `string` | ID oficial de SUNEDU (Mapeado a `cardnumber`). |
| `institutionalIdCard`| **ID Institucional** | `string` | Identificador interno propio (Reservado). |

### IV. Otros Tipos
* **Demographics**: `birthDate`, `gender`, `country`.
* **ContactInfo**: `secondaryMail` (Multivalor), `phoneNumberAlt`, `personalWeb`.
* **Employment**: `hireDate`, `terminationDate`.
* **Federated**: `orcid` (Indexado).

---

## ⚠️ 4. Control de Cambios y Solución de Errores

### Resolución de Error: SchemaException (`academicProgram`)
* **Problema**: Error `No target item that would conform to the path extension/academicProgram` detectado al reconciliar objetos.
* **Causa**: Discrepancia entre los atributos enviados por el recurso Lamb y la definición del esquema en midPoint.
* **Solución**: Se actualizó el esquema a la **v89**, separando códigos de nombres y declarando explícitamente `academicProgram` y `academicProgramCode`.

### Mejora Conceptual: "Multi-Role"
* Se implementó la separación entre siglas de área (`Code`) para lógica de permisos y nombres descriptivos (`Name`) para visualización. Esto permite que un docente mantenga su identidad académica principal (ej. FIA) mientras desempeña roles adicionales en otras áreas (ej. CRAI).
