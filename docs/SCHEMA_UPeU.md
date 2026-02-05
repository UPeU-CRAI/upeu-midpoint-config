# 📘 Diccionario Técnico: Esquema de Persona UPeU v2.2

Este documento es la referencia oficial para la estructura de identidad en la **Universidad Peruana Unión**. Define el mapeo entre los sistemas fuente (Lamb Academic, Azure) y los destinos (Koha, Keycloak).

---

## 🏗️ 1. Metadatos del Esquema
* **Nombre:** Esquema de Extensión para Personas UPeU v2.2
* **Namespace:** `urn:upeu:midpoint:person`
* **OID:** `b7d55017-599f-4f2f-9493-9f64bba62c5b`
* **Versión:** 84

---

## 🏛️ 2. Atributos Nativos (midPoint Core)
*Principio: Usar primero lo nativo para garantizar compatibilidad y rendimiento.*

| Atributo | Concepto UPeU | Notas |
| :--- | :--- | :--- |
| `name` | **Login Único** | UPN de Microsoft Azure (@upeu.edu.pe). |
| `givenName` | **Nombres** | Fuente: Lamb Academic / RRHH. |
| `familyName` | **Apellidos** | Paterno y Materno concatenados. |
| `emailAddress` | **Correo Oficial** | Correo institucional gestionado en Entra ID. |
| `employeeNumber`| **Código Planilla** | Para personal docente y administrativo. |
| `employeeType` | **Vínculo** | Estudiante, Docente, Administrativo, Tercero. |
| `locality` | **Sede** | Lima, Juliaca, Tarapoto. |

---

## 🧬 3. Atributos de Extensión (`up:`)
Campos personalizados para la realidad académica de la UPeU.

### I. DemographicsType (Demografía)
| Elemento | Etiqueta | Descripción |
| :--- | :--- | :--- |
| `birthDate` | Fecha Nacimiento | Formato YYYY-MM-DD. |
| `gender` | Género | ISO 5218 (1:M, 2:F, 9:N/A). |
| `country` | País | ISO 3166-1 alpha-3 (Ej: PER). |

### II. ContactInfoType (Contacto)
| Elemento | Etiqueta | Descripción |
| :--- | :--- | :--- |
| `secondaryMail` | Correo Personal | **Multivalor**. Correos externos (@gmail, etc). |
| `phoneNumberAlt`| Teléfono Alt. | Número de celular o casa de respaldo. |

### III. Academic & Affiliation (Académico y Facultad)
| Elemento | Etiqueta | Descripción |
| :--- | :--- | :--- |
| `primaryAffiliation`| **Facultad/Área** | [cite_start]**(Necesario)** Unidad principal (Ej: FIA, FACS)[cite: 5, 8]. |
| `academicProgram` | **Carrera** | **(Propuesto)** Programa de estudio (Ej: Ing. Sistemas). |
| `studentCycle` | **Ciclo** | Ciclo académico actual (1-12). |
| `alumniStatus` | **Egreso** | Situación (Ej: Bachiller, Titulado). |

### IV. Identificadores (Únicos)
| Elemento | Etiqueta | Descripción |
| :--- | :--- | :--- |
| `taxId` | **DNI/CE** | Documento de identidad legal. **Indexado**. |
| `universityIdCard`| **Carnet Minedu** | ID oficial de SUNEDU (Mapeado a `cardnumber`). |
| `orcid` | **ORCID** | ID de investigación para docentes. **Indexado**. |

---

## ⚠️ 4. Bitácora de Errores y Soluciones

### Error de Reconciliación (Atributo no encontrado)
* [cite_start]**Error:** `No target item ... extension/primaryAffiliation`[cite: 5, 8, 43].
* [cite_start]**Causa:** Lamb envía la facultad pero el esquema XML no tiene el campo definido[cite: 30, 31].
* **Acción:** Actualizar el archivo de esquema XML en midPoint Studio añadiendo el elemento `primaryAffiliation`.
