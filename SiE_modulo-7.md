# Sistema de Información Escolar (SiE-UAM) - Módulo 7: Evaluación Académica
**Autor:** Omar Samuel Hernández Acosta
**Asistente:** Gemini
**Fecha:** 12 de marzo de 2026

---

## AVISO LEGAL Y DE USO ACADÉMICO

**NATURALEZA DEL DOCUMENTO** El presente documento constituye un **ejercicio teórico-académico** de análisis, modelado y diseño de sistemas de información. Su elaboración tiene como único propósito ilustrar la aplicación de metodologías de ingeniería de software en contextos normativos complejos, utilizando como caso de estudio la estructura organizacional y legislativa de la Universidad Autónoma Metropolitana (UAM), utilizando como herramienta las capacidades de procesamiento de la Inteligencia Artificial.

**USO EXCLUSIVAMENTE ACADÉMICO** Queda estrictamente prohibido el uso de este material para fines operativos, comerciales, legales o administrativos reales. **Este documento no representa, ni sustituye, ni oficializa ningún sistema de información vigente** en la Universidad Autónoma Metropolitana ni en ninguna otra institución pública o privada.

---

## Módulo 7: Evaluación Académica

### Propósito y Alcance

**Propósito:** Gestionar de manera integral, segura y auditable el proceso de evaluación del aprendizaje de los alumnos de la UAM. Esto abarca desde la generación de actas de evaluación ordinaria, el registro de calificaciones (MB, B, S, NA, I y equivalencias), la gestión de evaluaciones de recuperación y especiales, hasta los procesos normativos de impugnación y rectificación, garantizando la inmutabilidad y trazabilidad de los registros.

**Alcance Normativo:**
* **RESUAM:** Capítulo V (Evaluación del Aprendizaje, Art. 30-43), Capítulo IX (Evaluaciones, Art. 84-95).
* **Escala de Calificaciones:** MB (10), B (8), S (6), NA (0). Especiales: I (Incompleto - Posgrado), AO, EE, RE, EQ.
* **Integración Operativa:** Completamente alineado con el Submódulo 4.4 de Programación Escolar.

---

### Submódulo 7.1: Gestión de Actas y Roles de Evaluación

La generación y gestión de actas es el proceso de asentar legalmente el rendimiento académico del trimestre.

| Funcionalidad | Descripción | Regla de Negocio (RESUAM) |
|--------------|-------------|--------------------------|
| Generar acta única | Una sola acta por grupo por tipo de evaluación. | Art. 91: Responsabilidad de evaluación. |
| Gestión de Roles en Acta | Diferenciación estricta de funciones para el profesor: Calificar y/o Firmar. | Lineamientos operativos UAM. |
| Roles en Eval. Global | Profesor único califica y firma. Si hay varios sinodales, dividen las funciones. | Lineamientos operativos UAM. |
| Registro de calificaciones | Asentamiento de notas en la escala numérica oficial o letras de equivalencia. | Art. 87: Escala de calificaciones. |
| Estado de Incompleto (I) | Uso exclusivo para alumnos de posgrado. Plazo máximo de 1 trimestre para calificar. | Art. 87: Estado de Incompleto. |
| Firma y Cierre (5 días) | El profesor y secretario académico tienen 5 días hábiles tras la evaluación para firmar. | Art. 91: Plazo de entrega de actas. |

> **Explicación didáctica:** En el diseño de software escolar, un "Acta" no es una simple tabla que se actualiza. Es un **registro inmutable de estado**. Una vez que el profesor la "firma" y "cierra", el sistema debe aplicar un candado transaccional a nivel de Base de Datos. La separación de roles (quién califica vs. quién firma) es vital para el modelado en Enterprise Architect (UML).

---

### Submódulo 7.2: Evaluaciones de Recuperación y Quinta Oportunidad

Gestión de las oportunidades extraordinarias de acreditación, aplicables exclusivamente a nivel licenciatura.

| Funcionalidad | Descripción | Regla de Negocio (RESUAM) |
|--------------|-------------|--------------------------|
| Programar recuperación | Permite hasta 4 recuperaciones por UEA en licenciatura (NO aplica a posgrado). | Art. 89: Límite de recuperaciones. |
| Roles en Recuperación | El Profesor titular califica el examen, pero el **Coordinador del Programa firma el acta**. | Art. 92 RESUAM. |
| Validar pago de evaluación | El sistema verifica el pago ($2.20, $4.20 o $11.00) consultando al Módulo 10 antes de asentar calificación. | Lineamientos del Patronato UAM. |
| Quinta Oportunidad | Conformación de un jurado especial de 3 profesores del área, distintos a evaluaciones previas. | Art. 93: Garantía de imparcialidad. |

---

### Submódulo 7.3: Rectificaciones, Correcciones e Impugnaciones

Procedimientos normativos para alterar una calificación posterior a la firma de un acta.

| Funcionalidad | Descripción | Regla de Negocio (RESUAM) |
|--------------|-------------|--------------------------|
| Corrección Al Alza | Modificación de nota por error administrativo a favor del alumno. No requiere opinión del alumno. | Transparencia de procesos internos. |
| Corrección A La Baja | Modificación por error que perjudica al alumno. **Requiere firma de consentimiento del alumno.** | Transparencia y protección de derechos. |
| Levantar Impugnación | El alumno solicita formalmente la revisión de su Evaluación Global o Recuperación. | Art. 94-95 RESUAM. |
| Jurado por Impugnación | El sistema asigna un jurado que emite un dictamen final (Cambio o Confirmación de nota). | Art. 94-95 RESUAM. |
| Trazabilidad de Actas | Sistemas Escolares genera la nueva acta. Esta debe referenciar siempre de manera transaccional el **Acta de Origen**. | Auditoría y control de Integridad (BD). |

