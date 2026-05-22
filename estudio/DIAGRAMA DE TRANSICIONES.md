## DIAGRAMA DE TRANSICIONES

---

### 1. (Diagrama de Transiciones)
Dibujar un dt que, trabajando con Letras y Dígitos, reconozca los siguientes tokens:
- NUM → Números enteros (solamente Dígitos).
- HEX → Núm. hexadecimales (deben tener al menos una letra hexa o pueden terminar en "h"). Por ejemplo: A12h, 123h, B1, …
- ID → Combinación de letras y dígitos que no son NUM o HEX

**R:**
```
### PASO 1 — Clases de caracteres

| Clase      | Descripción                      |
|------------|----------------------------------|
| dígito     | 0-9                              |
| letraHex   | A\|B\|C\|D\|E\|F                 |
| 'h'        | la h especial que termina HEX    |
| letraNoHex | G\|I\|J\|...\|Z (todo excepto h) |
| otro       | EOF o separador                  |
 
### PASO 2 — Estados (trabajos)
 
| Estado | Significado                | Observación         |
|--------|----------------------------|---------------------|
| q0     | "no sé nada todavía"       | inicio              |
| q1     | "llevo solo dígitos"       | puede ser NUM o HEX |
| q2     | "llevo letras hex (A-F)"   | va camino a HEX     |
| q3     | "terminé en h"             | confirma HEX        |
| q4     | "llevo letraNoHex"         | es ID sí o sí       |
 
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

**R:**
```
### PASO 1 — Clases de caracteres

| Clase  | Descripción                    |
|--------|--------------------------------|
| dígito | 0,1,2,...,9                    |
| letra  | A,B,C...Z, a,b,c,...,z         |
| otro   | EOF o separador                |
 
### PASO 2 — Estados (trabajos)
 
| Estado | Significado                              | Observación          |
|--------|------------------------------------------|----------------------|
| q0     | "no sé nada todavía"                     | inicio               |
| q1     | "llevo solo dígitos"                     | puede ser NUM o NUMLET |
| q2     | "llevo solo letras"                      | puede ser LETNUM     |
| q3     | "empecé con dígitos, vi letra"           | va camino a NUMLET   |
| q4     | "empecé con letras, vi dígito"           | va camino a LETNUM   |
| q5     | "alterné más de una vez"                 | es ID sí o sí        |
 
### PASO 4 — Tabla de transición
 
| Estado | letra | dígito | otro    |
|--------|-------|--------|---------|
| q0     | q2    | q1     | ERROR   |
| q1     | q3    | q1     | NUM✓    |
| q2     | q2    | q4     | ID✓     |
| q3     | q3    | q5     | NUMLET✓ |
| q4     | q5    | q4     | LETNUM✓ |
| q5     | q5    | q5     | ID✓     |
```
---

### 3. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos:
- NUMP → Números enteros cuya cantidad de dígitos significativos es par
- NUMI → Números enteros cuya cantidad de dígitos significativos es impar

Por "dígito significativo" se refiere a que no se tomen en cuenta los ceros a la izquierda.

**R:**
```
### PASO 1 — Clases de caracteres

| Clase  | Descripción     |
|--------|-----------------|
| dígito | 1,2,...,9       |
| cero   | 0               |
| otro   | EOF o separador |
 
### PASO 2 — Estados (trabajos)
 
| Estado | Significado                        |
|--------|------------------------------------|
| q0     | "no sé nada todavía"               |
| q1     | "ceros a la izquierda"             |
| q2     | "llevo cantidad PAR de dígitos significativos"  |
| q3     | "llevo cantidad IMPAR de dígitos significativos"|
 
### PASO 4 — Tabla de transición
 
| Estado | cero | dígito | otro  |
|--------|------|--------|-------|
| q0     | q1   | q3     | ERROR |
| q1     | q1   | q3     | ERROR |
| q2     | q3   | q3     | NUMP✓ |
| q3     | q2   | q2     | NUMI✓ |
```
---

### 4. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos y Letras. Estos son:
- NUMO → Números octales, pueden terminar con la vocal 'O'.
- NUM → Números enteros que no son NUMO. Opcionalmente pueden terminar en 'd'.
- ID → combinación de letras y/o dígitos que no son NUM ni NUMO (debe tener al menos una letra)

**R:**
```
### PASO 1 — Clases de caracteres

| Clase     | Descripción              |
|-----------|--------------------------|
| octal     | 0-7                      |
| noOctal   | 8\|9                     |
| 'O'       | letra O especial         |
| 'd'       | letra d especial         |
| otraLetra | A-Z excepto O y d        |
| otro      | EOF o separador          |
 
### PASO 2 — Estados (trabajos)
 
| Estado | Significado                              |
|--------|------------------------------------------|
| q0     | "no sé nada"                             |
| q1     | "llevo solo octales" → puede ser NUMO o NUM |
| q2     | "llevo dígito no octal" → solo NUM       |
| q3     | "terminé en O" → confirma NUMO           |
| q4     | "terminé en d" → confirma NUM            |
| q5     | "llevo letra" → va camino a ID           |

