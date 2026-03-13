Sistema de Información Escolar (SiE-UAM) - Módulo 7: Evaluación
Autor: Omar Samuel Hernández Acosta
Asistente: Gemini
Fecha: 12 de marzo de 2026

AVISO LEGAL Y DE USO ACADÉMICO

NATURALEZA DEL DOCUMENTO
El presente documento constituye un ejercicio teórico-académico de análisis, modelado y diseño de sistemas de información. Su elaboración tiene como único propósito ilustrar la aplicación de metodologías de ingeniería de software en contextos normativos complejos, utilizando como caso de estudio la estructura organizacional y legislativa de la Universidad Autónoma Metropolitana (UAM), utilizando como herramienta las capacidades de procesamiento de la Inteligencia Artificial, aprovechando la experiencia laboral del autor en la industria del software.

USO EXCLUSIVAMENTE ACADÉMICO
Queda estrictamente prohibido el uso de este material para fines operativos, comerciales, legales o administrativos reales. Este documento no representa, ni sustituye, ni oficializa ningún sistema de información vigente en la Universidad Autónoma Metropolitana ni en ninguna otra institución pública o privada.

DESCARGO DE RESPONSABILIDAD

Sin Validez Legal: Las interpretaciones normativas contenidas en este modelo no constituyen asesoría jurídica ni sustituyen la consulta directa de los ordenamientos oficiales (Reglamento de Estudios Superiores, Ley Orgánica, etc.). En caso de discrepancia, prevalece siempre la legislación universitaria vigente publicada en el Semanario de la UAM.
Precisión de la Información: Aunque se ha procurado la fidelidad a los documentos fuente disponibles públicamente, el modelo puede contener simplificaciones, abstracciones o errores inherentes a un ejercicio pedagógico. No se garantiza la exactitud absoluta para la implementación técnica real.
Propiedad Intelectual: La normatividad universitaria citada es propiedad de la UAM y se utiliza bajo el principio de transparencia y acceso a la información pública. El modelo de arquitectura, diagramas y glosarios derivados son producto de este ejercicio académico específico por el autor, con ayuda de un asistente de AI.
CONFIDENCIALIDAD Y DATOS
Este documento no contiene datos personales sensibles de alumnos, profesores o personal administrativo. Toda la información referida a procesos, estados y catálogos es de carácter público e institucional.

VIGENCIA
Este material refleja el estado de la normatividad y los procesos institucionales conocidos hasta la fecha de su elaboración. Los cambios legislativos posteriores no están reflejados en este modelo estático.
Módulo 7: Evaluación Académica
Propósito y Alcance

Propósito: Gestionar de manera integral, segura y auditable el proceso de evaluación del aprendizaje de los alumnos de la UAM. Esto abarca desde la generación de actas de evaluación ordinaria, el registro de calificaciones (MB, B, S, NA), la gestión de evaluaciones de recuperación y especiales, hasta los procesos normativos de rectificación de calificaciones, garantizando la inmutabilidad de los registros una vez firmados.

Alcance Normativo:

RESUAM: Capítulo V (Evaluación del Aprendizaje, Art. 30-43), Capítulo VI (Rectificación de Calificaciones).

Escala de Calificaciones UAM: Muy Bien (MB), Bien (B), Suficiente (S), No Acreditado (NA).

Tipos de Evaluación: Ordinaria, Recuperación, Curricular/Especial.

Submódulo 7.1: Gestión de Actas (Evaluación Ordinaria)

