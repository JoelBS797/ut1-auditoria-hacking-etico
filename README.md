# UT1.01 · Ejercicio Auditoría

**Hacking Ético · Curso 25/26**
RA1. Determina herramientas de monitorización para detectar vulnerabilidades aplicando técnicas de hacking ético.

---

## Resumen del caso

El nuevo departamento de **Seguridad Ofensiva** (Juan y su equipo) recibe de Teresa su primer encargo: diseñar el **plan de auditoría del primer trimestre**. Las condiciones son las siguientes:

| Premisa | Valor |
|---|---|
| Quién audita | **Empresa externa** (es la primera vez que la organización se audita) |
| Presupuesto | **Máximo 5 auditorías** en el trimestre |
| Activos expuestos a internet | **20** (servidores web, servidores de correo, acceso VPN) |
| Activos críticos para el negocio | **3** de esos 20 |
| Requisito adicional | **Primera revisión de la red interna** |
| Fases de cada auditoría | 1) Pre-engagement · 2) Ejecución · 3) Seguimiento · 4) Reporting · 5) Cierre |

**Solución propuesta en una línea:** 4 auditorías (amplitud y profundidad, desde fuera y desde dentro) más una 5.ª de reserva que solo se gasta si se activa.

| ID | Auditoría | Enfoque | Origen | Información |
|---|---|---|---|---|
| AUD-01 | Perímetro (20 activos) | Análisis de vulnerabilidades | Externa | Caja negra |
| AUD-02 | Activos críticos (3) | Test de intrusión | Externa | Caja gris |
| AUD-03 | Red interna | Análisis de vulnerabilidades | Interna | Caja blanca |
| AUD-04 | *Assume breach* desde la LAN | Test de intrusión | Interna | Caja gris |

Vulnerabilidades valoradas (CVSS v3.0): **V1** correo 9.8 Crítica · **V2** SQLi 7.7 Alta · **V3** RCE en FTP 8.3 Alta.

---

## Índice

