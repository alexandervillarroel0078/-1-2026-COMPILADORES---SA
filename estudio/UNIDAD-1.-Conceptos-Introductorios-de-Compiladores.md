# TEORÍA COMPLETA — Compiladores Unidad 1

## 1. ¿Qué es un Compilador?

Un **Compilador** es un software que lee un programa **F** escrito en un lenguaje (el **lenguaje fuente**) y lo traduce a un programa equivalente **O** en otro lenguaje (el **lenguaje objeto**).

- Lee el Programa Fuente **char a char** (caracter a caracter).
- El Programa Fuente está escrito en **Plain-Text**.
- Como parte del proceso, **informa al usuario de errores** en el programa fuente.
- Un Compilador **NO CORRE (run)** el Programa Objeto. Solo traduce F → O.

---

## 2. Compilador Clásico vs No-Clásico

| Tipo | Qué hace |
|---|---|
| **Clásico / Estándar** | Programa Fuente (Texto) → Código Máquina (.exe) |
| **No-Clásico** | Programa Fuente (Texto) → Código en otro lenguaje (no-máquina) |

- El **Clásico** genera ejecutables stand-alone de código máquina absoluto.
- El **No-Clásico** puede traducir, por ejemplo, de C++ a Java, o de Delphi-Pascal a C++.
- La definición de compilador **no limita** el lenguaje del programa objeto.

---

## 3. Compilación a Código Intermedio (IL)

La tendencia actual es que el compilador traduzca el Programa Fuente a un **Lenguaje Intermedio (IL)** para que el código sea **portable**.

- **IL (Intermediate Language)**: lenguaje muy cercano al assembler, pero **independiente del código máquina** de cualquier computadora.
- El IL fue inventado para ser soportado por una **Máquina Virtual (VM)** — una computadora imaginaria/abstracta.
- Un programa llamado **intérprete de la VM** se encarga de la ejecución del Código Intermedio.

**Ejemplo con Java:**
- `javac.exe` (compilador) traduce `Juego.java` → `Juego.jar` (bytecode = IL).
- `java.exe` (intérprete de la JVM) ejecuta el `.jar`.
- Cada instrucción del IL de Java se representa como una secuencia de bytes: **bytecode**.

---

## 4. El Usuario de un Compilador

El usuario normalmente es:
1. **El Programador** — usa el compilador desde la línea de comandos (cmd).
2. **El IDE** — interactúa con el compilador y además permite abrir/guardar/editar archivos, detectar errores, depurar, ejecutar.

Flujo: Usuario → envía Programa Fuente → Compilador → si no hay errores: genera Programa Objeto / si hay errores: los comunica al Usuario.

> **IDE** (Integrated Development Environment): programa compuesto por Editor de Texto, Compilación, Ejecución, Depuración, etc. Puede ser para un solo lenguaje o varios.

---

## 5. Tipos de Compiladores

> Esta taxonomía **no es excluyente** — un compilador puede pertenecer a varios tipos.

### 5.1 Compilador de Una o Varias Pasadas
- **Varias Pasadas (Multi-pass)**: lee el Programa Fuente más de una vez por la complejidad del lenguaje. Ejemplo: MASM de Microsoft.
- **Una Pasada (One-pass)**: lee el Programa Fuente una sola vez. Ejemplo: Compilador de Java.

### 5.2 Compilador Cruzado (Cross-Compiler)
- Genera código ejecutable para una **plataforma diferente** a la que lo ejecuta.
- Útil cuando no se tiene acceso a la plataforma destino o es incómodo compilar en ella (sistemas empotrados: Arduino, Raspberry Pi, etc.).
- Ejemplo 1: Compilador C++ corriendo en Windows → genera código para Arduino (distinta plataforma).
- Ejemplo 2: Android Studio en Windows → genera `.apk` para correr en Android.

