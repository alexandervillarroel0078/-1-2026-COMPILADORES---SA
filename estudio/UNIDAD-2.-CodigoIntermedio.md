# Teoría Completa: Código Intermedio (IL / C3)

---

## 1. ¿Qué es el Código Intermedio?

**Código Intermedio** = programa escrito en un lenguaje especial llamado **Lenguaje Intermedio** (IL = *Intermediate Language*).

> Los términos "Código Intermedio" y "Lenguaje Intermedio" se usan coloquialmente como sinónimos.

---

## 2. Lenguaje Máquina

- Cada familia de CPUs entiende un lenguaje binario propio llamado **Lenguaje Máquina**.
- Cada instrucción es (o se codifica como) un **número binario**.
- El código compilado para CPU1 **no funciona** en CPU2 porque los mismos bits representan instrucciones distintas en cada familia.

---

## 3. Lenguaje Ensamblador (ASM)

- El **Ensamblador** traduce mnemónicos (instrucciones en texto) a su codificación binaria, uno a uno.
- Pero cada familia de CPU tiene sus propios mnemónicos:

```asm
; Intel 8086
ADD AX, BX   ; Reg1 = Reg1 + Reg2
SUB AX, BX   ; Reg1 = Reg1 - Reg2

; IBM 370
AR R1, R2    ; Reg1 = Reg1 + Reg2
SR R1, R2    ; Reg1 = Reg1 - Reg2
```

> **Conclusión:** Lenguaje Máquina y Ensamblador son específicos de cada computadora. Solo corren en las máquinas que los entienden.

---

## 4. El Lenguaje Intermedio (IL)

> Un **IL** tiene instrucciones del mismo nivel del Lenguaje Máquina (muy básicas, similares al ASM), pero es **independiente** de cualquier computadora real. Ninguna computadora física entiende directamente un IL.

- "Mismo nivel que el lenguaje máquina" = instrucciones muy básicas.
- "Independiente del lenguaje máquina" = ninguna computadora lo entiende directamente.

### ¿Quién ejecuta el IL?

Una **Máquina Virtual (VM = *Virtual Machine*)**: computadora imaginaria/abstracta inventada por el diseñador del IL.

> **Ejemplo famoso:** Java. `javac.exe` compila `.java` → `.jar` (Código Intermedio). Luego `java.exe` (la JVM) interpreta el `.jar`. Es decir, `java.exe` es un intérprete del IL generado por el compilador.

---

## 5. Front-End y Back-End del Compilador

El compilador moderno se divide en dos partes:

| Parte | Función | Etapas |
|---|---|---|
| **Front-End** | Analiza el código fuente y genera Código Intermedio | Analizador Léxico → Sintáctico → Semántico → Generador de CI |
| **Back-End** | Toma el CI y produce Código Objeto para la plataforma deseada | Optimador de Código → Generador de Código Objeto |

- La tarea del **Front-End** empieza con el Analizador Léxico y **termina con el Código Intermedio generado**.
- La tarea del **Back-End** es tomar el CI y generar el **Código Objeto** que se desee.

### Ventaja del esquema Front-End / Back-End

- Un solo **Front-End** puede alimentar múltiples **Back-Ends** para distintas plataformas (Windows x86, Linux ARM, Android, iOS...).
- Múltiples **Front-Ends** (Delphi, C++, Python) pueden compartir el mismo **Back-End**.

### Interpretación del IL (alternativa al Back-End)

Algunos compiladores solo generan Código Intermedio y no generan Código Absoluto. En vez de eso, **emulan la VM con un software intérprete** del IL.

---

## 6. Código de Tres Direcciones (C3 / 3AC)

> El IL más ampliamente usado. Abreviado **3AC**, **Código-3** o **C3**.

Cada instrucción tiene **como máximo 3 direcciones de memoria** y es típicamente una combinación de asignación y operador binario. Ejemplo: `t1 = t2 + t3`.

### ¿Qué es una "dirección de memoria"?

- Variables (`x`, `y`, `t1`, `t2`...)
- Procedimientos/funciones
- Etiquetas (`E1:`, `E2:`...)

