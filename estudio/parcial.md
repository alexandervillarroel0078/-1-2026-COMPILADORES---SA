# PARCIALES INF329 SA COMPILADORES — ORDENADOS POR FECHA

---

# PRIMER PARCIAL
## INF329 SA–COMPILADORES. GESTIÓN 1-2022
### Jue 26 de mayo de 2022

**1.** *(Teoría)*

(a) ¿Cuándo se genera un Error Léxico? Ejemplifique

(b) ¿Cuándo se genera un Error Semántico? Ejemplifique

**2.** *(Diagrama de Transiciones)* Un lenguaje toma como tokens a toda subsecuencia formada con **solo Letras**. Estos son:

- **AID** → EMPIEZA con una subsecuencia **PAR** de A'es consecutivas
- **ID** → Secuencia de Letras que no es un AID.

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens..

```
AABG  AID
ADAB  ID

paso 0


| Clase      | Descripción                      |
|------------|----------------------------------|
   letra
   'A'            
   otro      EOF o separador

### PASO 2 — Estados (trabajos) Los que despachan 
 
| Estado | Significado                | Observación         |
|--------|----------------------------|---------------------|
 q0 no se nada todavia 
 q1 solo lleva letra 'A'
 q2 solo llevo letras o a y despacho si es otro
 q3 soy aid 
 q4
 
| Estado | letra | 'A' | otro    |
|--------|-------|--------|---------|
q0   q3 q1 ERROR
q1   q3 q2 ID
q2   q4 q1 AID
q3   q3 q3 ID
q4   q4 q4 AID


```





**3.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra TODA la Cinta y en su único estado final, muestre en consola (print) **la cantidad de palabras** que están **dentro** de los comentarios multilínea de JAVA /* ... */.

Se considera **palabra** a toda subsecuencia que no tiene Espacios ni EOF. Es decir, el Espacio y el EOF son los separadores de las palabras.

---
```
### PASO 0 — Variables
int cp = 0

### PASO 1 — Clases
| Clase   | Descripción        |
|---------|--------------------|
| '/'     | barra              |
| '*'     | asterisco          |
| espacio | espacio            |
| EOF     | fin de cinta       |
| otro    | todo lo demás      |

### PASO 2 — Estados
| Estado | Significado                                    |
|--------|------------------------------------------------|
| q0     | fuera del comentario                           |
| q1     | vi '/' fuera del comentario                    |
| q2     | dentro del comentario, fuera de palabra        |
| q3     | dentro del comentario, dentro de palabra       |
| q4     | dentro del comentario, vi '*' (posible cierre) |
| q5     | final → return cp                              |

### PASO 4 — Tabla de transición
| Estado | '/'  | '*' | espacio | otro          | EOF          |
|--------|------|-----|---------|---------------|--------------|
| q0     | q1   | q0  | q0      | q0            | q5 return cp |
| q1     | q0   | q2  | q0      | q0            | q5 return cp |
| q2     | q3   | q4  | q2      | q3 / cp=cp+1  | q5 return cp |
| q3     | q3   | q4  | q2      | q3            | q5 return cp |
| q4     | q0   | q4  | q2      | q3            | q5 return cp |

```
# PRIMER PARCIAL (R)
## INF329 SA–COMPILADORES. GESTIÓN 1-2022
### Mar 26 de julio de 2022

**1.** Escriba un Esquema de Traducción C3, para la construcción ficticia:

    WHILEX (ExprBoole1, ExprBoole2){
        ODD : Sentencia1;
        EVEN: Sentencia2;
        Sentencia3;        //Siempre se ejecuta
    }

La cual se comporta como un while normal, dependiendo del número de ciclo que está realizando:

Si el número de ciclo es impar (ODD): 1er ciclo, 3er ciclo, ...
    WHILE(ExprBoole1) {
        Sentencia1;
        Sentencia3;
    }

Si el número de ciclo es par (EVEN): 2do ciclo, 4to ciclo, ...
    WHILE(ExprBoole2) {
        Sentencia2;
        Sentencia3;
    }

