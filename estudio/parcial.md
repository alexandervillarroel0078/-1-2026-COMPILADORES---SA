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
| Clase      | Descripción                      |
|------------|----------------------------------|
   letra
   'A'            
   otro      EOF o separador


| Estado | letra | 'A'  | otro  |
|--------|-------|------|-------|
| q0     | q3    | q1   | error |
| q1     | q3    | q2   | ID    |
| q2     | q4    | q1   | AID   |
| q3     | q3    | q3   | ID    |
| q4     | q4    | q4   | AID   |
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

### PASO 4 — Tabla de transición
| Estado | '/'        | '*'        | espacio | otro       | EOF       |
|--------|------------|------------|---------|------------|-----------|
| q0     | q1         | q0         | q0      | q0         | return cp |
| q1     | q0         | q2         | q0      | q0         | return cp |
| q2     | q3, cp++   | q3, cp++   | q2      | q3, cp++   | return cp |
| q3     | q3         | q4         | q2      | q3         | return cp |
| q4     | q0         | q4         | q2      | q3, cp++   | return cp |

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
## Paso 0 — Clases de entrada

| Clase  | Descripción                  |
|--------|------------------------------|
| letra  | cualquier letra menos 'A'    |
| 'A'    | la letra A                   |
| otro   | EOF o separador              |

## Paso 2 — Estados y su significado

| Estado | Descripción                                                        |
|--------|-------------------------------------------------------------------|
| q0     | inicial                                                           |
| q1     | leyendo letras, último char NO es 'A' → candidato ID             |
| q2     | cantidad de A's consecutivas al final es impar → candidato ID    |
| q3     | cantidad de A's consecutivas al final es par → candidato IDA     |

## Paso 4 — Tabla de transición

| Estado | letra | 'A' | otro  |
|--------|-------|-----|-------|
| q0     | q1    | q2  | ERROR |
| q1     | q1    | q2  | ID    |
| q2     | q1    | q3  | ID    |
| q3     | q1    | q2  | IDA   |
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
| Clase   | Descripción             |
|---------|-------------------------|
| '{'     | abre comentario Delphi  |
| '}'     | cierra comentario Delphi|
| espacio | espacio                 |
| EOF     | fin de cinta            |
| otro    | todo lo demás           |

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
t1 = 0
t2 = NUM1
t3 = NUM2
C3-ExprBoole(tk)
IF (tk=0) GOTO E1
E2:
    t4 = (t2 > t1)
    IF (t4=0) GOTO Ef
    C3-Sentencia1
    DEC t2
    GOTO E2
E1:
    t4 = (t3 > t1)
    IF (t4=0) GOTO Ef
    C3-Sentencia2
    DEC t3
    GOTO E1
Ef:
```
**2.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt **sin acciones semánticas**, que devuelva:

- **true** si todas las palabras de la cinta tienen longitud par.
- **false** si alguna palabra de la cinta tiene longitud impar.

Se considera palabra a todo substring que no tiene espacios ni EOF.
```
### PASO 1 — Clases
| Clase   | Descripción           |
|---------|-----------------------|
| char    | todo lo demás         |
| espacio | separador de palabras |
| EOF     | fin de cinta          |
                         
### PASO 4 — Tabla
| Estado | char | espacio      | EOF          |
|--------|------|--------------|--------------|
| q0     | q1   | q0           | return true  |
| q1     | q2   | return false | return false |
| q2     | q1   | q0           | return true  |

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
| Clase       | Descripción          |
|-------------|----------------------|
| digitoPar   | 0,2,4,6,8            |
| digitoImpar | 1,3,5,7,9            |
| 'A'         | letra A especial     |
| letra       | B-Z (todo excepto A) |
| otro        | EOF o separador      |