### Ejemplos

```
x = 10        ✓  (1 dirección)
z = y * w     ✓  (3 direcciones)
q = w         ✓  (2 direcciones)
p = y/t1 + z  ✗  (4 direcciones)
m = m*y + m   ✗  (4 direcciones)
```

---

## 7. Convenciones de identificadores en C3

- **Etiquetas:** solo `E1:`, `E2:`, `E3:`... (no nombres libres como en ASM)
- **Variables temporales** (creadas por el compilador): `t1`, `t2`, `t3`...
- Todas las variables son **numéricas enteras** (en este curso)
- Variables y procedimientos siguen las mismas reglas de nombres que en lenguajes de alto nivel
- Las **etiquetas son de ámbito global** → no repetir en todo el programa
- Las **variables temporales** `t1`, `t2`... son **locales** a cada procedimiento

> **Regla importante:** toda operación aritmética/lógica ocurre entre variables, **nunca con constantes directas**.
> - ✗ Incorrecto: `x = z – 10`
> - ✓ Correcto: `t1 = 10` luego `x = z – t1`

---

## 8. Juego de Instrucciones C3

### Asignaciones simples

```
Var = numero      ; ej: x = 10, t2 = 60
Var1 = Var2       ; ej: y = x, t1 = z
Var1 = -Var2      ; negación unaria, ej: x = -y
```

### Operaciones aritméticas (op ∈ {+, –, *, /, MOD})

```
Var1 = Var2 op Var3   ; ej: x = y * z,  t1 = z – t2
```

### Operaciones lógicas

```
Var1 = not Var2
Var1 = Var2 or  Var3
Var1 = Var2 and Var3
```

### Operaciones relacionales (oprel ∈ {=, ≠, ≤, <, ≥, >})

```
Var1 = (Var2 oprel Var3)   ; ej: t1 = (x < y),  z = (x ≠ y)
```

### Tratamiento de booleanos (el C3 solo maneja enteros)

- **Al leer** el valor de una variable `z`:
  - Si `z = 0` → `z` es `false`
  - Si `z ≠ 0` → `z` es `true`
- **Al escribir** en una variable `z = Expresión`:
  - Si el resultado es `false` → se escribe `0`
  - Si el resultado es `true` → se escribe `1`

```
x = -2
p = 0
z = x or p    ; z = 1  (x≠0 → true; p=0 → false; true or false = true → 1)

y = 50
t1 = -100
z = not y     ; z = 0  (y≠0 → true; not true = false → 0)
q = t1 and y  ; q = 1  (t1≠0 → true; y≠0 → true; true and true → 1)
```

### Incremento y Decremento

```
INC var   ; var = var + 1
DEC var   ; var = var – 1
```

### Saltos

```
GOTO etiqueta              ; salto incondicional
IF (var=1) GOTO etiqueta   ; salta si var es true (≠0)
IF (var=0) GOTO etiqueta   ; salta si var es false (=0)
```

### E/S por consola

```
READ(var)          ; lee valor desde teclado
WRITE(var)         ; muestra valor de var
WRITES("mensaje")  ; muestra texto literal
NL                 ; NewLine: cursor baja a la siguiente línea
```

### Procedimientos

```
CALL nombre_proc   ; llama al procedimiento
RET                ; retorna al punto de llamada (CALL)
```

> - El procedimiento principal se llama `$Main`
> - **No hay variables locales** → todas son globales (excepto temporales `t1`, `t2`... que sí son locales)
> - Las etiquetas son globales → si Proc1 usa E1 y E2, Proc2 debe usar E3, E4...

---

## 9. Esquemas de Traducción C3

Un **Esquema de Traducción C3** es un layout/plantilla que define el compilador para convertir construcciones de un lenguaje de alto nivel a C3.

### Notación usada

| Notación | Significado |
|---|---|
| `C3-Sentencia` | Instrucciones C3 que traducen una sentencia de alto nivel |
| `C3-Sentencias` | Instrucciones C3 que traducen una serie de sentencias |
| `C3-Expr(tk)` | Traducción de expresión aritmética; resultado queda en temporal `tk` |
| `C3-ExprBoole(tk)` | Traducción de expresión booleana; resultado queda en temporal `tk` |