Recuerde que ExprBoole1, ExprBoole2, Sentencia1, Sentencia2 y Sentencia3, generan su propio C3.
```
E_ODD:
  C3-ExprBoole1(tk)
  if (tk=0) GOTO Ef
  c3-sentencia1
  ce-sentenci3
  GOTO E_EVEN

E_EVEN:
  C3-ExprBoole2(tk)
  if (tk=0) GOTO Ef
  c3-sentencia2
  c3-sentencia3
  GOTO E_ODD

Ef:
```
**2.** *(Diagrama de Transiciones)* Un lenguaje toma como tokens a toda subsecuencia formada con **solo Letras**. Estos son:

- **IDA** → TERMINA con una subsecuencia **PAR** de A'es consecutivas.
- **ID** → Secuencia de Letras que no es un IDA.

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens..

```
paso 0
| Clase      | Descripción                      |
|------------|----------------------------------|
letra
'A'
otro  eof o separador

letra*AA    IDA
letra*A*A   IDA
letra*A     ID 
letra*      ID

### PASO 4 — Tabla de transición
| Estado | letra | 'A' | otro  |
|--------|-------|-----|-------|
| q0     | q1    | q2  | ERROR |
| q1     | q1    | q2  | ID✓   |
| q2     | q1    | q3  | ID✓   |
| q3     | q1    | q2  | IDA✓  |

```

**3.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra TODA la Cinta y en su único estado final, muestre en consola (print):

- La cantidad de palabras que están **dentro** de los comentarios multilínea de DELPHI { ... }, y
- La cantidad de palabras que están **fuera** de estos comentarios.

Se considera **palabra** a toda subsecuencia que no tiene Espacios ni EOF. Es decir, el Espacio y el EOF son los separadores de las palabras.
```
### PASO 0 — Variables
int cpd = 0
int cpf = 0

### PASO 1 — Clases
| Clase   | Descripción        |
|---------|--------------------|
| '{'     | barra              |
| '}'     | asterisco          |
 espacio
| EOF     | fin de cinta       |
| otro    | todo lo demás      |

### PASO 2 — Estados
| Estado | Significado                                      |
|--------|--------------------------------------------------|
| q0     | fuera del comentario, fuera de palabra           |
| q1     | fuera del comentario, dentro de palabra          |
| q2     | dentro del comentario, fuera de palabra          |
| q3     | dentro del comentario, dentro de palabra         |
| q4     | final → print cpd, print cpf                     |

### PASO 4 — Tabla de transición
| Estado | '{'  | '}'  | espacio | otro              | EOF                        |
|--------|------|------|---------|-------------------|----------------------------|
| q0     | q2   | q0   | q0      | q1 / cpf=cpf+1    | q4 print cpd, print cpf    |
| q1     | q2   | q1   | q0      | q1                | q4 print cpd, print cpf    |
| q2     | q2   | q0   | q2      | q3 / cpd=cpd+1    | q4 print cpd, print cpf    |
| q3     | q2   | q0   | q2      | q3                | q4 print cpd, print cpf    |
```
---

# PRIMER PARCIAL (R)
## INF329 SA COMPILADORES. GESTIÓN 1-2023
### Lun 26 de junio de 2023

**1.** *(Código-3)* Escriba un Esquema de Traducción para la construcción ficticia:

    if NUM1 (ExprBoole){
        Sentencia1;
    }
    else NUM2 {
        Sentencia2;
    }

//Por comodidad, asuma NUM1>0 y NUM2>0

Si la ExprBoole es true, la Sentencia1 se ejecutará **NUM1** veces; si es false, se ejecuta la Sentencia2 **NUM2** veces.