### 5.3 Compilador Nativo
- Contrario al cruzado: genera código máquina **específico para la misma computadora** donde corre el compilador.
- El ejecutable generado está **optimizado** para el SO y arquitectura de la máquina de compilación.
- Ejemplo: Compilador Delphi en Windows genera `Prog.exe` para correr en Windows.

### 5.4 Compilador Optimizador
- Minimiza ciertos atributos del programa para aumentar eficiencia y rendimiento:
  - Reduciendo el **tiempo de ejecución** del Programa Objeto.
  - Reduciendo la **memoria** que ocupa en ejecución.
  - Reduciendo el **tamaño** del Programa Objeto.
- La optimización se realiza **después** de la Generación de Código.
- Muchos compiladores profesionales (Java, Delphi) usan optimización.

### 5.5 Compilador JIT (Just in Time)
- También llamado **Compilador Dinámico** (en tiempo de ejecución).
- Se usa cuando el Programa Objeto (Código Intermedio) **ya está corriendo**.
- Traduce a Código Máquina nativo funciones o un archivo completo del Código Intermedio.
- En el caso de funciones: se compila **justo cuando va a ejecutarse** ("just-in-time").

### 5.6 Metacompilador
- Sinónimo de **compilador de compiladores**.
- Recibe como entrada las **especificaciones del lenguaje** y genera como salida el **compilador para ese lenguaje**.
- Lo que se ha logrado desarrollar: **generadores de analizadores léxicos y sintácticos**.

---

## 6. Las Fases de un Compilador (Etapas)

Un compilador trabaja en **dos etapas** y **seis fases**:

### Etapa de Análisis (Front-End)
1. Analizador Léxico (Scanner)
2. Analizador Sintáctico (Parser)
3. Analizador Semántico

### Etapa de Síntesis (Back-End)
4. Generador de Código Intermedio
5. Optimador de Código
6. Generador de Código Objeto

> **Etapa de Análisis**: lee char a char, verifica correctitud. Si hay errores → comunica y finaliza. Si no hay errores → pasa a síntesis.

> **Etapa de Síntesis**: donde realmente se traduce F en O.

Las seis fases son ayudadas por dos módulos adicionales:
- **Tabla de Símbolos**
- **Manejador de Errores**

---

## 7. La Tabla de Símbolos (TS)

- Un **identificador (ID)**: nombre que el programador le da a un elemento del lenguaje (class, función, variable, procedimiento, etc.).
- No confundir ID con **String Constante (Literal)**: encerradas entre comillas, ej. `"Hola Mundo"`.

La **Tabla de Símbolos** es una Estructura de Datos donde cada símbolo del Código Fuente está asociado con:
- Su **ubicación**
- Su **tipo de datos**
- Su **ámbito (scope)**

Datos almacenados: todos los **identificadores (ID)** del programa y otra información necesaria para el compilador.

- El **Analizador Léxico y el Sintáctico escriben/leen** en ella.
- Las demás fases **solo leen** sus datos.
- En la práctica, la TS está compuesta por **varias tablas** (puede usar estructuras como Hash Tables).

---

## 8. Analizador Léxico (Scanner / Lexer / Analex)

- Es la **primera fase** del compilador.
- Lee **char a char** el Programa Fuente.
- Agrupa caracteres consecutivos en **tokens** (substrings que obedecen a un patrón de formación).
- Envía los tokens **uno por uno** al Parser.
- Es considerada la **fase más lenta** del compilador (por leer char a char).

**Ejemplo:**
```
F = "x := 640 * Velocidad – 456;"
```
```
x     :=      640   *    Velocidad  –      456   ;
ID  ASSIGN   NUM   POR     ID     MENOS   NUM  PTOCOMA
```

El Analex **NO verifica el orden** de los tokens — solo los reconoce. Si el fuente es erróneo, igual reconoce tokens sin comunicar error.

**Tareas adicionales del Analex:**
- Ignorar los **comentarios** del Programa Fuente.
- Instalar (insertar) las **StringCtte's e ID's** en la Tabla de Símbolos.