1. [**Apartado 1 · Diseño del plan de auditoría**](apartado1-plan-auditoria.md)
   - [Punto de partida y suposiciones](apartado1-plan-auditoria.md#0-punto-de-partida)
   - [1.1 Elección de las auditorías](apartado1-plan-auditoria.md#11-elección-de-las-auditorías)
   - [1.2 Activos incluidos en el alcance](apartado1-plan-auditoria.md#12-activos-incluidos-en-el-alcance-de-cada-auditoría)
   - [1.3 Tipo de auditoría (enfoque, origen e información)](apartado1-plan-auditoria.md#13-tipo-de-auditoría-enfoque-origen-e-información)
   - [1.4 Objetivo de cada auditoría](apartado1-plan-auditoria.md#14-objetivo-de-cada-auditoría)
   - [1.5 Por qué 4 auditorías y no 5](apartado1-plan-auditoria.md#15-por-qué-4-auditorías-y-no-5)
2. [**Apartado 2 · Fases y calendario**](apartado2-fases-calendario.md)
   - [Criterios comunes y vista global](apartado2-fases-calendario.md#20-criterios-comunes)
   - [AUD-01](apartado2-fases-calendario.md#aud-01--análisis-de-vulnerabilidades-externo-20-activos--caja-negra) · [AUD-02](apartado2-fases-calendario.md#aud-02--pentest-externo-de-activos-críticos-3-activos--caja-gris) · [AUD-03](apartado2-fases-calendario.md#aud-03--análisis-de-vulnerabilidades-interno-red-interna--caja-blanca) · [AUD-04](apartado2-fases-calendario.md#aud-04--pentest-interno-assume-breach-caja-gris)
   - [Resumen de reuniones de seguimiento](apartado2-fases-calendario.md#resumen-de-reuniones-de-seguimiento)
3. [**Apartado 3 · Valoración de vulnerabilidades (CVSS v3.0)**](apartado3-vulnerabilidades-cvss.md)
   - [V1 · Correo](apartado3-vulnerabilidades-cvss.md#v1--sistema-de-correo-control-total-del-servidor) · [V2 · SQLi](apartado3-vulnerabilidades-cvss.md#v2--inyección-sql-en-servidor-web-con-acceso-a-otras-bases-de-datos) · [V3 · FTP](apartado3-vulnerabilidades-cvss.md#v3--rce-en-servidor-ftp-interno-con-privilegios-de-sistema)
   - [Tabla resumen](apartado3-vulnerabilidades-cvss.md#tabla-resumen)
4. [`img/`](img/): capturas de la calculadora CVSS de FIRST (`v1-cvss.png`, `v2-cvss.png`, `v3-cvss.png`)

---

## Criterio de la rúbrica → dónde se cubre

Los criterios 1 a 10 son los de la rúbrica oficial (1 punto cada uno). Los criterios 11 y 12 son requisitos explícitos del enunciado que no puntúan por separado, pero que la corrección comprobará.

| # | Criterio | Archivo / sección |
|:-:|---|---|
| 1 | **Ap. 1:** elección justificada de las auditorías del plan | [apartado1 § 1.1](apartado1-plan-auditoria.md#11-elección-de-las-auditorías) y [§ 1.5](apartado1-plan-auditoria.md#15-por-qué-4-auditorías-y-no-5) |
| 2 | **Ap. 1:** activos incluidos en el alcance de cada auditoría, justificados | [apartado1 § 1.2](apartado1-plan-auditoria.md#12-activos-incluidos-en-el-alcance-de-cada-auditoría) (y suposiciones en [§ 0.2](apartado1-plan-auditoria.md#02-suposiciones)) |
| 3 | **Ap. 1:** tipo según origen, información y enfoque, justificado por auditoría | [apartado1 § 1.3](apartado1-plan-auditoria.md#13-tipo-de-auditoría-enfoque-origen-e-información) |
| 4 | **Ap. 1:** objetivo de cada auditoría | [apartado1 § 1.4](apartado1-plan-auditoria.md#14-objetivo-de-cada-auditoría) y [tabla resumen final](apartado1-plan-auditoria.md#tabla-resumen-final) |
| 5 | **Ap. 2:** calendario con las 5 fases y tiempos acordes a los activos y al tipo | [apartado2 § 2.0](apartado2-fases-calendario.md#criterio-de-estimación-de-tiempos) (criterio) y el § 2.1 de cada auditoría (gantt + tabla) |
| 6 | **Ap. 2:** hitos a cumplir en cada fase | [apartado2](apartado2-fases-calendario.md), § 2.2 de cada auditoría |
| 7 | **Ap. 2:** reuniones de seguimiento justificadas y en tiempo prudencial | [apartado2](apartado2-fases-calendario.md), § 2.3 de cada auditoría y [resumen](apartado2-fases-calendario.md#resumen-de-reuniones-de-seguimiento) |
| 8 | **Ap. 3:** vector CVSS y puntuación correctos | [apartado3](apartado3-vulnerabilidades-cvss.md), § 3.1 de V1, V2 y V3, y [tabla resumen](apartado3-vulnerabilidades-cvss.md#tabla-resumen) |
| 9 | **Ap. 3:** justificación precisa de cada métrica | [apartado3](apartado3-vulnerabilidades-cvss.md), § 3.2 de cada vulnerabilidad y [análisis de sensibilidad](apartado3-vulnerabilidades-cvss.md#análisis-de-sensibilidad-métricas-discutibles) |
| 10 | **Ap. 3:** ¿afecta al servidor o al cliente?, justificado | [apartado3](apartado3-vulnerabilidades-cvss.md), § 3.3 de cada vulnerabilidad |
| 11 | *Enunciado, ap. 3:* captura de la calculadora CVSS | [apartado3](apartado3-vulnerabilidades-cvss.md), § 3.4 de cada vulnerabilidad · [`img/`](img/) |
| 12 | *Enunciado:* cumplir las premisas del caso (≤ 5 auditorías, 20 activos, 3 críticos, red interna, empresa externa) | [apartado1 § 0.1](apartado1-plan-auditoria.md#01-premisas-del-caso-enunciado) y [cobertura de activos § 1.2](apartado1-plan-auditoria.md#12-activos-incluidos-en-el-alcance-de-cada-auditoría) |

---

## Notas

- Todas las **suposiciones** (reparto de los activos, tamaño de la red interna, fechas, duraciones) están marcadas como **Suposición** en el texto.
- Los diagramas usan **Mermaid**, que GitHub y HackMD renderizan de forma nativa.
- Las puntuaciones CVSS se han calculado a mano con las fórmulas de la especificación v3.0 y se han comprobado con la [calculadora oficial de FIRST](https://www.first.org/cvss/calculator/3.0).