Tome en cuenta que ExprBoole, Sentencia1 y Sentencia2 generan su propio código-3.
```
 t4 = NUM1
 T5 = NUM2

Ei:
 t1 = 0
 t6 = 1
 C3-ExprBoole(tk)
 if (tk=0) GOTO E2
 GOTO E1

E2:
 t3 = (t5 > t1)
 if (t3=0) GOTO Ef
 c3-sentencia2
 t5 = t5 - t6
 GOTO E2

E1:
 t2 = (t4 > t1)
 if (t2=0) GOTO Ef
 c3-sentencia1
 t4 = t4 - t6
 GOTO E1

Ef:

```
**2.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt **sin acciones semánticas**, que devuelva:

- **true** si todas las palabras de la cinta tienen longitud par.
- **false** si alguna palabra de la cinta tiene longitud impar.

Se considera palabra a todo substring que no tiene espacios ni EOF.
```
### PASO 1 — Clases
| Clase   | Descripción                    |
|---------|--------------------------------|
| espacio | separador de palabras          |
| EOF     | fin de cinta                   |
| otro    | todo lo demás                  |

### PASO 2 — Estados
| Estado | Significado                                        |
|--------|----------------------------------------------------|
| q0     | fuera de palabra, todo bien (ninguna impar aún)    |
| q1     | dentro de palabra, longitud impar hasta ahora      |
| q2     | dentro de palabra, longitud par hasta ahora        |
| q3     | fuera de palabra, ya vi una palabra impar          |
| q4     | dentro de palabra, longitud impar (después de q3)  |
| qf     | estado final                                       |

### PASO 4 — Tabla
| Estado | otro | espacio | EOF             |
|--------|------|---------|-----------------|
| q0     | q1   | q0      | qf return true  |
| q1     | q2   | q3      | qf return false |
| q2     | q1   | q0      | qf return true  |
| q3     | q4   | q3      | qf return false |
| q4     | q3   | q0      | qf return false |

### PASO 3 — Ejemplos
"AB CD EOF"   → 2 palabras de longitud 2 → true
"AB C EOF"    → C tiene longitud 1 (impar) → false
"EOF"         → no hay palabras → true
"ABC EOF"     → longitud 3 (impar) → false
```


**3.** *(Diagrama de Transiciones)* Un lenguaje forma sus tokens con **solamente** Letras y Dígitos. Estos son:

- **NUMX** → Números enteros que alternan estrictamente dígitos pares con dígitos impares (debe tener al menos dos dígitos)
- **NUM** → Números enteros que no son NUMX.
- **IDNUMA** → Números enteros que terminan en 'A'
- **ID** → Combinación de letras y dígitos que no es NUMX o NUM o IDNUMA.

Dibuje un dt, sin acciones semánticas, para reconocer a (los nombres de) estos tokens.

```
### PASO 1 — Clases
| Clase    | Descripción                    |
|----------|--------------------------------|
| digitoPar  | 0,2,4,6,8                    |
| digitoImpar| 1,3,5,7,9                    |
| 'A'      | letra A especial               |
| letra    | B-Z (todo excepto A)           |
| otro     | EOF o separador                |

### PASO 2 — Estados
| Estado | Significado                                           |
|--------|-------------------------------------------------------|
| q0     | no sé nada                                            |
| q1     | llevo solo dígitos, último fue PAR (puede ser NUMX)   |
| q2     | llevo solo dígitos, último fue IMPAR (puede ser NUMX) |
| q3     | rompí alternación → es NUM sí o sí                   |
| q4     | llevo letras/dígitos → es ID sí o sí                 |
| q5     | último char fue 'A' → puede ser IDNUMA                |

### PASO 3 — Ejemplos
"21"    → par,impar → NUMX ✓
"23"    → par,impar → NUMX ✓
"42"    → par,par   → NUM (rompió alternación) ✓
"213"   → par,impar,par → NUMX ✓
"2"     → solo un dígito → NUM ✓
"hA"    → IDNUMA ✓
"h2"    → ID ✓
"246"   → NUM ✓
"12"    → impar,par → NUMX ✓

### PASO 4 — Tabla
| Estado | digitoPar | digitoImpar | 'A' | letra | otro    |
|--------|-----------|-------------|-----|-------|---------|
| q0     | q1        | q2          | q5  | q4    | ERROR   |
| q1     | q3        | q2          | q5  | q4    | NUM✓    |
| q2     | q1        | q3          | q5  | q4    | NUM✓    |
| q3     | q3        | q3          | q5  | q4    | NUM✓    |
| q4     | q4        | q4          | q5  | q4    | ID✓     |
| q5     | q4        | q4          | q5  | q4    | IDNUMA✓ |
```
# PRIMER PARCIAL (R2)
## INF329 SA–COMPILADORES. GESTIÓN 2-2023
### Vie 29 de diciembre de 2023