Funcionalidad	Descripción	Regla de Negocio (RESUAM)
Generar acta de evaluación	Creación del documento digital con la lista de alumnos inscritos en un grupo/UEA.	Art. 32: La evaluación se asienta en actas oficiales.
Capturar calificaciones	Registro de la calificación final obtenida por cada alumno en la escala oficial de la UAM.	Art. 35: Escala MB, B, S, NA.
Validar captura completa	El sistema no permite cerrar el acta si existen alumnos sin calificación asignada (evita espacios en blanco).	Control de integridad de datos.
Firma electrónica de actas	Autorización definitiva del acta por parte del profesor titular mediante credenciales seguras.	Art. 32: El profesor es responsable de asentar y firmar.
Cierre y candado de acta	Bloqueo inmutable del acta una vez firmada. Cualquier cambio posterior requiere el submódulo 7.3.	Seguridad y auditoría de la base de datos.
Notificación automática	Al firmar el acta, las calificaciones acreditadas y no acreditadas pasan inmediatamente al Kardex temporal del alumno.	Transparencia y sincronización con Módulo 8.
Explicación didáctica: En el diseño de software escolar, un "Acta" no es una simple tabla que se actualiza. Es un registro inmutable de estado. Una vez que el profesor la "firma" y "cierra", el sistema debe aplicar un candado transaccional. Si el profesor cometió un error tipográfico y puso "NA" en lugar de "MB", el sistema no le permitirá simplemente "editar" el registro; lo forzará a usar el proceso de Rectificación, manteniendo un rastro de auditoría completo.

Submódulo 7.2: Evaluaciones Especiales y Recuperación

Funcionalidad	Descripción	Regla de Negocio (RESUAM)
Solicitar evaluación de recuperación	Permite al alumno que obtuvo "NA" o no presentó la evaluación ordinaria inscribirse a la recuperación.	Art. 36 y 37: Derecho a recuperación.
Validar límite de oportunidades	Verifica cuántas veces ha presentado el alumno la UEA en recuperación.	Art. 38: Límite de oportunidades reglamentarias.
Generar actas de recuperación	Agrupa a los alumnos solicitantes en actas específicas para esta modalidad, independientes del grupo original.	Organización de la programación escolar.
Gestionar evaluación de saberes previamente adquiridos	Proceso excepcional para alumnos que demuestran dominio de la UEA sin haberla cursado ordinariamente.	Art. 40: Evaluaciones por acreditación de saberes.
Gestionar evaluación curricular (Especial)	Para alumnos a los que les falte una sola UEA para terminar los créditos de su plan de estudios.	Art. 42: Última oportunidad antes de la baja definitiva.
Flujo de Evaluación de Recuperación:

┌─────────────────────────────────────────────────────────────────────────────────┐
│                    PROCESO DE EVALUACIÓN DE RECUPERACIÓN                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐              │
│  │ Alumno obtiene  │───▶│ Apertura de     │───▶│ Sistema valida  │              │
│  │ NA en Ordinaria │    │ periodo de      │    │ límite de       │              │
│  │                 │    │ Recuperación    │    │ oportunidades   │              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘              │
│                                                         │                       │
│                                                         ▼                       │
│  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐              │
│  │ Firma y Cierre  │◀───│ Captura de      │◀───│ Generación de   │              │
│  │ de Acta por el  │    │ Calificación    │    │ Acta de         │              │
│  │ Profesor        │    │ (MB, B, S, NA)  │    │ Recuperación    │              │
│  └─────────────────┘    └─────────────────┘    └─────────────────┘              │
│           │                                                                     │
│           ▼                                                                     │
│  ┌─────────────────┐                                                            │
│  │ Envío a Kardex  │                                                            │
│  │ (Módulo 8)      │                                                            │
│  └─────────────────┘                                                            │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

Explicación didáctica: Las evaluaciones especiales y de recuperación manejan su propio ciclo de vida. Un error común al programar esto es mezclar los identificadores de actas. El SiE-UAM debe generar un ID_Acta completamente nuevo para la evaluación de recuperación, vinculándolo al alumno, pero desvinculándolo del grupo ordinario original (ya que un examen de recuperación puede agrupar a estudiantes de diferentes profesores).

Submódulo 7.3: Rectificación de Calificaciones

