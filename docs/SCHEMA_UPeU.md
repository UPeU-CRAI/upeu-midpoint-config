# 📘 Diccionario Técnico: Esquema de Persona UPeU v2.2 (v88)

Este documento es la referencia oficial de identidad para la **Universidad Peruana Unión**. Define el contrato de datos entre Lamb Academic, Microsoft Azure, Koha y midPoint.

---

## 🏗️ 1. Metadatos del Esquema
* **Nombre:** Esquema de Extensión para Personas UPeU v2.2
* **Namespace:** `urn:upeu:midpoint:person`
* **OID:** `b7d55017-599f-4f2f-9493-9f64bba62c5b`
* **Versión Actual:** 88 (Actualizado Feb 2026)
* **Estado:** Activo y Verificado.

---

## 🏛️ 2. Atributos Nativos (midPoint Core)
*Principio: Priorizar campos nativos para optimizar el rendimiento y la compatibilidad.*

| Atributo | Concepto UPeU | Notas |
| :--- | :--- | :--- |
| `name` | **Identificador Único** | UPN de Azure (@upeu.edu.pe). |
| `givenName` | **Nombres** | Fuente oficial: Lamb Academic. |
| `familyName` | **Apellidos** | Paterno y Materno. |
| `emailAddress` | **Correo Institucional** | Sincronizado desde Microsoft Entra ID. |
| `employeeNumber`| **Código RRHH** | Usado para personal y docentes. |
| `employeeType` | **Vínculo** | Estudiante, Docente, Administrativo. |
| `locality` | **Sede / Campus** | Lima, Juliaca, Tarapoto. |

---

## 🧬 3. Atributos de Extensión (`up:`)
Campos personalizados para la gestión académica y administrativa.

### I. AffiliationDataType (Facultad y Carrera)
*Esta sección fue actualizada en la v88 para resolver conflictos de reconciliación.*

| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `primaryAffiliation`| **Facultad / Área** | `string` | Unidad principal (Ej: FIA, FACS). |
| `academicProgram` | **Carrera** | `string` | Programa de estudio específico. |
| `languageSkills` | Idiomas | `string` | Idiomas dominados por el usuario. |

### II. AcademicStatusType (Estatus Estudiantil)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `studentCycle` | **Ciclo Académico** | `int` | Ciclo actual (1 al 12). |
| `alumniStatus` | Estado de Egreso | `string` | Situación (Ej: Bachiller, Titulado). |

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

### Resolución de Error: SchemaException
* **Problema:** Error `No target item ... extension/primaryAffiliation` detectado en Feb 2026.
* **Causa:** El recurso Lamb enviaba datos de facultad sin destino en el esquema.
* **Solución:** Se actualizó el esquema a la **v88** integrando formalmente los campos `primaryAffiliation` y `academicProgram`.