**1.** *(Código-3)* Escriba un esquema de traducción para la construcción:

    if(ExprBoole){
        Sentencia1;
    }
    else twice {
        Sentencia2;
    }

Si la ExprBoole es true, la Sentencia1 se ejecutará **una vez**; si es false, se ejecuta la Sentencia2 **dos veces (twice)**.

Tome en cuenta que ExprBoole, Sentencia1 y Sentencia2 generan su propio código-3.

**2.** *(Aplicación de los dt)* En una variable global int X, se almacena un número mayor que cero.

Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra las celdas necesarias y devuelva en su único estado final, la X-ésima palabra que hay en la cinta. Si tal palabra no existe, devolver la cadena vacía.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.
```
### PASO 0 — Variables
int cp = 0
string actual = ""

### PASO 1 — Clases
| Clase   | Descripción              |
|---------|--------------------------|
| espacio | separador de palabras    |
| EOF     | fin de cinta             |
| otro    | todo lo demás            |

### PASO 2 — Estados
| Estado | Significado                        |
|--------|------------------------------------|
| q0     | fuera de palabra, buscando X-ésima |
| q1     | dentro de palabra                  |
| qf     | estado final                       |

### PASO 4 — Tabla
| Estado | otro                      | espacio              | EOF              |
|--------|---------------------------|----------------------|------------------|
| q0     | q1 / actual=actual+c      | q0                   | qf return ""     |
| q1     | q1 / actual=actual+c      | q0 / cp++ if(cp=X) qf return actual; actual="" | qf / cp++ if(cp=X) return actual; return "" |
```
**3.** *(Diagrama de Transiciones)* Un lenguaje usa Tokens que son formados con **solamente** Dígitos y Letras. Estos son:

- **NUMP** → Números enteros pares
- **NUMI** → Números enteros impares
- **IDV** → Combinación de Letras y/o Dígitos, **terminada en vocal**
- **ID** → Ninguno de los anteriores. (Combinación de Letras y Dígitos)

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.
```
### PASO 1 — Clases
| Clase  | Descripción              |
|--------|--------------------------|
| parDig | 0,2,4,6,8                |
| imDig  | 1,3,5,7,9                |
| vocal  | A,E,I,O,U                |
| letra  | consonantes              |
| otro   | EOF o separador          |

### PASO 2 — Estados
| Estado | Significado                          |
|--------|--------------------------------------|
| q0     | no sé nada                           |
| q1     | llevo solo dígitos, último PAR        |
| q2     | llevo solo dígitos, último IMPAR      |
| q3     | llevo letras/dígitos, último NO vocal |
| q4     | último char fue vocal                 |

### PASO 4 — Tabla
| Estado | parDig | imDig | vocal | letra | otro   |
|--------|--------|-------|-------|-------|--------|
| q0     | q1     | q2    | q4    | q3    | ERROR  |
| q1     | q1     | q2    | q4    | q3    | NUMP✓  |
| q2     | q1     | q2    | q4    | q3    | NUMI✓  |
| q3     | q3     | q3    | q4    | q3    | ID✓    |
| q4     | q3     | q3    | q4    | q3    | IDV✓   |
```
---

# PRIMER PARCIAL
## INF329 SA–COMPILADORES. GESTIÓN 1-2024

**1.** *(Esquema de traducción)* Escriba un esquema de traducción C3 para la construcción ficticia:

    whileOdd (ExprBoole){
        Sentencia;
    }