> Se usa `tk` porque a priori no se sabe cuántos temporales se necesitarán.

### Asignación aritmética: `Var = Expr`

```
C3-Expr(tk)
Var = tk
```

### Asignación booleana: `Var = ExprBoole`

```
C3-ExprBoole(tk)
Var = tk
```

### while

```
; while (ExprBoole) { Sentencias; }

Ei:
    C3-ExprBoole(tk)
    IF (tk=0) → GOTO Ef
    C3-Sentencias
    GOTO Ei
Ef:
```

### if

```
; if (ExprBoole) { Sentencias; }

C3-ExprBoole(tk)
IF (tk=0) → GOTO Ef
C3-Sentencias
Ef:
```

### if-else

```
; if (ExprBoole) { Sentencias1; } else { Sentencias2; }

C3-ExprBoole(tk)
IF (tk=0) → GOTO En
C3-Sentencias1
GOTO Ef
En:
    C3-Sentencias2
Ef:
```

### do-while

```
; do { Sentencias; } while (ExprBoole)

Ei:
    C3-Sentencias
    C3-ExprBoole(tk)
    IF (tk=1) → GOTO Ei
```

### for i = a to b (se convierte primero a while)

```
i = a
Ei:
    t1 = (i ≤ b)
    IF (t1=0) → GOTO Ef
    C3-Sentencias
    INC i
    GOTO Ei
Ef:
```

---

## 10. Ejemplos completos de conversión a C3

### while con print

```
; Java: k=1; n=5; while(k<=n){ print(k); k++; }

k = 1
n = 5
E1:
    t1 = (k ≤ n)
    IF (t1=0) → GOTO E2
    write(k)
    INC k
    GOTO E1
E2:
```

### Factorial

```
; while(k<=N){ F=F*k; k++; }

writeS("N=")
read(N)
F = 1
k = 1
E1:
    t1 = (k ≤ N)
    IF (t1=0) → GOTO E2
    F = F * k
    INC k
    GOTO E1
E2:
writeS("El factorial es ")
write(F)
```

### Expresión: z = x * (y + w) / a

```
t1 = y + w
t2 = x * t1
z  = t2 / a
```

### Expresión con constante: p = x * 10

```
t1 = 10
p  = x * t1
```

### Expresión compleja: x = y + z % w – 5

```
t1 = z MOD w
t2 = y + t1
t3 = 5
x  = t2 – t3
```

### Expresión booleana: sw = (x == y) || (z != 0)

```
t1 = (x = y)
t2 = 0
t3 = (z ≠ t2)
sw = t1 or t3
```

### Con procedimientos: leer N validado, imprimir N asteriscos

```
Proc. Lectura
E1:
    writeS("N=")
    read(N)
    t1 = 0
    t2 = (N ≤ t1)
    IF (t2=1) → GOTO E1
    RET

Proc. $Main
    CALL Lectura
    k = 1
E2:
    t1 = (k ≤ N)
    IF (t1=0) → GOTO E3
    writeS("*")
    INC k
    GOTO E2
E3:
    RET
```

---

## 11. Representación interna del C3: Cuádruplas

Aunque el C3 se escribe en texto, internamente cada instrucción se almacena en una estructura encapsulada. El número de campos da el nombre:

| Representación | Campos | Memoria usada | Complejidad de algoritmos |
|---|---|---|---|
| Terceto | 3 | poca | más compleja |
| **Cuádrupla** | **4** | **media** | **poca ← usada en el curso** |
| Quíntupla | 5 | mucha | simple |

### Estructura de una Cuádrupla

```
| opCode | Dir1 | Dir2 | Dir3 |
```

- `opCode`: número que identifica la operación (constante definida por el programador, ej: `OPSUMA = 3`)
- `Dir1`, `Dir2`, `Dir3`: direcciones de memoria involucradas
- Si un campo no se usa → se marca con `—`

### Ejemplos de cuádruplas

