# 📘 Diccionario Técnico: Esquema de Persona UPeU v2.2

Este documento constituye el **Contrato de Identidad** para la Universidad Peruana Unión (UPeU). Define cómo se estructuran los datos de alumnos, docentes y administrativos dentro de midPoint, asegurando la interoperabilidad entre Lamb Academic, Koha y Microsoft Entra ID.

---

## 🏗️ 1. Metadatos del Esquema
Información técnica del objeto dentro del repositorio de midPoint.

* **Nombre:** Esquema de Extensión para Personas UPeU v2.2
* **Namespace:** `urn:upeu:midpoint:person`
* **OID:** `b7d55017-599f-4f2f-9493-9f64bba62c5b`
* **Versión Actual:** 84
* **Estado:** Activo (`active`)

> **Nota de Arquitectura:** Este esquema prioriza el uso de campos nativos (`c:UserType`) para optimizar el rendimiento y utiliza extensiones solo para atributos específicos de la realidad universitaria peruana.

---

## 🏛️ 2. Atributos Nativos (midPoint Core)
Se deben utilizar estos campos antes de recurrir a la extensión para mantener la compatibilidad con las políticas del sistema.

| Atributo | Uso en UPeU | Notas |
| :--- | :--- | :--- |
| `name` | Identificador de Login | Generalmente el correo institucional (UPN). |
| `givenName` | Nombres | Cargados desde el ERP Lamb Academic. |
| `familyName` | Apellidos | Incluye ambos apellidos del usuario. |
| `emailAddress` | Correo Principal | Fuente: Microsoft Entra ID. |
| `employeeNumber`| Código de Personal | Identificador único para planilla y docentes. |
| `employeeType` | Tipo de Usuario | Valores: `Estudiante`, `Docente`, `Administrativo`. |
| `locality` | Sede / Campus | Valores: `Lima`, `Juliaca`, `Tarapoto`. |

---

## 🧬 3. Atributos de Extensión (`up:`)
Campos personalizados definidos en el XML de extensión de la UPeU.

### I. DemographicsType (Datos Demográficos)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `birthDate` | Fecha de Nacimiento | `date` | Formato YYYY-MM-DD. |
| `gender` | Género | `string` | ISO 5218 (1: Masc, 2: Fem, 9: N/A). |
| `country` | País de Residencia | `string` | ISO 3166-1 alpha-3 (Ej: `PER`). |

### II. ContactInfoType (Contacto Adicional)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `secondaryMail` | Correo Secundario | `string` | **Multivalor**. Correos personales de respaldo. |
| `phoneNumberAlt`| Teléfono Alternativo | `string` | Número de contacto secundario. |
| `personalWeb` | Página Personal | `string` | URL de portafolios o CV. |

### III. EmploymentDataType (Datos Laborales)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `hireDate` | Fecha de Ingreso | `date` | Inicio de vínculo con la universidad. |
| `terminationDate`| Fecha de Cese | `date` | Fin de contrato o retiro definitivo. |

### IV. AffiliationDataType (Afiliación)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `primaryAffiliation`| **Afiliación Principal** | `string` | **(Propuesto)** Facultad o Área (Ej: FIA). Resuelve errores de reconciliación. |
| `languageSkills` | Idiomas | `string` | Idiomas dominados por el usuario. |

### V. AcademicStatusType (Estatus Académico)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `studentCycle` | Ciclo Académico | `int` | Ciclo actual (1-12) para lógica de biblioteca. |
| `alumniStatus` | Estado de Egreso | `string` | Situación del graduado (Ej: Bachiller). |

### VI. FederatedIdentityType (Identidad Digital)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `orcid` | ORCID | `string` | ID de investigador. **Indexado**. |

### VII. UniqueIdentifiersType (Identificadores)
| Elemento | Etiqueta | Tipo | Descripción |
| :--- | :--- | :--- | :--- |
| `taxId` | **DNI/CE** | `string` | ID legal en Perú. **Indexado**. |
| `institutionalIdCard`| **ID Institucional** | `string` | Identificador interno propio (UPeU). **Indexado**. |
| `universityIdCard`| **Carnet Minedu** | `string` | ID oficial SUNEDU. **Indexado**. |
| `externalSystemId`| **ID Sistema Externo**| `string` | Clave de integración técnica. **Indexado**. |

---

## ⚠️ 4. Troubleshooting: Error de Reconciliación
Si aparece el error `No target item that would conform to the path extension/primaryAffiliation`:

1. **Causa:** El recurso Lamb Academic envía la facultad, pero el campo no está definido en el esquema.
2. **Solución:** Añadir el elemento `primaryAffiliation` al bloque `AffiliationDataType` del esquema XML v2.2.