### PASO 2 — Estados
| Estado | Significado                                         |
|--------|-----------------------------------------------------|
| q0     | inicial                                             |
| q1     | un solo dígito PAR                                  |
| q2     | un solo dígito IMPAR                                |
| q3     | 2+ dígitos alternando, último PAR → candidato NUMX  |
| q4     | 2+ dígitos alternando, último IMPAR → candidato NUMX|
| q5     | rompió alternación → NUM sí o sí                    |
| q6     | lleva letras/dígitos mezclados → ID sí o sí         |
| q7     | último char fue 'A' → candidato IDNUMA              |

### PASO 3 — Ejemplos
"21"  → q0→q2→q4, otro → NUMX ✓
"42"  → q0→q1→q5, otro → NUM  ✓
"213" → q0→q2→q4→q3, otro → NUMX ✓
"2"   → q0→q1, otro → NUM ✓
"hA"  → q0→q6→q7, otro → IDNUMA ✓
"12"  → q0→q2→q3, otro → NUMX ✓

### PASO 4 — Tabla
| Estado | digitoPar | digitoImpar | 'A' | letra | otro   |
|--------|-----------|-------------|-----|-------|--------|
| q0     | q1        | q2          | q7  | q6    | ERROR  |
| q1     | q5        | q3          | q7  | q6    | NUM    |
| q2     | q4        | q5          | q7  | q6    | NUM    |
| q3     | q5        | q4          | q7  | q6    | NUMX   |
| q4     | q3        | q5          | q7  | q6    | NUMX   |
| q5     | q5        | q5          | q7  | q6    | NUM    |
| q6     | q6        | q6          | q7  | q6    | ID     |
| q7     | q6        | q6          | q7  | q6    | IDNUMA |
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
```
i = 2
t1 = 0
C3-ExprBoole(tk)
IF (tk=0) GOTO E1
C3-Sentencia1
GOTO Ef
E1:
    t2 = (i = t1)
    IF (t2=1) GOTO Ef
    C3-Sentencia2
    DEC i
    GOTO E1
Ef:
```
**2.** *(Aplicación de los dt)* En una variable global int X, se almacena un número mayor que cero.

Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra las celdas necesarias y devuelva en su único estado final, la X-ésima palabra que hay en la cinta. Si tal palabra no existe, devolver la cadena vacía.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.
```
### PASO 0 — Variables
int cp = 0
string actual = ""

### PASO 1 — Clases
| Clase   | Descripción           |
|---------|-----------------------|
| espacio | separador de palabras |
| EOF     | fin de cinta          |
| otro    | todo lo demás         |

### PASO 2 — Estados
| Estado | Significado                        |
|--------|------------------------------------|
| q0     | fuera de palabra, buscando X-ésima |
| q1     | dentro de palabra                  |
| qf     | estado final                       |

### PASO 3 — Ejemplos
X=2, "hola mundo EOF"  → 1ra: "hola", 2da: "mundo" → return "mundo"
X=1, "hola EOF"        → 1ra: "hola" → return "hola"
X=3, "ab cd EOF"       → solo 2 palabras → return ""
X=1, "EOF"             → no hay palabras → return ""

### PASO 4 — Tabla
| Estado | otro             | espacio                                        | EOF                                      |
|--------|------------------|------------------------------------------------|------------------------------------------|
| q0     | q1 / actual+=c   | q0                                             | qf / return ""                           |
| q1     | q1 / actual+=c   | cp++ / if(cp=X) qf return actual; actual="" q0 | cp++ / if(cp=X) qf return actual; return "" |

```
**3.** *(Diagrama de Transiciones)* Un lenguaje usa Tokens que son formados con **solamente** Dígitos y Letras. Estos son:

