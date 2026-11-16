# Unidad 1: Arquitectura Dirigida por Modelos (MDA) - Parte I

**Materia:** DAAS  
**Origen:** [[Unidad-1-P1.pdf]]  
**Tags:** #DAAS #MDA #MDD #Modelado #Metamodelos #OMG

---

## 1. El Ciclo de Vida Tradicional vs. Enfoque Basado en Modelos

En el desarrollo de software convencional, el proceso suele estar **conducido por el diseño de bajo nivel y el código fuente**.

```
[Requisitos (Texto)] 
       ↓
[Análisis (Texto / Diagramas)] 
       ↓
[Diseño (Texto / Diagramas)] 
       ↓
[Codificación (Código)]  <--- "El atajo de los programadores"
       ↓
[Testeo (Código)] 
       ↓
[Emplazamiento / Despliegue]
```

### El "Atajo de los Programadores" y sus consecuencias
[Requisitos] → [Codificación]

En la práctica, los desarrolladores suelen saltar directamente de los requisitos/diseño preliminar a la codificación. Los modelos iniciales quedan rápidamente **desactualizados y obsoletos** porque el código evoluciona por separado.
* **Problemas resultantes:**
  * Baja productividad a largo plazo.
  * Mantenimiento complejo y costoso.
  * Documentación desfasada o nula.
  * Rigidez y falta de flexibilidad frente a cambios tecnológicos.

---

## 2. Desarrollo de Software Dirigido por Modelos (MDD)

> **Model-Driven Development (MDD):** Enfoque que sitúa a los modelos en el **centro del proceso de desarrollo**, otorgándoles el mismo valor e importancia que al código fuente, con el respaldo de herramientas de software automatizadas.
> Mejora el proceso de construcción de software basándose en un proceso guiado por modelos y soportado por potentes herramientas de desarrollo.

* **Premisa fundamental:** Los modelos se van refinando desde los niveles más abstractos hasta los más concretos mediante sucesivos pasos de **transformación y/o refinamiento** hasta generar el código ejecutable final.
* **El motor de MDD:** La **transformación automatizada entre modelos**.

### Diferencia Clave: Artefactos de Desarrollo
* **Desarrollo clásico:** Artefactos informales (documentos de texto, bocetos, diagramas que no son interpretables por máquinas).
* **MDD:** Los artefactos son **modelos formales**, dotados de sintaxis y semántica precisas, directamente interpretables y procesables por computadoras.

---

## 3. Tipos de Modelos en MDD

MDD clasifica los modelos según su grado de abstracción:

```
[ CIM ] -> Modelo del Dominio / Negocio (Independiente de Computación)
   ↓ (Transformación)
[ PIM ] -> Modelo Lógico del Sistema (Independiente de Plataforma)
   ↓ (Transformación guiada por PDM)
[ PSM ] -> Modelo Tecnológico (Específico para Java, Relacional, .NET, etc.)
   ↓ (Generación de código)
[ IM ]  -> Código Fuente / Implementación Ejecutable
```

| Tipo de Modelo | Nombre Completo                 | Propósito y Características                                                                                                                                                                                                                          |
| :------------- | :------------------------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **CIM**        | *Computation Independent Model* | **Modelo del Dominio.** Vista de muy alto nivel que no muestra detalles de estructura computacional. Utiliza el vocabulario propio de los expertos del negocio o dominio.                                                                            |
| **PIM**        | *Platform Independent Model*    | Modela la funcionalidad requerida por el negocio **abstrayéndose por completo de la tecnología de implementación** (ignora SO, lenguajes, frameworks, bases de datos o topología de red). Un mismo PIM puede implementarse en múltiples plataformas. |
| **PSM**        | *Platform Specific Model*       | Proyección y adaptación del PIM sobre una tecnología concreta. Incorpora construcciones y conceptos propios de dicha plataforma (ej. clases Java, tablas/claves foráneas en SQL). Un único PIM suele generar **múltiples PSMs complementarios**.     |
| **IM**         | *Implementation Model*          | Representación del código fuente generado directamente a partir de cada PSM.                                                                                                                                                                         |

![477](../../../Adjuntos/Pasted%20image%2020261114180846.png)

> [!NOTE]
> Generar código desde un modelo cercano a la plataforma (PSM) ya existía en herramientas CASE clásicas. Lo verdaderamente innovador de MDD es la **transformación automatizada de modelo a modelo** (e.g., PIM $\to$ PSM).

---
### Fases Principales en MDD
1. **Fase I (Diseño del PIM):** Creación del modelo independiente de la plataforma.
2. **Fase II (Transformación PIM a PSM):** Traducción del PIM a uno o más modelos específicos de la plataforma destino.
3. **Fase III (Transformación PSM a Código):** Generación automática del código ejecutable.

