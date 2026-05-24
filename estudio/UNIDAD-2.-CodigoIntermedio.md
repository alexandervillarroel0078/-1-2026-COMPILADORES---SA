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



 