- **NUMP** → Números enteros pares
- **NUMI** → Números enteros impares
- **IDV** → Combinación de Letras y/o Dígitos, **terminada en vocal**
- **ID** → Ninguno de los anteriores. (Combinación de Letras y Dígitos)

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.
```
### PASO 1 — Clases
| Clase | Descripción     |
|-------|-----------------|
| parDig   | 0,2,4,6,8    |
| imDig    | 1,3,5,7,9    |
| vocal    | A,E,I,O,U    |
| consona  | consonantes  |
| otro     | EOF o separador |

### PASO 2 — Estados
| Estado | Significado                                    |
|--------|------------------------------------------------|
| q0     | inicial                                        |
| q1     | solo dígitos, último PAR → candidato NUMP      |
| q2     | solo dígitos, último IMPAR → candidato NUMI    |
| q3     | letras/dígitos mezclados, último NO vocal → ID |
| q4     | letras/dígitos mezclados, último vocal → IDV   |

### PASO 3 — Ejemplos
"24"    → q0→q1→q1, otro → NUMP ✓
"13"    → q0→q2→q2, otro → NUMI ✓
"hola"  → q0→q3→q4→q3→q4, otro → IDV ✓
"test"  → q0→q3→q4→q3→q3, otro → ID ✓
"2A"    → q0→q1→q4, otro → IDV ✓
"2b"    → q0→q1→q3, otro → ID ✓

### PASO 4 — Tabla
| Estado | parDig | imDig | vocal | consona | otro      |
|--------|--------|-------|-------|---------|-----------|
| q0     | q1     | q2    | q4    | q3      | ERROR     |
| q1     | q1     | q2    | q4    | q3      | NUMP      |
| q2     | q1     | q2    | q4    | q3      | NUMI      |
| q3     | q3     | q3    | q4    | q3      | ID        |
| q4     | q3     | q3    | q4    | q3      | IDV       |

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
```
 t1 = 1 
 t2 = 2
 t3 = 0
Ei:
 c3-exprBoole(tk)
 if (tk=0) goto Ef

 t4 = (t1 MOD t2)
 t5 = (t4 = t3)
 if (t5=1) goto E2
 c3-sentencia1
 inc t1
 goto Ei

E2:
 inc t1
 goto Ei

Ef:
```
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

### PASO 3 — Ejemplos
"yz"    → YZ ✓
"yza"   → ID (empieza con yz pero sigue)
"abc"   → ID ✓
"123"   → NUM ✓
"a1"    → IDNUM ✓
"y1"    → IDNUM ✓
"y"     → ID ✓


| Estado | dígito | 'y' | 'z' | letra | otro/EOF (retract) |
|--------|--------|-----|-----|------------|-------------------|
| q0     | q1     | q2  | q4  | q4         | error             |
| q1     | q1     | q5  | q5  | q5         | return NUM        |
| q2     | q5     | q4  | q3  | q4         | return ID         |
| q3     | q5     | q4  | q4  | q4         | return YZ         |
| q4     | q5     | q4  | q4  | q4         | return ID         |
| q5     | q5     | q5  | q5  | q5         | return IDNUM      |

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
## INF329 SA–COMPILADORES. GESTIÓN 2-2024
### Vie 01 de noviembre de 2024

**1.** *(Esquema de traducción)* Escriba un esquema de traducción C3 para la construcción ficticia:

    repeat NUM{
        Sentencia;
    }until(ExprBoole)

Este bucle repite la Sentencia, NUM veces. Pero en cada pasada, luego de ejecutar la Sentencia, consulta el valor de la ExprBoole: Si es true, el bucle finaliza; si es false, continúa.

En otras palabras, este bucle finaliza luego de ejecutar NUM veces la Sentencia o cuando la ExprBoole es true.

Recuerde que ExprBoole, y Sentencia generan su propio C3.
```
Ei:
 c3-sentencia
 dec NUM
 c3-exprBoole(tk)
 if (tk=1) goto Ef

 t1 = 0
 t2 = (NUM = t1)
 if (t2=1) goto Ef
 goto Ei

