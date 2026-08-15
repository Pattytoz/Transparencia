# Proyecto de Auditoría de Datos y Business Intelligence: Registro de Transparencia Pública (2003–2025)

**[Ver presentación del proyecto en YouTube](https://youtu.be/zAQIQUcIZG8)**

---

## Resumen Ejecutivo
Análisis integral y auditoría de calidad sobre un conjunto de más de 260,000 registros históricos de solicitudes de información pública. El objetivo principal del proyecto consistió en evaluar la integridad de los datos, medir los tiempos reales de respuesta institucional y diseñar propuestas de arquitectura de software para optimizar la gobernanza del sistema.

---

## Stack Tecnológico
`Power BI` | `Power Query (ETL / Data Wrangling)` | `DAX` | `Python`

![Clasificación de Texto en Python](assets/Python.png)

---

## Puntos Clave del Proyecto

* **ETL & Data Wrangling:** Depuración y estandarización de más de 500 variantes de nombres de dependencias públicas mediante Power Query.
  
  ![Depuración y Estandarización](assets/1.jpg)
  
* **Modelado DAX:** Creación de métricas para auditar plazos límite y detectar discrepancias entre el cumplimiento oficial y el cumplimiento real.
  
  ![Métricas y Análisis DAX](assets/DAX.jpg)
  
* **Data Storytelling & BI:** Diseño de tableros interactivos e intuitivos en Power BI para el análisis de tendencias, volumen por materia y comportamiento por dependencia (IFAI/INAI, PJF, PEMEX).
  
  ![Dashboard PEMEX](assets/PEMEX.png)
  
  ![Dashboard IFAI/INAI](assets/IFAI.jpg)
  
  ![Dashboard PJF](assets/PJF.jpg)
  
* **Gobernanza de Datos:** Formulación de propuestas técnicas orientadas a la estandarización de captura desde el origen y la automatización de plazos por sistema.
  
  ![Propuestas Técnicas](assets/PROPUESTAS.png)

---

## Ejemplos de Medidas DAX Utilizadas

```dax
% Cumplimiento a Tiempo = 
VAR SolicitudesATiempo = 
    CALCULATE(
        [Total Folios],
        'Sheet1'[FECHARESPUESTA] <= 'Sheet1'[FECHALIMITE]
    )
RETURN
    DIVIDE(SolicitudesATiempo, [Total Folios], 0)
```

```dax
% Plazos Irregulares / Alterados = 
VAR TotalSolicitudes = COUNTROWS('Transparencia')
VAR SolicitudesAlteradas = 
    CALCULATE(
        COUNTROWS('Transparencia'), 
        'Transparencia'[Alerta Fecha Límite] = "3. Plazo Irregular / Alterado (>30 días)"
    )
RETURN
    DIVIDE(SolicitudesAlteradas, TotalSolicitudes, 0)
```

```dax
Alerta Fecha Límite = 
VAR DiasHabilesPlazo = NETWORKDAYS('Transparencia'[FECHASOLICITUD], 'Transparencia'[FECHALIMITE]) - 1
RETURN
    SWITCH(
        TRUE(),
        ISBLANK('Transparencia'[FECHALIMITE]), "Sin Fecha Límite",
        DiasHabilesPlazo <= 20, "1. Plazo Ordinario (<=20 días)",
        DiasHabilesPlazo <= 30, "2. Plazo con Prórroga (21-30 días)",
        "3. Plazo Irregular / Alterado (>30 días)"
    )
```

---

## Optimización de Rendimiento y Arquitectura ETL

* **Estrategia de Carga:** Para optimizar el uso de memoria RAM y acelerar el tiempo de respuesta del motor ante más de 260,000 registros, se realizó la combinación de consultas (*Merge*) directamente en **Power Query**.
* **Diseño del Modelo:** Se optó por una arquitectura desnormalizada en una tabla maestra (`Transparencia`), desactivando la detección automática de relaciones para reducir el impacto en memoria y eliminar la sobrecarga de consultas cruzadas en DAX.

---

## Principales Hallazgos (Insights)

* **Normalización:** Reducción de más de 500 variantes de nombres a 38 dependencias consolidadas.
* **Auditoría de Cumplimiento:** Identificación de brechas entre el cumplimiento oficial reportado y el cumplimiento real tras auditar los ajustes manuales de fechas.
* **Solicitudes Atípicas:** Detección y análisis de 6,531 registros con contracciones en el tratamiento institucional.

---

## Propuestas de Solución Técnica

1. **Estandarización en Origen:** Captura estructurada mediante listas desplegables en el portal para eliminar dispersión y ruido desde la captura.
2. **Blindaje por Sistema:** Cálculo automatizado de fechas límite mediante programación para prevenir modificaciones manuales de plazos.
3. **Transparencia Proactiva:** Implementación de repositorios de datos abiertos para los 5 temas con mayor concentración de demanda (82% del total nacional).

---

## Autoría y Contacto

**Patricia E. Torres G.**  
*Ingeniera en Software | Administradora de Empresas | Data Analyst*

* 🔗 **LinkedIn:** [linkedin.com/in/patricia-e-torres](https://www.linkedin.com/in/patricia-e-torres)
* ✉️ **Correo:** [otropattytoz@gmail.com](mailto:otropattytoz@gmail.com)

> *Proyecto y código desarrollados por Patty Torres para fines de portafolio personal. Todos los derechos reservados.*