```
; x = y + z
| OPSUMA | Dir(x) | Dir(y) | Dir(z) |

; p = not q
| OPNOT  | Dir(p) | Dir(q) |   —   |
```

### Programa C3 como vector de cuádruplas

```
Índice | opCode   | Dir1       | Dir2   | Dir3
-------|----------|------------|--------|------
  0    | OPCall   | Dir(Lect)  |   —    |  —
  1    | OPNUM    | Dir(k)     |   1    |  —
  2    | ETIQUETA | 1          |   —    |  —
  3    | OPMEI    | Dir(t1)    | Dir(k) | Dir(N)
  4    | OPIF0    | Dir(t1)    |   2    |  —
  5    | OPWRITES | Dir("*")   |   —    |  —
  6    | OPINC    | Dir(k)     |   —    |  —
  7    | OPGOTO   | 1          |   —    |  —
  8    | ETIQUETA | 2          |   —    |  —
  9    | OPRET    |   —        |   —    |  —
```

---

## 12. Resumen rápido de conceptos clave

| Concepto | Definición corta |
|---|---|
| IL | Lenguaje de bajo nivel, independiente de cualquier CPU real |
| VM | Computadora imaginaria que entiende y ejecuta el IL |
| C3 / 3AC | IL más usado; cada instrucción tiene máximo 3 direcciones |
| Temporal (`t1`, `t2`...) | Variable auxiliar creada por el compilador; local a cada procedimiento |
| Etiqueta (`E1:`, `E2:`...) | Marca de salto; ámbito global en todo el programa |
| Front-End | Parte del compilador que analiza el fuente y produce CI |
| Back-End | Parte del compilador que convierte CI a código objeto real |
| Cuádrupla | Estructura de 4 campos numéricos que representa una instrucción C3 |
| `opCode` | Campo de la cuádrupla que identifica la operación |


---

## 13. Cosas importantes que el PDF no cubre explícitamente

---

### Variables temporales y su conteo

Cuando conviertes una expresión compleja, los temporales se numeran secuencialmente y **nunca se reutilizan dentro de la misma expresión**. Regla:

- Si ya usaste `t1` y `t2` en un procedimiento anterior, el siguiente procedimiento **puede volver a empezar desde `t1`** (son locales).
- Pero dentro de un mismo bloque de código **nunca repitas el número**.

---

### Expresiones con llamadas a funciones

Si una expresión incluye una llamada a función, primero se hace el `CALL` y el resultado se recoge en un temporal:

```
; z = factorial(n) + 1
CALL factorial
t1 = resultado   ; el retorno de la función
t2 = 1
z  = t1 + t2
```

---

### For con STEP diferente a 1

El PDF solo muestra `for i = a to b` con incremento de 1. Si el paso es diferente, **no se puede usar INC**:

```
; for i = a to b step k
i = a
Ei:
    t1 = (i ≤ b)
    IF (t1=0) → GOTO Ef
    C3-Sentencias
    i = i + k        ; si k no es 1, no puedes usar INC
    GOTO Ei
Ef:
```

---

### Expresiones booleanas compuestas y cortocircuito

En Java/C el `&&` y `||` hacen **cortocircuito** (si el primer operando ya define el resultado, no evalúa el segundo). En C3 básico **no hay cortocircuito**, se evalúa todo siempre. Esto importa si el profe pregunta la diferencia.

---

### Anidamiento de estructuras

Cuando tienes un `if` dentro de un `while`, las etiquetas siguen siendo globales y consecutivas. **Nunca se repiten aunque estén en distintos niveles de anidamiento:**

```
; while(...){ if(...){ } }

E1:                      ← inicio del while
    C3-ExprBoole(tk)
    IF (tk=0) → GOTO E2
    C3-ExprBoole(tm)     ← if adentro del while
    IF (tm=0) → GOTO E3
    C3-Sentencias
E3:                      ← fin del if
    GOTO E1
E2:                      ← fin del while
```

---

### Lo que el C3 NO puede hacer (en este curso)