**Flujo de Correcciones e Impugnaciones (Auditoría Técnica):**

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│              FLUJO DE CORRECCIÓN E IMPUGNACIÓN (TRAZABILIDAD)                   │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│  A) CORRECCIÓN DE CALIFICACIÓN (Rectificación por el Profesor)                  │
│     │                                                                           │
│     ├─▶ SI ES AL ALZA (Beneficia al alumno):                                    │
│     │   └─▶ Profesor firma conformidad. (No requiere al alumno)                 │
│     │   └─▶ Se genera Nueva Acta referenciando [Acta_Origen]                    │
│     │                                                                           │
│     └─▶ SI ES A LA BAJA (Perjudica al alumno):                                  │
│         └─▶ Profesor firma conformidad.                                         │
│         └─▶ **Sistema exige Firma de Consentimiento del Alumno.** │
│         └─▶ Se genera Nueva Acta referenciando [Acta_Origen]                    │
│                                                                                 │
│  B) IMPUGNACIÓN (Solicitada por el Alumno)                                      │
│     │                                                                           │
│     └─▶ 1. Alumno levanta impugnación.                                          │
│     └─▶ 2. Sistema conforma Jurado (3 Profesores).                              │
│     └─▶ 3. Jurado emite Dictamen (Cambio o Confirmación).                       │
│     └─▶ 4. Sistemas Escolares asienta nota en Nueva Acta.                       │
│     └─▶ 5. Se liga la transacción al [Acta_Origen] mediante FK.                 │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

```

### Variables de Datos Críticas del Módulo 7 (Alineadas a AGA)

El siguiente diccionario de datos servirá como base para el diagrama de clases en Enterprise Architect.

| Variable / Campo | Descripción | Tipo | Restricción / Valores |
|------------------|-------------|------|-----------------------|
| **Clave_Acta** | ID único del acta generada | Alfanumérico | PK (Llave primaria) |
| **Clave_UEA** | Identificador de la materia | Numérico | FK (Llave Foránea a Módulo 2) |
| **Tipo_Evaluacion** | Modalidad | Carácter | Global / Recuperacion / Quinta_Oportunidad |
| **Rol_Profesor** | Función en el acta | Carácter | Calificar / Firmar / Ambos |
| **Firma_Coordinador**| Firma en acta de recuperación | Carácter | S/N (Solo aplica en Recuperación) |
| **Acta_Origen** | Rastro de auditoría | Alfanumérico | Referencia a FK del acta original si hay cambio |
| **Calificacion** | Nota asignada | Carácter | MB, B, S, NA, AO, EE, RE, EQ, I |
| **Firma_Consentimiento**| Aprobación del alumno | Carácter | S/N (Requerido en corrección a la baja) |
| **Dictamen_Impugnacion**| Resultado del jurado | Carácter | Cambio / Confirmacion |
| **Estado_Acta** | Ciclo de vida | Numérico | 1=Abierta, 2=Firma_Pendiente, 3=Cerrada |

---

### Estados del Ciclo de Vida (Lifecycle) para UML

Para modelar los Diagramas de Máquina de Estados en EA:

* **Ciclo de vida del Acta:**
  `Borrador (Abierta)` → `En_Firma (Pendiente de roles)` → `Firmada_Cerrada` → `Rectificada/Impugnada (Opcional)`
* **Ciclo de vida de la Calificación (Detalle):**
  `Capturada` → `Validada_Escala` → `Asentada` → `Histórica_Kardex`

---

### Integración con Otros Módulos

El Módulo de Evaluación funciona como un hub central que consume y exporta datos al resto de la arquitectura.

| Módulo Origen/Destino | Punto de Integración | Datos Intercambiados |
|-----------------------|---------------------|---------------------|
| **Módulo 4: Programación** | Base operativa de entrada | Recepción de listas de grupos, validación de sinodales, áreas de conocimiento para jurados. |
| **Módulo 10: Pagos** | Prerrequisito de Recuperaciones | Verifica cuotas pagadas ($2.20 / $4.20 / $11.00) antes de habilitar captura de calificación. |
| **Módulo 3: Alumnos** | Modificación de Estatus | Si el alumno acumula NA, el módulo de evaluación dispara alertas de posible pérdida de calidad de alumno. |
| **Módulo 8: Kardex** | Asentamiento final (Salida) | Envía el estatus final de las actas cerradas para actualizar créditos y promedios históricos. |

---

### Conclusión del Módulo 7

El **Módulo de Evaluación Académica** representa el núcleo transaccional más sensible del SiE-UAM. A nivel de ingeniería de software, su mayor reto no es almacenar una simple letra (MB, B, S, NA), sino gestionar de forma robusta la **concurrencia, la seguridad de roles y la trazabilidad legal**. 

La separación arquitectónica de roles (Calificador vs. Firmante), la intervención obligatoria del Coordinador en procesos de recuperación, y los estrictos controles de auditoría para correcciones e impugnaciones (asegurando el vínculo FK con el `Acta_Origen`) garantizan que el sistema tecnológico soporte y haga cumplir rigurosamente la normatividad estipulada en los Capítulos V y IX del RESUAM.