---
## 4. Propuestas Principales de MDD: MDA vs. DSM

Dentro de la disciplina MDD destacan dos corrientes principales:
1. **MDA (*Model-Driven Architecture*):** Propuesta y estándar formal promovido por el **OMG** (*Object Management Group*).
2. **DSM (*Domain-Specific Modeling*):** Enfoque basado en lenguajes y modelos creados a medida para dominios muy específicos.

### Distinción MDD vs. MDA:
* **MDD:** Es el concepto general / paradigma de desarrollo guiado por modelos. Refiere a las actividades llevadas a cabo por los desarrolladores de software.
* **MDA:** Es la **especificación formal y estandarizada del OMG** que provee el marco y las herramientas normativas para ejecutar MDD.

### 4.1 MDA
Uno de los principales objetivos de MDA es hacer la especificación de un sistema de manera independiente de los detalles de implementación.
MDA proporciona un marco que permite:
1. Dar una especificación de un sistema con independencia de los detalles de implementación.
2. Especificar plataformas de implementación.
3. Elegir una de estas plataformas.
4. Transformar la especificación del sistema en una plataforma de implementación.


---

## 5. [Conceptos](Parte%201/Conceptos.md) Fundamentales de MDA

- **Sistema:** colección de componentes.
- **Modelo (de un sistema):** abstracción.
- **Arquitectura:** estructura organizativa de un sistema.
- **Plataforma:** Conjunto de subsistemas y tecnologías que proporcionan un conjunto coherente de funcionalidad mediante interfaces y patrones de uso.
- **Aplicación:** funcionalidad que es desarrollada.
- **Metamodelo:** Define estructura, semántica y restricciones para una familia de modelos.
- **Transformación:** proceso de convertir un modelo en otro modelo del mismo sistema
- **Mapping:** especificación de una transformación (por eje: PIM → PSM)

---

## 6. Arquitectura de Cuatro Niveles de la OMG

El OMG establece una jerarquía formal de cuatro capas de modelado y metamodelado:

```
[ M3: Meta-metamodelo (MOF) ]   -> Define las reglas para crear metamodelos
               ↑ describe
[ M2: Metamodelo (UML, etc.) ]  -> Define elementos del lenguaje (Clase, Atributo, Tabla...)
               ↑ describe
[ M1: Modelo de Usuario ]       -> Modelo concreto de un sistema (ej. Clase "Video")
               ↑ describe
[ M0: Instancias en Ejecución ] -> Objetos reales en runtime (ej. objeto "aVideo")
```

| Nivel  | Rol OMG                     | Tecnologías / Ejemplos                  | Descripción                                                                                                                            |
| :----: | :-------------------------- | :-------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| **M3** | **Meta-metamodelo**         | **MOF** (*Meta-Object Facility*)        | Nivel superior. Define las propiedades y construcciones que pueden tener los metamodelos. Es auto-descriptivo.                         |
| **M2** | **Metamodelo**              | UML, Metamodelo Relacional, CWM         | Describe los conceptos y reglas de un lenguaje de modelado concreto (ej. qué es una clase, una relación, un tipo de dato o una tabla). |
| **M1** | **Modelo (*User Model*)**   | Diagrama de clases de una aplicación    | Modela un sistema concreto para un dominio de negocio específico (ej. la clase `Video` con el atributo `title: String`).               |
| **M0** | **Instancias (*Run-Time*)** | Datos en memoria / Objetos en ejecución | Objetos y valores reales en tiempo de ejecución (ej. la instancia particular `:Video` con `title = "2001: A Space Odyssey"`).          |

---

## 7. Mecanismo de Transformación en MDA

Para llevar a cabo el desarrollo en MDA:
1. Se define un **meta-metamodelo común (MOF en M3)**.
2. Con MOF se construyen los **metamodelos (M2)**: el metamodelo independiente de la plataforma (*PIM Metamodel*) y el específico (*PSM Metamodel*).
3. Se especifica formalmente una **regla de transformación (*Transformation Specification*)** entre ambos metamodelos.
4. Con dicha especificación, las herramientas automáticas toman el **PIM del usuario (M1)** y lo transforman en el **PSM del usuario (M1)**, y posteriormente en código ejecutable.

---

## Enlaces y Notas Relacionadas
* [[02 - Resumen - Unidad 1 - MDA Parte II]]
* [[Ciclo de vida del desarrollo de software]]
* [[MDD (Model Driven Development)]]
* [[Parte 1/Conceptos]]
* [[Acrónimos]]