- **No hay arrays ni punteros**
- **No hay paso de parámetros** a procedimientos (todas las variables son globales)
- **No hay tipos de datos**, todo es entero
- **No hay recursión** explícitamente definida (aunque técnicamente `CALL` podría llamarse a sí mismo)

---

### Diferencia entre `=` como asignación y `=` como comparación

En C3 el **contexto** lo define:

```
x = y          ; ASIGNACIÓN  → x toma el valor de y
t1 = (x = y)   ; COMPARACIÓN → t1 vale 1 si x es igual a y, 0 si no
```

> La diferencia es que la comparación siempre va **dentro de paréntesis**.





## 14. Teoría: Analizador Léxico y Diagramas de Transiciones

---

### ¿Qué es el Analizador Léxico?

- Es la **primera fase** del compilador (también llamado Scanner, Lexer o Analex).
- Lee el Programa Fuente **char a char**.
- Agrupa caracteres consecutivos en **tokens**.
- Envía los tokens **uno por uno** al Parser (Analizador Sintáctico).
- Es considerada la **fase más lenta** del compilador (por leer char a char).
- **NO verifica el orden** de los tokens, solo los reconoce.
- Si el fuente es erróneo, igual reconoce tokens sin comunicar error.

**Tareas adicionales del Analex:**
- Ignorar los **comentarios** del Programa Fuente.
- Instalar (insertar) las **StringCtte's e ID's** en la Tabla de Símbolos.

---

### Conceptos clave: Token, Lexema y Patrón

| Concepto | Definición | Ejemplo |
|---|---|---|
| **Token** | Categoría o tipo al que pertenece un símbolo | `NUM`, `ID`, `ASSIGN`, `IF` |
| **Lexema** | La cadena concreta que fue reconocida | `640`, `Velocidad`, `:=`, `if` |
| **Patrón** | Regla que describe qué lexemas pertenecen a un token | `NUM` = uno o más dígitos |

> Un **token** es como una clase, el **lexema** es una instancia de esa clase.

**Ejemplo:**
```
F = "x := 640 * Velocidad – 456;"

x          →  ID
:=         →  ASSIGN
640        →  NUM
*          →  POR
Velocidad  →  ID
–          →  MENOS
456        →  NUM
;          →  PTOCOMA
```

---

### Tipos de errores por fase del compilador

| Fase | Tipo de error que detecta | Ejemplo |
|---|---|---|
| **Léxico** | Carácter que no pertenece al alfabeto del lenguaje | `@`, `#` en un lenguaje que no los define |
| **Sintáctico** | Token en posición incorrecta | `int x = ;` |
| **Semántico** | Tipos incompatibles, variables no declaradas | `int x = 1 + "hola"` → Type mismatch |

> **Error léxico:** ocurre cuando el Analex encuentra un carácter (o secuencia) que no puede asociar a ningún token definido en el lenguaje.

---

### ¿Qué es un Diagrama de Transiciones (DT)?

Un **Diagrama de Transiciones** es un grafo dirigido que modela el comportamiento del Analizador Léxico para reconocer tokens. Es equivalente a un **Autómata Finito**.

**Componentes:**

| Componente | Descripción | Notación gráfica |
|---|---|---|
| **Estado** | Situación en la que se encuentra el reconocedor | Círculo `○` |
| **Estado inicial** | Donde empieza el reconocedor | Círculo con flecha entrante `→○` |
| **Estado final** | El token fue reconocido exitosamente | Círculo doble `⊙` |
| **Estado de error** | Secuencia inválida | Estado `E` o trampa |
| **Transición** | Cambio de estado al leer un carácter | Flecha etiquetada con el char |

---

### Retroceso del cabezal (Retract)

Cuando el DT llega a un estado final, a veces ya leyó **un carácter de más** (el que hizo que se detuviera). En ese caso debe **retroceder** el cabezal una posición.

- Se indica con un `*` en el estado final, o con la notación **retract** / **ret**.
- Significa: "este carácter no pertenece al token actual, devolvélo a la cinta".

