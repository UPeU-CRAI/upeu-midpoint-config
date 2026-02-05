# 📘 Diccionario Técnico: Esquema de Persona UPeU v2.4 (v90)

Este documento es la referencia oficial de identidad para la **Universidad Peruana Unión**. Define el contrato de datos entre Lamb Academic, Microsoft Azure, Koha y midPoint.

---

## 🏗️ 1. Metadatos del Esquema
* **Nombre:** Esquema de Extensión para Personas UPeU v2.4
* **Namespace:** `urn:upeu:midpoint:person`
* **OID:** `b7d55017-599f-4f2f-9493-9f64bba62c5b`
* **Versión Actual:** 90 (Actualizado Feb 2026)
* **Estado:** Activo, Probado y Verificado.

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
| `locality` | **Ciudad** | Lima, Juliaca, Tarapoto. |

---

## 🧬 3. Atributos de Extensión (`up:`)
Campos personalizados diseñados bajo el principio de **"Una Identidad, Múltiples Roles"**.

### I. DemographicsType (Demografía)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `birthDate` | **Fecha Nacimiento** | `string` | **Cambio v90**: Texto plano formato "YYYY-MM-DD" para compatibilidad total. |
| `gender` | Género | `string` | ISO 5218 (1:M, 2:F) o texto directo. |
| `country` | País | `string` | ISO 3166-1 alpha-3 (Ej: PER). |

### II. AffiliationDataType (Facultad / Unidad)
Esta sección define la "casa" o contrato principal del usuario.

| Elemento | Etiqueta | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `campus` | **Sede / Campus** | `string` | Campus físico de estudio o trabajo. | `Lima` |
| `primaryAffiliationCode`| **Siglas Afiliación** | `string` | Código corto indexado para lógica de grupos. | `FIA`, `CRAI` |
| `primaryAffiliationName`| **Nombre Afiliación** | `string` | Nombre oficial completo de la facultad o área. | `Facultad de Ingeniería...` |
| `languageSkills` | Idiomas | `string` | Idiomas dominados por el usuario. | `Español, Inglés` |

### III. AcademicStatusType (Estatus y Carrera)
Define el estado del alumno y su programa académico específico.

| Elemento | Etiqueta | Tipo | Descripción | Ejemplo |
| :--- | :--- | :--- | :--- | :--- |
| `academicProgram` | **Nombre Carrera** | `string` | Nombre oficial completo del programa. | `Educación Lingüística...` |
| `academicProgramCode`| **Código Programa** | `string` | Sigla o codificación interna (Indexado). | `P23` |
| `studentCycle` | **Ciclo Académico** | `int` | Ciclo actual (1 al 12). | `5` |
| `alumniStatus` | Estado de Egreso | `string` | Situación (Ej: Bachiller, Titulado). | `Egresado` |

### IV. UniqueIdentifiersType (Identificadores)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `taxId` | **DNI / CE** | `string` | Documento legal peruano. **Indexado**. |
| `universityIdCard`| **Carnet Minedu** | `string` | ID oficial de SUNEDU (Mapeado a `cardnumber`). |
| `institutionalIdCard`| **ID Institucional** | `string` | Identificador interno propio (Reservado). |

---

## ⚠️ 4. Bitácora de Soluciones Técnicas

### Solución: Conflicto de Tipos en Fecha (`birthDate`)
* **Problema:** Error `No type mapping for XSD type date` al importar fechas desde SQL.
* **Causa:** El conector JDBC enviaba `String` ("1996-02-12") y midPoint exigía un objeto `XMLGregorianCalendar` puro sin hora.
* **Solución (v90):** Se redefinió el campo `birthDate` como `xsd:string` en el esquema. Esto eliminó la necesidad de scripts de conversión complejos y aseguró que el dato fluya intacto hacia sistemas externos como Koha.

### Solución: Atributo Faltante (`campus`)
* **Problema:** Error `SchemaException: No target item ... extension/campus`.
* **Solución (v90):** Se añadió el campo `campus` a la sección `AffiliationDataType` para recibir el dato de sede desde Lamb Academic.