---

## 9. Analizador Sintáctico (Parser)

- **Sintaxis**: conjunto de reglas (gramática) que definen las secuencias correctas (orden) de los tokens en un lenguaje.
- El Parser revisa el **orden correcto** en que deben aparecer los tokens.
- Recibe tokens del Scanner y emite error si no están en el orden correcto.

---

## 10. Analizador Semántico

- **Semántica**: interpretación del significado de las sentencias del Programa Fuente (¿qué desea hacer el programador?).
- Se encarga de comprobar que el **significado** de lo que se lee es válido.
- Verifica:
  - **Compatibilidad de tipos** de datos.
  - **Rangos** de asignación.
  - **Ámbito (scope)** de variables.

**Ejemplo de errores semánticos** (código sintácticamente correcto):
```java
byte x, y;
string s;
int V[5];
x = 300;      // Out of range: byte va de 0 a 255
y = s + x;    // Tipos incompatibles: s es string
V[100] = 0;   // Out of range: no existe la casilla 100
```

---

## 11. Generador de Código Intermedio

- Se activa al constatar que el Programa Fuente F es **válido** (sin errores).
- Convierte F en un código cercano al assembler: el **IL (Intermediate Language)**.
- Facilita el trabajo de las fases siguientes (Optimación y Generación de Código Objeto).

**Ejemplo:**
```java
x := 640 * Velocidad – 456;
```
Se convierte en:
```
t1 = 640
t2 = Velocidad
t3 = t1 * t2
t4 = 456
t5 = t3 – t4
x = t5
```
> El IL mostrado es el **Código de Tres Direcciones (C3)** — el más popular de todos los IL's. El diseñador del compilador puede optar por otro IL o uno propio.

---

## 12. Optimador de Código

- Reduce el tamaño del Código Intermedio para que el Código Objeto final sea **más pequeño y más rápido**.

**Ejemplo:**
```
ANTES                  DESPUÉS (optimado)
t1 = 640               t1 = 640
t2 = Velocidad         t3 = t1 * Velocidad
t3 = t1 * t2      →    t4 = 456
t4 = 456               x = t3 – t4
t5 = t3 – t4
x = t5
```

---

## 13. Generador de Código Objeto Final

- Genera el **Código Objeto Final** a partir del Código Intermedio optimado.

**Ejemplo:**
```
t1 = 640                MOV [t1], 640
t3 = t1 * Velocidad     MOV AX, Velocidad
t4 = 456           →    MUL BX
x = t3 – t4             MOV [t3], BX
                         ……… (etc)
```

---

## 14. ¿Qué es un Intérprete?

- Software capaz de **analizar y ejecutar (run)** otros programas escritos en un Lenguaje de Programación.
- Realiza la traducción **a medida que sea necesaria**, instrucción por instrucción.
- **No guarda** el resultado de la traducción (no genera Programa Objeto).
- Desde la perspectiva del usuario: es un **Runner/Ejecutor** y una **Máquina Abstracta**.

---

## 15. Diferencia entre Compilador e Intérprete

| Compilador | Intérprete |
|---|---|
| Convierte F en O equivalente | Analiza y ejecuta F directamente |
| NO ejecuta ninguna instrucción de F ni de O | NO genera Programa Objeto |
| Traduce todo antes de ejecutar | Traduce instrucción por instrucción al ejecutar |

---

## 16. Ventajas y Desventajas del Intérprete

**Desventaja:**
- Los programas interpretados son **más lentos** que los compilados (traduce mientras ejecuta).

**Ventajas:**
- Más **flexibles** como entornos de programación y depuración (debug).
- Facilita reemplazar partes del programa o añadir módulos nuevos.
- Ofrecen un entorno **no dependiente de la Plataforma** donde corre el Intérprete, sino del propio Intérprete (razón para llamarlo "Máquina").