```
; Ejemplo: reconocer NUM (dígitos)
; Al leer el primer no-dígito sabemos que el NUM terminó,
; pero ese char no-dígito pertenece al siguiente token → retract

Estado 0 --dígito--> Estado 1 --dígito--> Estado 1 --no-dígito--> Estado 2*
                                                                   (final, retract)
```

---

### Convenciones de notación en los DT

```
Letra        → cualquier letra del alfabeto (a-z, A-Z)
Dígito       → 0-9
otro / other → cualquier carácter que no está en las demás transiciones
EOF          → fin de archivo
ε            → transición vacía (sin consumir carácter)
*            → estado final con retroceso del cabezal
```

---

### Estructura general de un DT

```
        char1          char2
  q0 ---------> q1 ---------> q2 (final)
  |
  | otro
  v
  qE (error)
```

- Desde el **estado inicial** `q0` se leen caracteres.
- Cada carácter lleva a un nuevo estado.
- Al llegar a un **estado final** se retorna el token reconocido.
- Si no hay transición válida → **error léxico**.

---

### Plantillas base de DT más comunes

#### NUM → uno o más dígitos

```
        dígito        dígito         otro
  q0 ----------> q1 ----------> q1 --------> q2* (retract, return NUM)
```

> `q1` tiene un lazo con `dígito` (se queda en q1 mientras siga leyendo dígitos).

#### ID → empieza con letra, continúa con letras o dígitos

```
        letra       letra|dígito        otro
  q0 --------> q1 --------------> q1 --------> q2* (retract, return ID)
```

#### Palabra reservada vs ID

El DT reconoce todo como ID, luego se consulta la **Tabla de Símbolos** para ver si es una palabra reservada (`if`, `while`, `for`...).

---

### Cómo diseñar un DT paso a paso

1. **Leer bien la definición** de cada token.
2. **Identificar el primer carácter** que distingue cada token → eso define las primeras transiciones desde `q0`.
3. **Trazar el camino feliz** (el caso válido).
4. **Agregar lazos** donde el mismo tipo de carácter puede repetirse.
5. **Cerrar con estado final** cuando ya se reconoció el token.
6. **Decidir si hay retract** (¿el último carácter leído pertenece al token o no?).
7. **Agregar transiciones de error** para caracteres no esperados.

---

### Ejemplo completo: NUM, ID y operadores simples

```
Tokens: NUM (dígitos), ID (letra seguida de letra|dígito), PLUS (+), ASSIGN (=)

Desde q0:
  - dígito  → q1 (reconociendo NUM)
  - letra   → q2 (reconociendo ID)
  - '+'     → q5 (final, return PLUS)
  - '='     → q6 (final, return ASSIGN)
  - otro    → qE (error léxico)

Desde q1:
  - dígito  → q1 (lazo, sigue el NUM)
  - otro    → q3* (final con retract, return NUM)

Desde q2:
  - letra|dígito → q2 (lazo, sigue el ID)
  - otro         → q4* (final con retract, return ID)
```

---

### DT como aplicación: recorrer una cinta

Algunos ejercicios no piden reconocer tokens sino **recorrer toda la cinta** y contar o procesar algo. En ese caso el DT funciona como un programa completo:

- Tiene un **estado contador** o variable acumuladora.
- Recorre hasta encontrar **EOF**.
- El estado final devuelve el resultado acumulado.

#### Ejemplo: contar palabras en la cinta

```
; Palabra = subsecuencia sin espacios ni EOF

Variables: contador = 0

Desde q0 (fuera de palabra):
  - espacio/EOF → q0 (lazo, sigue buscando)
  - otro char   → q1 (entró a una palabra), contador++

Desde q1 (dentro de una palabra):
  - no-espacio y no-EOF → q1 (lazo, sigue la palabra)
  - espacio             → q0 (terminó la palabra)
  - EOF                 → q2 (final, return contador)
```

#### Ejemplo: contar palabras que empiezan con 'A'