Funcionalidad	Descripción	Regla de Negocio (RESUAM)
Levantar solicitud de rectificación	El profesor titular inicia un trámite formal para corregir una calificación previamente firmada en un acta.	Procedimiento normativo post-acta.
Validar vigencia temporal	El sistema permite rectificaciones ágiles si se hacen dentro del plazo marcado por el calendario escolar.	Calendario Escolar UAM.
Gestionar justificación académica	Captura del motivo por el cual se solicita el cambio (ej. error de captura, revisión de examen).	Transparencia de procesos.
Flujo de aprobación jerárquica	Si la solicitud está fuera de tiempo ordinario, el sistema la enruta para autorización del Director de División o Consejo Divisional.	Controles y balances institucionales.
Ejecutar rectificación en BD	Una vez aprobada, el sistema actualiza la calificación, insertando un registro en la tabla de auditoría (logs).	Integridad referencial.
Flujo de Rectificación (Auditoría Técnica):

┌─────────────────────────────────────────────────────────────────────────────────┐
│                   FLUJO DE APROBACIÓN PARA RECTIFICACIÓN                        │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                                 │
│ ┌────────────────┐    ┌────────────────┐                                        │
│ │ Profesor nota  │───▶│ Crea solicitud │                                        │
│ │ error en Acta  │    │ en el Sistema  │                                        │
│ └────────────────┘    └────────────────┘                                        │
│                               │                                                 │
│                               ▼                                                 │
│                       ¿Dentro del plazo                                         │
│                       calendario?                                               │
│                        /             \                                          │
│                   SÍ  /               \ NO                                      │
│                      ▼                 ▼                                        │
│       ┌────────────────┐         ┌────────────────┐                             │
│       │ Aprobación     │         │ Requiere VoBo. │                             │
│       │ Automática     │         │ de Consejo/Dir.│                             │
│       │ de Sist.       │         │ Divisional     │                             │
│       └────────────────┘         └────────────────┘                             │
│                      \                 /                                        │
│                       \               /                                         │
│                        ▼             ▼                                          │
│                     ┌────────────────────┐                                      │
│                     │ Actualización BD + │                                      │
│                     │ Registro en Log de │                                      │
│                     │ Auditoría          │                                      │
│                     └────────────────────┘                                      │
│                               │                                                 │
│                               ▼                                                 │
│                     ┌────────────────────┐                                      │
│                     │ Sincronización con │                                      │
│                     │ Kardex (Módulo 8)  │                                      │
│                     └────────────────────┘                                      │
│                                                                                 │
└─────────────────────────────────────────────────────────────────────────────────┘

Variables de Datos Críticas del Módulo 7

Variable / Campo,Descripción,Tipo,Catálogo / Restricción
ID_Acta,Identificador único del acta generada,Alfanumérico,PK (Llave primaria)
Clave_UEA,Clave institucional de la materia,Numérico,FK (Llave foránea a Catálogo UEAs)
Tipo_Eval,Modalidad de la evaluación,Carácter,"O=Ordinaria, R=Recuperación, E=Especial"
Calificacion,Nota asignada al alumno,Carácter,"MB, B, S, NA, NP (No Presentó)"
Estado_Acta,Control del ciclo de vida del acta,Numérico,"1=Abierta, 2=Capturando, 3=Firmada_Cerrada"
Folio_Rectif,Número de trámite si hubo rectificación,Numérico,Nulo si nunca se ha rectificado

Módulo,Punto de Integración,Datos Intercambiados
Módulo 6: Programación,Generación inicial de actas,Toma listas de alumnos inscritos oficialmente en grupos para generar las actas vacías.
Módulo 4: Profesores,Permisos y firmas,Valida que el ID_Profesor logueado coincida con el profesor asignado a la UEA para permitir la firma.
Módulo 8: Kardex,Asentamiento histórico,"Envía las calificaciones finales (MB, B, S) para calcular el promedio global y los créditos acumulados."
