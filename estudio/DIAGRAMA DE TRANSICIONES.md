
## DIAGRAMA DE TRANSICIONES

### 1. (Diagrama de Transiciones)
Dibujar un dt que, trabajando con Letras y Dígitos, reconozca los siguientes tokens:
- NUM → Números enteros (solamente Dígitos).
- HEX → Núm. hexadecimales (deben tener al menos una letra hexa o pueden terminar en "h"). Por ejemplo: A12h, 123h, B1, …
- ID → Combinación de letras y dígitos que no son NUM o HEX

**R:**
```
## R:

### PASO 1 — Clases de caracteres

| Clase      | Descripción                        |
|------------|------------------------------------|
| dígito     | 0-9                                |
| letraHex   | A\|B\|C\|D\|E\|F                   |
| 'h'        | la h especial que termina HEX      |
| letraNoHex | G\|I\|J\|...\|Z (todo excepto h)   |
| otro       | EOF o separador                    |

---

### PASO 2 — Estados (trabajos)

| Estado | Significado                  | Observación          |
|--------|------------------------------|----------------------|
| q0     | "no sé nada todavía"         | inicio               |
| q1     | "llevo solo dígitos"         | puede ser NUM o HEX  |
| q2     | "llevo letras hex (A-F)"     | va camino a HEX      |
| q3     | "terminé en h"               | confirma HEX         |
| q4     | "llevo letraNoHex"           | es ID sí o sí        |

---

### PASO 3 — 3 preguntas por cada estado

**q0** — "no sé nada":
1. ¿Me quedo? → nada, soy el inicio
2. ¿Cambio? → dígito→q1, letraHex→q2, h|letraNoHex→q4
3. ¿Terminé? → otro → ERROR

**q1** — "llevo solo dígitos":
1. ¿Me quedo? → dígito → loop
2. ¿Cambio? → letraHex→q2, h→q3, letraNoHex→q4
3. ¿Terminé? → otro → NUM✓

**q2** — "llevo letras hex":
1. ¿Me quedo? → dígito|letraHex → loop
2. ¿Cambio? → h→q3, letraNoHex→q4
3. ¿Terminé? → otro → HEX✓

**q3** — "terminé en h":
1. ¿Me quedo? → nada, h es terminal
2. ¿Cambio? → cualquier letra|dígito → q4
3. ¿Terminé? → otro → HEX✓

**q4** — "llevo letraNoHex":
1. ¿Me quedo? → dígito|letraHex|h|letraNoHex → loop
2. ¿Cambio? → nada, ya es ID sí o sí
3. ¿Terminé? → otro → ID✓

---

### PASO 4 — Tabla de transición

| Estado | dígito | letraHex | 'h' | letraNoHex | otro  |
|--------|--------|----------|-----|------------|-------|
| q0     | q1     | q2       | q4  | q4         | ERROR |
| q1     | q1     | q2       | q3  | q4         | NUM✓  |
| q2     | q2     | q2       | q3  | q4         | HEX✓  |
| q3     | q4     | q4       | q4  | q4         | HEX✓  |
| q4     | q4     | q4       | q4  | q4         | ID✓   |

```

---

### 2. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos y Letras. Estos son:
- NUM → números enteros
- LETNUM → Empieza con Letra(s) y termina con Digito(s).
- NUMLET → Empieza con Digito(s) y termina con Letra(s).
- ID → (ninguno de los anteriores).

Dibuje un Diagrama de Transiciones, para reconocer a (los nombres de) estos Tokens.

**R:**

---

### 3. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos:
- NUMP → Números enteros cuya cantidad de dígitos significativos es par
- NUMI → Números enteros cuya cantidad de dígitos significativos es impar

Dibuje un dt para reconocer a (los nombres de) estos Tokens.

Por "dígito significativo", en este caso, se refiere a que no se tomen en cuenta los ceros a la izquierda del NUM.

**R:**

---