```
Variables: contador = 0

Desde q0 (fuera de palabra):
  - espacio/EOF → q0
  - 'A'         → q1, contador++   (entró palabra con A)
  - otra letra  → q2               (entró palabra sin A)

Desde q1 (dentro de palabra que empezó con A):
  - no-espacio y no-EOF → q1
  - espacio             → q0
  - EOF                 → qF (return contador)

Desde q2 (dentro de palabra que NO empezó con A):
  - no-espacio y no-EOF → q2
  - espacio             → q0
  - EOF                 → qF (return contador)
```

---

### Reglas importantes para los ejercicios de DT

- **Un estado final por token** reconocido (o uno compartido con retract).
- **Nunca dos tokens se reconocen en el mismo estado final** sin distinción.
- Si un lexema puede ser de dos tipos (ej: `123h` es HEX pero `123` es NUM), el DT debe **diferir la decisión** hasta leer suficientes caracteres.
- El **orden de prioridad** importa: si una cadena puede ser HEX o NUM, el DT debe reconocer primero el más específico (HEX).
- Los estados con `*` (retract) devuelven el último carácter a la cinta antes de retornar el token.

---

### Resumen: diferencias entre tipos de estados finales

| Tipo | Significado | ¿Consume el último char? |
|---|---|---|
| Final normal | El último char leído pertenece al token | Sí |
| Final con retract (`*`) | El último char NO pertenece al token | No, lo devuelve |
| Estado error | Carácter inválido, se lanza error léxico | — |

---

### Tokens especiales frecuentes en los ejercicios

| Token | Definición típica |
|---|---|
| `NUM` | Uno o más dígitos (`0-9`) |
| `ID` | Empieza con letra, continúa con letra o dígito |
| `HEX` | Dígitos hexadecimales (`0-9`, `A-F`), a veces termina en `h` |
| `FLOAT` | Número con punto decimal, ej: `3.14`, `.5`, `3.` |
| `IDNUM` | Alternación estricta letra-dígito o dígito-letra |
| `LETNUM` | Empieza con letras, termina con dígitos |
| `NUMLET` | Empieza con dígitos, termina con letras |


## 15. Teoría adicional: Cinta, Variables en DT y EOF

---

## 15.1 La Cinta de Caracteres (Tape)

La **Cinta de Caracteres** es la estructura de datos que representa el Programa Fuente para el Analizador Léxico.

```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┐
│ x │   │ = │   │ 6 │ 4 │ 0 │ ; │EOF│
└───┴───┴───┴───┴───┴───┴───┴───┴───┘
  ↑
cabezal
```

- Cada celda contiene **un carácter**.
- El **cabezal** apunta a la celda actual y avanza una posición cada vez que lee.
- La cinta siempre termina con **EOF** (End Of File).
- El cabezal **nunca retrocede solo** → solo retrocede si el DT hace un **retract**.
- El DT recorre la cinta de **izquierda a derecha**, celda por celda.

### Operaciones del cabezal

| Operación | Descripción |
|---|---|
| `leer()` | Lee el carácter actual y avanza el cabezal |
| `retract()` | Retrocede el cabezal una posición (el char no fue consumido) |
| `EOF` | Carácter especial que indica que no hay más caracteres |

### Ciclo de vida del Analex sobre la cinta

```
1. Cabezal apunta a la primera celda
2. El DT empieza en su estado inicial
3. Lee un char → cambia de estado
4. Repite hasta llegar a un estado final → retorna el token
5. El DT vuelve al estado inicial y sigue leyendo
6. Cuando lee EOF → termina el análisis léxico
```

---

## 15.2 Variables dentro de un DT

Los DT clásicos solo reconocen si una cadena pertenece o no a un lenguaje. Pero en compiladores, el DT necesita también **acumular información** mientras recorre la cinta.

Las variables se anotan **sobre las transiciones** o en los **estados**, y se actualizan cada vez que el cabezal pasa por ahí.

### Notación usada en el curso

Las acciones se escriben sobre la flecha de transición, debajo del carácter que la dispara:

```
          char
  q0 ────────────> q1
       acción()
```

### Variables más comunes en los ejercicios

| Variable | Uso típico |
|---|---|
| `cnt` / `contador` | Contar palabras, tokens, ocurrencias |
| `word` / `actual` | Acumular la palabra actual carácter a carácter |
| `max` | Guardar la palabra o valor máximo encontrado |
| `len` / `longitud` | Longitud de la palabra actual |