### PASO 4 — Tabla de transición
 
| Estado | octal | noOctal | 'O' | 'd' | otraLetra | otro  |
|--------|-------|---------|-----|-----|-----------|-------|
| q0     | q1    | q2      | q5  | q5  | q5        | ERROR |
| q1     | q1    | q2      | q3  | q4  | q5        | NUMO✓ |
| q2     | q2    | q2      | q5  | q4  | q5        | NUM✓  |
| q3     | q5    | q5      | q5  | q5  | q5        | NUMO✓ |
| q4     | q5    | q5      | q5  | q5  | q5        | NUM✓  |
| q5     | q5    | q5      | q5  | q5  | q5        | ID✓   |
```
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
```
### PASO 1 — Clases de caracteres

| Clase  | Descripción     |
|--------|-----------------|
| letra  | A-Z, a-z        |
| dígito | 0-9             |
| otro   | EOF o separador |

### PASO 2 — Estados (trabajos)

| Estado | Significado                                        |
|--------|----------------------------------------------------|
| q0     | "no sé nada"                                       |
| q1     | "llevo solo dígitos" → puede ser NUM               |
| q2     | "llevo solo letras" → puede ser ID                 |
| q3     | "alternando: último fue dígito, empecé con letra"  |
| q4     | "alternando: último fue letra, empecé con dígito"  |
| q5     | "rompí alternación" → es ID sí o sí                |

### PASO 4 — Tabla de transición

| Estado | letra | dígito | otro   |
|--------|-------|--------|--------|
| q0     | q2    | q1     | ERROR  |
| q1     | q4    | q1     | NUM✓   |
| q2     | q2    | q3     | ID✓    |
| q3     | q3    | q5     | IDNUM✓ |
| q4     | q5    | q4     | IDNUM✓ |
| q5     | q5    | q5     | ID✓    |
```
---

### 6. (Diagrama de Transiciones)
Un lenguaje toma como tokens a toda subsecuencia formada con solo Letras. Estos son:
- IDBA → Termina con la subsecuencia "BA".
- IDA → Es 'A' o, termina en 'A' pero el anterior char no es 'B'.
- ID → subsecuencia de Letras que no es IDBA ni IDA.

**R:**
```
### PASO 1 — Clases de caracteres

| Clase     | Descripción               |
|-----------|---------------------------|
| 'A'       | letra A especial          |
| 'B'       | letra B especial          |
| otraLetra | C-Z (todo excepto A y B)  |
| otro      | EOF o separador           |

### PASO 2 — Estados (trabajos)

| Estado | Significado                              |
|--------|------------------------------------------|
| q0     | "no sé nada"                             |
| q1     | "último char fue otraLetra"              |
| q2     | "último char fue 'B'"                    |
| q3     | "último char fue 'A' sin B antes"        |
| q4     | "último char fue 'BA'"                   |

### PASO 4 — Tabla de transición

| Estado | 'A' | 'B' | otraLetra | otro  |
|--------|-----|-----|-----------|-------|
| q0     | q3  | q2  | q1        | ERROR |
| q1     | q3  | q2  | q1        | ID✓   |
| q2     | q4  | q2  | q1        | ID✓   |
| q3     | q3  | q2  | q1        | IDA✓  |
| q4     | q3  | q2  | q1        | IDBA✓ |
```
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
```
### PASO 1 — Clases de caracteres

| Clase  | Descripción        |
|--------|--------------------|
| dígito | 0-9                |
| '.'    | punto especial     |
| 'F'    | letra F especial   |
| letra  | A-Z excepto F      |
| otro   | EOF o separador    |

### PASO 2 — Estados (trabajos)

| Estado | Significado                                        |
|--------|----------------------------------------------------|
| q0     | "no sé nada"                                       |
| q1     | "llevo solo dígitos" → puede ser NUM               |
| q2     | "vi punto después de dígitos" → parte decimal      |
| q3     | "vi punto al inicio" → esperando dígitos           |
| q4     | "llevo dígitos después del punto" → FLOAT o DOUBLE |
| q5     | "llevo letra" → va camino a ID                     |
| q6     | "terminé en F" → confirma FLOAT                    |

### PASO 4 — Tabla de transición

| Estado | dígito | '.'   | 'F'   | letra | otro    |
|--------|--------|-------|-------|-------|---------|
| q0     | q1     | q3    | q5    | q5    | ERROR   |
| q1     | q1     | q2    | q5    | q5    | NUM✓    |
| q2     | q4     | ERROR | q5    | q5    | DOUBLE✓ |
| q3     | q4     | ERROR | ERROR | ERROR | ERROR   |
| q4     | q4     | ERROR | q6    | q5    | DOUBLE✓ |
| q5     | q5     | ERROR | q5    | q5    | ID✓     |
| q6     | q5     | ERROR | q5    | q5    | FLOAT✓  |
```
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