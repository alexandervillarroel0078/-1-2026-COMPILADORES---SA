# Apuntes de Compiladores

## BLOQUE 1 — ¿Qué es un compilador?

Un compilador es un programa que lee tu código fuente (Java, C++, etc) y lo traduce a otro lenguaje (código máquina o código intermedio). No ejecuta nada — solo traduce.

```
Tu código Java → COMPILADOR → archivo .jar
```

---

## BLOQUE 2 — Las 6 fases del compilador

El compilador trabaja en 6 pasos en orden:

1. **Analizador Léxico** → lee char por char, agrupa en tokens
2. **Analizador Sintáctico** → verifica que el orden de tokens sea correcto
3. **Analizador Semántico** → verifica que los tipos sean compatibles
4. **Generador Código Intermedio** → traduce a IL (C3)
5. **Optimador** → reduce el código
6. **Generador Código Objeto** → produce el ejecutable final

Los primeros 3 son el **Front-End** (análisis).
Los últimos 3 son el **Back-End** (síntesis).

---

## BLOQUE 3 — El Código de Tres Direcciones (C3)

Es el lenguaje intermedio más usado. Cada instrucción usa máximo 3 variables. No hay constantes en operaciones — todo va en variables.

**Reglas básicas:**
```
x = 5           // asignación número
x = y           // asignación variable
x = y + z       // operación (máximo 3 variables)
x = y + 5       // ❌ INCORRECTO
t1 = 5          // ✓ primero asignás la constante
x = y + t1      // ✓ luego operás
```

**Booleanos — solo 0 y 1:**
```
0 = false
1 = true (cualquier ≠ 0)
```

**Comparaciones:**
```
t1 = (x > y)    // t1=1 si x>y, t1=0 si no
t1 = (x = y)    // t1=1 si son iguales, t1=0 si no
```

**Saltos:**
```
goto E1                  // salta siempre a E1
if (t1=1) goto E1        // salta si t1 vale 1
if (t1=0) goto E1        // salta si t1 vale 0
```

**Regla del if:**
- Si la condición **SE CUMPLE** → salta
- Si **NO** se cumple → sigue la siguiente línea

**I/O:**
```
read(x)          // leer del teclado
write(x)         // mostrar variable
writeS("texto")  // mostrar texto
NL               // nueva línea
```

**Inc/Dec:**
```
inc x    // x = x + 1
dec x    // x = x - 1
```

**Procedimientos:**
```
call MiProc    // llamar procedimiento
RET            // retornar
```

---

## BLOQUE 4 — Convertir Java a C3

El proceso siempre es el mismo:

**while:**
```java
while (x > 0) { sentencias; }
```
```
E1:
  t1 = 0
  t2 = (x > t1)
  if (t2=0) goto E2
  C3-Sentencias
  goto E1
E2:
```

**do-while:**
```java
do { sentencias; } while (x > 0);
```
```
E1:
  C3-Sentencias
  t1 = 0
  t2 = (x > t1)
  if (t2=1) goto E1
```

**if:**
```java
if (x > 0) { sentencias; }
```
```
  t1 = 0
  t2 = (x > t1)
  if (t2=0) goto E1
  C3-Sentencias
E1:
```

**if-else:**
```java
if (x > 0) { sent1; } else { sent2; }
```
```
  t1 = 0
  t2 = (x > t1)
  if (t2=0) goto En
  C3-Sentencias1
  goto Ef
En:
  C3-Sentencias2
Ef:
```

**break dentro de while:**
```java
while(true) { if (x==0) break; }
```
```
E1:
  t1 = 0
  t2 = (x = t1)
  if (t2=1) goto Ef   // break = goto Ef
  goto E1
Ef:
```

**par/impar:**
```java
if (x % 2 == 0)  // es par
```
```
t1 = 2
t2 = x mod t1    // resto de dividir x entre 2
t3 = 0
t4 = (t2 = t3)   // ¿el resto es 0?
if (t4=0) goto En // no → impar
```

---

## BLOQUE 5 — Esquemas de Traducción

Son plantillas genéricas. En vez de código concreto usás nombres genéricos:

```
C3-ExprBoole(tk)    // traducción de una expresión booleana
                    // tk = temporal donde queda el resultado
C3-Sentencias       // traducción de un bloque de sentencias
Ei, Ef, En          // etiquetas genéricas
```