### Ejemplo: contar palabras en la cinta

```
Variables: cnt = 0

        espacio/EOF          no-espacio, no-EOF
  q0 ───────────────> q0    
  q0 ───────────────────────> q1
                               cnt++
        no-espacio, no-EOF
  q1 ───────────────────────> q1

        espacio
  q1 ───────────────> q0

        EOF
  q1 ───────────────> qF (return cnt)

        EOF
  q0 ───────────────> qF (return cnt)
```

### Ejemplo: acumular la palabra más larga

```
Variables: actual = "", max = "", len = 0, maxLen = 0

        no-espacio, no-EOF
  q1 ───────────────────────> q1
       actual += char
       len++

        espacio
  q1 ───────────────> q0
       if (len > maxLen):
           max = actual
           maxLen = len
       actual = ""
       len = 0
```

### Reglas para usar variables en DT

- Las variables se **inicializan antes** de que el DT empiece a correr.
- Las acciones sobre las transiciones se ejecutan **cada vez** que esa transición se dispara.
- Las acciones sobre los estados finales se ejecutan **una sola vez** al llegar.
- Se puede usar cualquier lógica (`if`, asignación, incremento) dentro de las acciones.

---

## 15.3 Manejo de EOF en el DT

### ¿Qué es EOF?

EOF es un carácter especial al final de la cinta que indica que no hay más caracteres. No es un espacio ni un salto de línea, es un símbolo propio.

### Cuándo usar EOF como transición de cierre

EOF se usa como transición de cierre cuando el token **puede terminar al final de la cinta** sin necesidad de un carácter delimitador:

```
; NUM puede terminar en EOF (no necesita un char extra para saber que terminó)

        dígito        otro / EOF
  q0 --------> q1 ────────────> q2* (retract si fue otro, return NUM)
```

### Cuándo EOF es un error

EOF es un error cuando el token **requiere un carácter de cierre** que nunca llegó:

```
; String literal: empieza con " y debe cerrar con "
; Si llega EOF antes del cierre → error léxico

        "                char normal            "
  q0 ──────> q1 ─────────────────────> q1 ──────> q2 (return STRING)
                                        |
                                       EOF
                                        ↓
                                       qE (ERROR: string no cerrado)
```

### Tabla resumen: EOF en distintos contextos

| Situación | Comportamiento correcto |
|---|---|
| Leyendo NUM y llega EOF | Cerrar el NUM, hacer retract, return NUM |
| Leyendo ID y llega EOF | Cerrar el ID, hacer retract, return ID |
| Leyendo STRING y llega EOF | Error léxico (string no cerrado) |
| Fuera de cualquier token y llega EOF | Retornar token especial EOF al parser |
| Recorriendo cinta completa (ejercicios 8-12) | EOF es la condición de parada, ir al estado final y retornar resultado |

### EOF en ejercicios de recorrido de cinta (8-12)

En estos ejercicios el DT no busca tokens, sino que **procesa toda la cinta**. EOF siempre es la condición de parada y lleva al estado final que retorna el resultado:

```
; Patrón general para ejercicios de recorrido

  q_cualquiera ──EOF──> qFinal (return resultado)
```

> **Regla práctica:** en ejercicios de recorrido, cada estado debe tener una transición con EOF que lleve al estado final. Si no la tiene, el DT queda "colgado" al llegar al fin de la cinta.

---

## 15.4 Resumen: DT clásico vs DT con variables

| Característica | DT clásico | DT con variables |
|---|---|---|
| Objetivo | Reconocer si una cadena pertenece a un token | Reconocer Y acumular información |
| Salida | Nombre del token | Token + valor acumulado (contador, string, etc.) |
| Uso en compiladores | Reconocimiento de tokens | Recorrido de cinta con procesamiento |
| Acciones en transiciones | No tiene | Sí: `cnt++`, `word += char`, `if (...)` |
| Estado final | `return TOKEN` | `return resultado` |