Este bucle se comporta como un while normal, es decir, el bucle finaliza cuando su ExprBoole es false.

Pero, si el número de **pasada es par** (2da, 4ta, 6ta, ...), la **Sentencia NO es ejecutada**.

Obviamente, cuando el número de pasada es impar, la Sentencia es ejecutada.

Recuerde que ExprBoole, y Sentencia generan su propio C3.

**2.** *(Diagrama de Transiciones)* Un lenguaje usa Tokens que son formados con **solamente** Dígitos y Letras. Estos son:

- **NUM** → números enteros
- **YZ** → (su único lexema es "yz").
- **IDNUM** → Tienen al menos una Letra y al menos un Dígito
- **ID** → Solamente letras (menos el lexema "yz").

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.
```
### PASO 1 — Clases
| Clase  | Descripción              |
|--------|--------------------------|
| 'y'    | letra y especial         |
| 'z'    | letra z especial         |
| letra  | letras excepto y,z       |
| dígito | 0-9                      |
| otro   | EOF o separador          |

### PASO 2 — Estados
| Estado | Significado                          |
|--------|--------------------------------------|
| q0     | no sé nada                           |
| q1     | vi 'y'                               |
| q2     | llevo solo letras (no y sola)        |
| q3     | llevo dígitos                        |
| q4     | llevo letras y dígitos → IDNUM       |

### PASO 3 — Ejemplos
"yz"    → YZ ✓
"yza"   → ID (empieza con yz pero sigue)
"abc"   → ID ✓
"123"   → NUM ✓
"a1"    → IDNUM ✓
"y1"    → IDNUM ✓
"y"     → ID ✓

### PASO 4 — Tabla
| Estado | 'y' | 'z' | letra | dígito | otro   |
|--------|-----|-----|-------|--------|--------|
| q0     | q1  | q2  | q2    | q3     | ERROR  |
| q1     | q2  | q5  | q2    | q4     | ID✓    |
| q2     | q2  | q2  | q2    | q4     | ID✓    |
| q3     | q4  | q4  | q4    | q3     | NUM✓   |
| q4     | q4  | q4  | q4    | q4     | IDNUM✓ |
| q5     | q2  | q2  | q2    | q4     | YZ✓    |
```
**3.** *(Eliminación de Comentarios)* Dibuje un dt que reconozca el token DIV (lexema "/") y elimine el comentario multilinea que empieza con "/*". El comentario multilinea puede ser finalizado con "*/" o "*)" o "//".
```
### PASO 1 — Clases
| Clase  | Descripción          |
|--------|----------------------|
| '/'    | barra                |
| '*'    | asterisco            |
| ')'    | paréntesis cierre    |
| otro   | todo lo demás        |
| otro   | EOF o separador      |

### PASO 2 — Estados
| Estado | Significado                                      |
|--------|--------------------------------------------------|
| q0     | no sé nada                                       |
| q1     | vi '/' → puede ser DIV o inicio comentario       |
| q2     | dentro del comentario                            |
| q3     | dentro del comentario, vi '*'                    |
| q4     | dentro del comentario, vi '/' (posible //)       |

### PASO 3 — Ejemplos
"/"      → DIV ✓
"/* */"  → elimina comentario, nada
"/* *)"  → elimina comentario, nada
"/* //"  → elimina comentario, nada
"//"     → elimina comentario de línea... espera, ¿// dentro del comentario cierra?

### PASO 4 — Tabla
| Estado | '/'  | '*'  | ')'  | otro  |
|--------|------|------|------|-------|
| q0     | q1   | q0   | q0   | q0    |
| q1     | q4   | q2   | q0   | DIV✓  |
| q2     | q3   | q2   | q2   | q2    |
| q3     | q0   | q3   | q0   | q2    |
| q4     | q4   | q2   | q2   | q0    |
```
---

# PRIMER PARCIAL
## INF329 SA COMPILADORES. GESTIÓN 1-2025
### Jue 30 de mayo de 2025