Ef:
```
**2.** *(Diagrama de Transiciones)* Un lenguaje forma sus tokens con solamente Dígitos y Letras. Estos son:

- **PNUM** → números enteros que tienen una cantidad par de Dígitos.
- **INUM** → números enteros que tienen una cantidad impar de Dígitos.
- **ID** → combinación de letras y dígitos (debe tener al menos una letra).

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.
```
| Estado | dígito | letra | otro/EOF (retract) |
|--------|--------|-------|--------------------|
| q0     | q1     | q3    | error              |
| q1     | q2     | q3    | return INUM        |
| q2     | q1     | q3    | return PNUM        |
| q3     | q3     | q3    | return ID          |

```
**3.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra toda la Cinta y devuelva en su estado final, el NUM de mayor valor. Si no existen NUM's en la Cinta, devolver –1.

Como se sabe, un NUM es una palabra formada con solamente Dígitos. Recuerde que las palabras en la Cinta, se separan con Espacios o con el EOF.

Para convertir un String a entero, use la función toInt. Por ejemplo,

    int x=toInt(ac); //ac es un String.

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
```
 t1 = 0
 t2 = 100
Ei:
 read(dato)
 t3 = (dato < t1)
 t4 = (dato > t2)
 t5 = (t3 or t4)
 if (t5=1) goto Ef
 s = s + dato
 goto Ei

Ef
 write(S)
```
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
| Estado | dígito | letra | otro/EOF (retract) |
|--------|--------|-------|--------------------|
| q0     | q1     | q2    | error              |
| q1     | q1     | q4    | return NUM         |
| q2     | q3     | q6    | return ID          |
| q3     | q5     | q2    | return IDUM        |
| q4     | q1     | q3    | return IDUM        |
| q5     | q5     | q5    | return ID          |
| q6     | q3     | q6    | return ID          |
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

# PRIMER PARCIAL
## INF329 SA–COMPILADORES. GESTIÓN 2-2024
### Vie 01 de noviembre de 2024

**1.** *(Esquema de traducción)* Escriba un esquema de traducción C3 para la construcción ficticia:

    repeat NUM{
        Sentencia;
    }until(ExprBoole)

Este bucle repite la Sentencia, NUM veces. Pero en cada pasada, luego de ejecutar la Sentencia, consulta el valor de la ExprBoole: Si es true, el bucle finaliza; si es false, continúa.

En otras palabras, este bucle finaliza luego de ejecutar NUM veces la Sentencia o cuando la ExprBoole es true.

Recuerde que ExprBoole, y Sentencia generan su propio C3.
```
Ei:
 c3-sentencia
 dec NUM
 c3-exprBoole(tk)
 if (tk=1) goto Ef

 t1 = 0
 t2 = (NUM = t1)
 if (t2=1) goto Ef
 goto Ei

Ef:
```
**2.** *(Diagrama de Transiciones)* Un lenguaje forma sus tokens con solamente Dígitos y Letras. Estos son:

- **PNUM** → números enteros que tienen una cantidad par de Dígitos.
- **INUM** → números enteros que tienen una cantidad impar de Dígitos.
- **ID** → combinación de letras y dígitos (debe tener al menos una letra).

Dibuje un dt, **sin acciones semánticas**, para reconocer a (los nombres de) estos Tokens.
```
| Estado | dígito | letra | otro/EOF (retract) |
|--------|--------|-------|--------------------|
| q0     | q1     | q3    | error              |
| q1     | q2     | q3    | return INUM        |
| q2     | q1     | q3    | return PNUM        |
| q3     | q3     | q3    | return ID          |
```
**3.** *(Aplicación de los dt)* Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra toda la Cinta y devuelva en su estado final, el NUM de mayor valor. Si no existen NUM's en la Cinta, devolver –1.

Como se sabe, un NUM es una palabra formada con solamente Dígitos. Recuerde que las palabras en la Cinta, se separan con Espacios o con el EOF.

Para convertir un String a entero, use la función toInt. Por ejemplo,

    int x=toInt(ac); //ac es un String.

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
```

```
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