El proceso para armar un esquema:
1. Leés el enunciado paso a paso
2. Cada frase → una o más líneas de C3
3. Verificás con valores concretos

---

## BLOQUE 6 — Diagramas de Transiciones (DT)

Es cómo el Analizador Léxico reconoce tokens. Lee la cinta char por char.

**La cinta:**
```
x = 2 7 ; EOF
↑
cabezal (lee de izquierda a derecha)
```

**Elementos del DT:**

| Elemento | Qué es |
|---|---|
| Círculo | Estado |
| Doble círculo | Estado final — acepta token |
| Flecha con char | Si leo ese char → cambio de estado |
| Loop | Me quedo en el mismo estado |
| otro | Char no esperado — acepta y avanza |
| Espacio | espacio, TAB o salto de línea |
| EOF | fin del archivo |
| FIN | token especial — llegué al EOF |
| ERROR | token especial — no reconocí nada |

**Reglas importantes:**
- Solo 1 estado inicial (el 0)
- Todos los estados son diferentes
- Todo estado con salidas debe tener arista `otro`
- No puede haber ambigüedad — dos flechas con los mismos chars
- Debe tener al menos un estado final

**Proceso para armar un DT:**
1. ¿Qué puede ser el primer char? → flechas desde q0
2. ¿Qué puede venir después? → estados intermedios
3. ¿Cuándo termina el token? → estado final (doble círculo)

**Ejemplo — reconocer NUM e ID:**
```
q0 → dígito → q1 → dígito → q1 → otro → qf1 (NUM)
q0 → letra  → q2 → letra/dígito → q2 → otro → qf2 (ID)
```

---

## BLOQUE 7 — Tokens ERROR y FIN

Todo DT real necesita manejar estos dos casos:
```
FIN   → cuando el cabezal llega al EOF
ERROR → cuando lee algo que no encaja en ningún token
```

La estructura general siempre es:
```
      q0
     / | \
  letra díg otro
   |    |    |
   ...  ...  ERROR
        |
       FIN (cuando llega EOF)
```

---

## Resumen de lo que entra en el práctico

| Ejercicio | Qué necesitás saber |
|---|---|
| 1-2 | Teoría fases del compilador |
| 3-7 | Armar esquemas de traducción |
| 8-10 | Convertir Java a C3 |
| DT 1-7 | Dibujar DT para reconocer tokens |
| DT 8-12 | DT que recorren la cinta y devuelven un valor |

---

## Lo que falta

### 1. Definiciones Regulares (DEFREG)

Son como "atajos" para describir tokens matemáticamente:
```
Digito → 0|1|2|3|4|5|6|7|8|9
Letra  → a|b|c|...|z
NUM    → Digito+        // uno o más dígitos
ID     → Letra+         // una o más letras
```

Los operadores son:

| Operador | Significa |
|---|---|
| \| | o (uno u otro) |
| * | 0 o más veces |
| + | 1 o más veces |
| . | concatenación |

### 2. La Cinta de Caracteres

El Analizador Léxico usa una "cinta" para leer el programa fuente:
```
f o r β i = 3 β d o EOF
↑
cabezal
```

Métodos:
```java
cc()       // lee el char actual
avanzar()  // mueve el cabezal a la derecha
init()     // vuelve al inicio
```

### 3. Tokens ERROR y FIN en el DT

Todo DT completo necesita manejar estos:
```
FIN   → llega al EOF → fin del archivo
ERROR → char que no encaja en ningún token
```

La estructura general de todo DT:
```
        q0
      /  |  \
  letra díg  otro→ERROR
   |    |
   ...  ...
        |
       EOF→FIN
```

### 4. Espacio en el DT

El compilador toma como espacio 3 cosas:
```
Espacio → β (barra espaciadora) | TAB | EOLN
```
En el DT desde q0 el espacio se ignora — el cabezal avanza sin cambiar de estado.

---

## ¿Entra en el práctico?

| Tema | ¿Entra? |
|---|---|
| DEFREG | Posiblemente — los DT 1-7 pueden pedirte definirlas |
| Cinta | Sí — DT 8-12 trabajan con cinta |
| ERROR y FIN | Sí — todo DT completo los necesita |
| Espacio | Sí — separador de tokens |