**1.** Convierta el siguiente fragmento JAVA a Código-3

    while (true){
        read(Dato);
        if (Dato<0 || Dato>100)
            break;
        S = S + Dato;
    }
    print(S);

Puesto que en este fragmento de código hay un loop-forever, usted deberá hacer lo mismo en su código-3 equivalente. También haga uso, con total libertad, de las variables enteras Dato y S.

**2.** *(Diagrama de Transiciones)* Un lenguaje usa Tokens que son formados con **solamente** Dígitos y Letras. Estos son:

- **NUM** → números enteros
- **IDUM** → Intercalación estricta de Dígitos y Letras (al menos 2 chars).
- **ID** → (ninguno de los anteriores).

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.

Note que todo substring compuesto por Letras y/o Dígitos forma un token.
```
### PASO 1 — Clases
| Clase  | Descripción     |
|--------|-----------------|
| letra  | A-Z, a-z        |
| dígito | 0-9             |
| otro   | EOF o separador |

### PASO 2 — Estados
| Estado | Significado                                       |
|--------|---------------------------------------------------|
| q0     | no sé nada                                        |
| q1     | llevo solo dígitos → puede ser NUM                |
| q2     | llevo solo letras → puede ser ID                  |
| q3     | alternando: último fue letra, empecé con dígito   |
| q4     | alternando: último fue dígito, empecé con letra   |
| q5     | rompí alternación → ID sí o sí                    |

### PASO 3 — Ejemplos
"123"   → NUM ✓
"abc"   → ID ✓
"1a2b"  → IDUM ✓
"a1b2"  → IDUM ✓
"1a2"   → IDUM ✓
"11a"   → ID ✓
"a"     → ID ✓

### PASO 4 — Tabla
| Estado | letra | dígito | otro   |
|--------|-------|--------|--------|
| q0     | q2    | q1     | ERROR  |
| q1     | q3    | q1     | NUM✓   |
| q2     | q2    | q4     | ID✓    |
| q3     | q5    | q3     | IDUM✓  |
| q4     | q4    | q5     | IDUM✓  |
| q5     | q5    | q5     | ID✓    |
```
**3.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra **todas** las celdas, y devuelva la cantidad de palabras dentro de paréntesis que hay en la Cinta.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF. Por comodidad asuma que no se están usando paréntesis anidados.
```
### PASO 0 — Variables
int cp = 0

### PASO 1 — Clases
| Clase   | Descripción           |
|---------|-----------------------|
| '('     | abre paréntesis       |
| ')'     | cierra paréntesis     |
| espacio | separador             |
| EOF     | fin de cinta          |
| otro    | todo lo demás         |

### PASO 2 — Estados
| Estado | Significado                                    |
|--------|------------------------------------------------|
| q0     | fuera del paréntesis                           |
| q1     | dentro del paréntesis, fuera de palabra        |
| q2     | dentro del paréntesis, dentro de palabra       |
| qf     | estado final                                   |

### PASO 3 — Ejemplos
"(hola mundo) EOF"        → 2 ✓
"() EOF"                  → 0 ✓
"hola (mundo) EOF"        → 1 ✓
"(hola) (mundo) EOF"      → 2 ✓

### PASO 4 — Tabla
| Estado | '('  | ')'  | espacio | otro             | EOF          |
|--------|------|------|---------|------------------|--------------|
| q0     | q1   | q0   | q0      | q0               | qf return cp |
| q1     | q1   | q0   | q1      | q2 / cp=cp+1     | qf return cp |
| q2     | q1   | q0   | q1      | q2               | qf return cp |
```
---

# PRIMER PARCIAL (R)
## INF329 SA COMPILADORES. GESTIÓN 1-2025
### Sáb 14 de junio de 2025

**1.** *(Código-3)* Escriba un Esquema de Traducción para la construcción ficticia:

    while NUM (ExprBoole){
        Sentencia;
    }