### 4. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos y Letras. Estos son:
- NUMO → Números octales, los cuales pueden terminar con la vocal 'O'. Recuerde que los octales solo usan los dígitos 0 al 7.
- NUM → Números enteros que no son un NUMO. Opcionalmente pueden terminar en la letra 'd'.
- ID → combinación de letras y/o dígitos que no son NUM ni NUMO (debe tener al menos una letra)

Dibuje un dt para reconocer a (los nombres de) estos Tokens.

Lexemas de ejemplo:
```
23456  → NUMO (porque solo usa dígitos octales)
23456d → NUM  (porque son solo dígitos y termina en 'd')
78695  → NUM  (son solo dígitos y usa dígitos no-octales)
7563O  → NUMO (solo usa dígitos octales y termina en 'O')
854O   → ID   (al utilizar el 8 ya deja de ser octal)
o1234  → ID   (empieza con letra. No es NUMO ni NUM)
345dd  → ID
AB2C   → ID
```

**R:**

---

### 5. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos y Letras:
- NUM → Números enteros
- IDNUM → Alternación estricta de Letras y Dígitos (al menos 2 chars)
- ID → Ninguno de los anteriores. (Combinación de Letras y Dígitos)

Dibuje un dt para reconocer a (los nombres de) estos Tokens.

Ejemplos:
```
234   → NUM
2d4w5 → IDNUM
dd333 → ID
a3b5  → IDNUM
a4    → IDNUM
Z     → ID
```
/* Alternación estricta: Letra Digito Letra Digito … o Digito Letra Digito Letra … */

**R:**

---

### 6. (Diagrama de Transiciones)
Un lenguaje toma como tokens a toda subsecuencia formada con solo Letras. Estos son:
- IDBA → Termina con la subsecuencia "BA".
- IDA → Es 'A' o, termina en 'A' pero el anterior char no es 'B'.
- ID → subsecuencia de Letras que no es IDBA ni IDA.

Dibuje un dt para reconocer a (los nombres de) estos Tokens.

**R:**

---

### 7. (Diagrama de Transiciones)
Un lenguaje forma sus tokens con todas las cadenas formadas con Letra, Digito y '.'. Estos son:
- NUM → Números enteros
- FLOAT → Números reales informales que terminan en 'F'.
- DOUBLE → Números reales informales que NO terminan en 'F'.
- ID → Combinación de Letras y/o Dígitos, que no es NUM, FLOAT ni DOUBLE.

Note que el punto ('.') puede generar un error: si no forma parte de un FLOAT o un DOUBLE; o si el char que le sigue no es Digito.

Un número es considerado real si y solo si está presente el punto ('.') decimal. Se le dice informal, porque se le permite al programador (si él quiere), ignorar la parte entera o la parte decimal, pero no ambas.

Ejemplos: 0.47, .47, 37.0, 37., 60.00, 223.876

Dibuje un dt, para reconocer a estos Tokens.

**R:**

---

### 8. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra todas las celdas y devuelva (return en un estado final), la cantidad de palabras que hay en la Cinta.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**

---

### 9. (Aplicación de los dt)
En este contexto, llamamos palabra a toda subsecuencia formada con solamente letras. Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra todas las celdas y devuelva la palabra de mayor longitud que está presente en la Cinta. Este dt toma como separador de las palabras a todo char que no sea Letra.

Si la Cinta no tiene ninguna palabra, su dt devolverá "" (cadena vacía).

Por comodidad, asuma que todas las palabras (si existen) tienen diferente longitud.

**R:**

---

### 10. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra todas las celdas y devuelva (return en un estado final) la cantidad de palabras que empiezan con 'A'.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**

---

### 11. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra todas las celdas y devuelva la cantidad de palabras que aparecen dentro de los comentarios de línea. Los comentarios que se usan son los usados por JAVA (//….)

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**

---

### 12. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, de no más de tres estados, que recorra todas las celdas y devuelva (return en un estado final), la cantidad de palabras que hay en la Cinta.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**