//Por comodidad, asuma NUM>0

Este es casi un while normal. Este ciclo se ejecuta hasta que la ExprBoole es false o ya se hayan ejecutado NUM ciclos.

Tome en cuenta que ExprBoole y Sentencia generan su propio C3.

**2.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra todas las celdas y devuelva la cantidad de palabras formadas con solo letras que empiezan con 'A' y terminan en 'Z'.
```
### PASO 0 — Variables
int cp = 0

### PASO 1 — Clases
| Clase   | Descripción           |
|---------|-----------------------|
| 'A'     | letra A especial      |
| 'Z'     | letra Z especial      |
| letra   | letras excepto A y Z  |
| espacio | separador             |
| EOF     | fin de cinta          |
| otro    | todo lo demás (dígitos, símbolos) |

### PASO 2 — Estados
| Estado | Significado                                         |
|--------|-----------------------------------------------------|
| q0     | fuera de palabra                                    |
| q1     | dentro de palabra que empieza con 'A'               |
| q2     | dentro de palabra que NO empieza con 'A'            |
| q3     | dentro de palabra que empieza con 'A', último fue 'Z'|
| qf     | estado final                                        |

### PASO 3 — Ejemplos
"AZ EOF"        → 1 ✓
"ABZ EOF"       → 1 ✓
"AZB EOF"       → 0 (termina en B no Z) 
"BZ EOF"        → 0 (no empieza en A)
"AZ BZ EOF"     → 1 ✓
"AZ ABZ EOF"    → 2 ✓

### PASO 4 — Tabla
| Estado | 'A' | 'Z'          | letra | espacio      | EOF          | otro |
|--------|-----|--------------|-------|--------------|--------------|------|
| q0     | q1  | q2           | q2    | q0           | qf return cp | q2   |
| q1     | q1  | q3           | q1    | q0           | qf return cp | q2   |
| q2     | q2  | q2           | q2    | q0           | qf return cp | q2   |
| q3     | q1  | q3           | q1    | q0 cp=cp+1   | qf cp=cp+1 return cp | q2 |
```
* Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**3.** *(Diagrama de Transiciones)* Un lenguaje forma sus tokens con **solamente** Letras y Dígitos. Estos son:

- **NUMX** → Números enteros que alternan estrictamente dígitos pares con dígitos impares (debe tener al menos dos dígitos)
- **NUM** → Números enteros que no son NUMX.
- **ID** → Combinación de letras y dígitos que no es NUMX ni NUM.

Dibuje un dt, sin acciones semánticas, para reconocer a (los nombres de) estos tokens.
```
### PASO 1 — Clases
| Clase      | Descripción     |
|------------|-----------------|
| digitoPar  | 0,2,4,6,8       |
| digitoImpar| 1,3,5,7,9       |
| letra      | A-Z             |
| otro       | EOF o separador |

### PASO 2 — Estados
| Estado | Significado                                        |
|--------|----------------------------------------------------|
| q0     | no sé nada                                         |
| q1     | llevo dígitos, último PAR → puede ser NUMX         |
| q2     | llevo dígitos, último IMPAR → puede ser NUMX       |
| q3     | rompí alternación → NUM sí o sí                   |
| q4     | llevo letras/dígitos → ID sí o sí                 |

### PASO 3 — Ejemplos
"21"    → impar,par → NUMX ✓
"12"    → par,impar → NUMX ✓
"22"    → par,par   → NUM ✓
"2"     → solo un dígito → NUM ✓
"a1"    → ID ✓
"212"   → NUMX ✓

### PASO 4 — Tabla
| Estado | digitoPar | digitoImpar | letra | otro   |
|--------|-----------|-------------|-------|--------|
| q0     | q1        | q2          | q4    | ERROR  |
| q1     | q3        | q2          | q4    | NUM✓   |
| q2     | q1        | q3          | q4    | NUM✓   |
| q3     | q3        | q3          | q4    | NUM✓   |
| q4     | q4        | q4          | q4    | ID✓    |
```