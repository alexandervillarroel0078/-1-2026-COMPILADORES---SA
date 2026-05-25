# PRÁCTICO 1 - INF329 SA–COMPILADORES. GESTIÓN 1–2026

---

## CÓDIGO-3

### 1. (Teoría)
¿Qué es un error Léxico? Ejemplifique

**R:**

---

### 2. (Teoría)
En el siguiente fragmento de código JAVA, se genera un error de "Type mismatch" (tipos no coinciden o tipos incompatibles)
```java
String s = "1";
int x = 1 + s; //Type mismatch
```
¿Qué fase del compilador fue el que arrojó este error?

**R:**

---

### 3. (Esquema de traducción)
Escriba un esquema de traducción para la siguiente construcción ficticia:
```
DO
    Sentencia;
UNTIL ExprBoole1 TOGGLE ExprBoole2;
```
Esta construcción ejecuta la Sentencia en un ciclo. Para salir de este ciclo, al llegar al UNTIL:
La primera vez consulta por la ExprBoole1, si es true se sale del ciclo; la segunda vez consulta por la ExprBoole2, si es false se sale del ciclo; la tercera vez consulta por la ExprBoole1, si es true se sale del ciclo; la cuarta vez consulta por la ExprBoole2, si es false sale del ciclo; …

Note que en cada ciclo se consulta por la ExprBoole1 (true para salir) y en el siguiente ciclo por la ExprBoole2 (false para salir), pero jamás se consulta por ambas simultáneamente.

Recuerde que Sentencia, ExprBoole1 y ExprBoole2 generan su propio C3.

**R:**

---

### 4. (Esquema de traducción)
Escriba un esquema de traducción C3 para la construcción ficticia:
```
until (ExprBoole){
    Sentencia;
}norZero (ID);
// ID = una variable (por ejemplo: Z)
```
Esta construcción hace lo siguiente:
- a) Consulta el valor de verdad de ExprBoole. Si es falsa continúa en (b); si es verdadera todo finaliza.
- b) Ejecuta la Sentencia.
- c) Decrementa el valor del ID. Si ID es mayor que cero, continúa en (a); caso contrario todo finaliza.

Recuerde que cada ExprBoole y Sentencia generan su propio C3.

**R:**



```
Ei:
    C3-ExprBoole(tk)
    IF (tk=1) GOTO Ef
    C3-Sentencia
    DEC ID
    t1 = 0
    t2 = (ID > t1)
    IF (t2=1) GOTO Ei
Ef:
```


---

### 5. (Esquema de traducción)
Escriba un esquema de traducción C3 para la construcción ficticia:
```
whileCase (Expr != Expr3){
    case Expr1 : Sentencia1;
    case Expr2 : Sentencia2;
}
```
Esta construcción hace lo siguiente:
- a) Verifica que Expr == Expr3. Si son iguales, todo termina; caso contrario, continúa en (b).
- b) Verifica que Expr == Expr1. Si son iguales, ejecuta la Sentencia1 y luego continúa en (a); caso contrario, continúa en (c).
- c) Verifica que Expr == Expr2. Si son iguales, ejecuta la Sentencia2 y luego continúa en (a); caso contrario, continúa en (a).

Recuerde que Expr, Expr1, Expr2, Expr3, Sentencia1 y Sentencia2 generan su propio C3.

**R:**
```
Ei:
  C3-Expr(t0) 
  C3-Expr3(t3)
  t4 = (t0 = t3)
  if (t4=1) => GOTO Ef

  C3-Expr1(t1) 
  t5 = (t0=t1)
  if (t5=0) => GOTO E1
  C3-Sentencia1
  GOTO Ei

E1: 
  C3-Expr2(t2)  
  t6 = (t0 = t2)
  if (t6=0) => GOTO Ei
  C3-Sentencia2
  GOTO Ei
Ef:
```
---

### 6. (Esquema de traducción)
Escriba un esquema de traducción C3 para la construcción ficticia:
```
forX ID=NUM1 to NUM2 STEP NUM3{
    par
        Sentencia1;
    impar
        Sentencia2
}until (ExprBoole);
// NUM1 y NUM2 = 2 números enteros (por ejemplo: 3 y 8)
// ID = una variable (por ejemplo: Z)
```
Esta construcción empieza asignando ID = NUM1. Luego entra a un ciclo, el cual hace lo siguiente:
- a) Si el ID es par ejecuta la Sentencia1; caso contrario, ejecuta la Sentencia2.
- b) Hace ID = ID + NUM3.
- c) Si ID > NUM2 O la ExprBoole es true, todo finaliza. Caso contrario, continúa en (a).

Recuerde que ExprBoole, Sentencia1 y Sentencia2 generan su propio C3.

**R:**
```
ID = NUM1
Ei:
 t1 = 2
 t2 = ID mod t1
 t3 = 0
 t4 = (t2 = t3)
 if (t4=1) GOTO E1
 C3-Sentencia2
 GOTO E2

E1:
 C3-Sentencia1

E2:
 ID = ID + NUM3

 t5 = (ID > NUM2)
 C3-ExprBoole(tk)
 t6 = (t5 or tk)
 if (t6=0) GOTO Ei

Ef:
```

---

### 7. (Código-3)
Escriba un esquema de traducción para la construcción:
```java
if NUM (ExprBoole){ //NUM es un entero > 0 (e.g. 4)
    Sentencia1;
}
else {
    Sentencia2;
}
```
Si la ExprBoole es true, Sentencia1 se ejecutará NUM veces; si es false, se ejecuta la Sentencia2 (una sola vez).

Tome en cuenta que ExprBoole, Sentencia1 y Sentencia2 generan su propio código-3.

**R:**
```
C3-ExprBoole(tk)
 if (tk=1) GOTO E1
 C3-Sentencia2
 GOTO Ef

E1:
 C3-Sentencia1
 DEC NUM
 t1 = 0
 t2 = (NUM > t1)
 if (t2=1) GOTO E1

Ef: 
```

---

### 8. (Código-3)
Convierta el siguiente fragmento JAVA a Código-3:
```java
while (true){
    read(Dato);
    if (Dato==0)
        break; //Salir del while
    if (Dato%2==0)
        S = S + Dato;
    else
        S = S – Dato;
}
print(S);
```

**R:**
```

Ei:
 READ(dato)
 t1=0
 t2=(dato = t1)
 if (t2=1) GOTO Ef

 t3=2
 t4= (dato MOD t3)
 t5= (t4 = t1)
 if (t5=0) GOTO E1

 S = S + dato
 GOTO Ei

E1:
 S = S - dato
 GOTO Ei

Ef:
 WRITE(S)
```


---

### 9. (Código-3)
Convierta el siguiente fragmento JAVA a Código-3:
```java
do{
    read(Dato);
    if (Dato != 100)
        S = S + Dato;
    else
        S = S – Dato;
}while (Dato != 0 && Dato !=100)
print(S);
```
Use con total libertad las variables enteras Dato y S.

**R:**
```
Ei:
 READ(dato)
 t1 = 100
 t2 = (dato != t1)
 if (t2=1) GOTO E1
 S = S - dato
 GOTO E2

E1:
 S = S + dato

E2: 
 t3 = 0
 
 t4 = (dato != t3)
 t5 = (dato != t1)

 t6 = (t4 and t5)

 if (t6=1) GOTO Ei

Ef:
WRITE(S)

```
---

### 10. (Código-3)
Convierta el siguiente fragmento JAVA a Código-3:
```java
i=0;
c=10;
while (true){ //Loop-forever
    if (i % 2 != 0){
        System.out.print("impar");
        c--;
    }
    else{
        System.out.print("par");
    }
    if (c == 0){
        break; //Salir del while
    }
    i++;
} //End while
System.out.print("Fin");
```
Use con total libertad las variables enteras i y c.

**R:**
```

 i = 0 
 c = 10
Ei:
 t1 = 0
 t2 = 2

 t3 = (i mod t2)
 t4 = (t3 != t1)

 if (t4=1) GOTO E1
 WRITES("par")
 GOTO E2
 
E1:
 WRITES("impar")
 DEC C

E2:
 t5 = (c = t1)
 if (t5=1) GOTO Ef
 INC i
 GOTO Ei

Ef
 WRITES("fin")
 
```
---


































## DIAGRAMA DE TRANSICIONES

---

### 1. (Diagrama de Transiciones)
Dibujar un dt que, trabajando con Letras y Dígitos, reconozca los siguientes tokens:
- NUM → Números enteros (solamente Dígitos).
- HEX → Núm. hexadecimales (deben tener al menos una letra hexa o pueden terminar en "h"). Por ejemplo: A12h, 123h, B1, …
- ID → Combinación de letras y dígitos que no son NUM o HEX

**R:**
```
### Clases de caracteres

| Clase      | Descripción                      |
|------------|----------------------------------|
| dígito     | 0-9                              |
| letraHex   | A\|B\|C\|D\|E\|F                 |
| 'h'        | la h especial que termina HEX    |
| letraNoHex | G\|I\|J\|...\|Z (todo excepto h) |
| otro       | EOF o separador                  |
 
### Tabla de transición
 
| Estado | dígito | letraHex | 'h' | letraNoHex | otro  |
|--------|--------|----------|-----|------------|-------|
| q0     | q1     | q2       | q4  | q4         | ERROR |
| q1     | q1     | q2       | q3  | q4         | NUM   |
| q2     | q2     | q2       | q3  | q4         | HEX   |
| q3     | q4     | q4       | q4  | q4         | HEX   |
| q4     | q4     | q4       | q4  | q4         | ID    |
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
 
### PASO 4 — Tabla de transición
 
| Estado | letra | dígito | otro    |
|--------|-------|--------|---------|
| q0     | q2    | q1     | ERROR   |
| q1     | q3    | q1     | NUM     |
| q2     | q2    | q4     | ID      |
| q3     | q3    | q5     | NUMLET  |
| q4     | q5    | q4     | LETNUM  |
| q5     | q5    | q5     | ID      |
```
---

### 3. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos:
- NUMP → Números enteros cuya cantidad de dígitos significativos es par
- NUMI → Números enteros cuya cantidad de dígitos significativos es impar

Por "dígito significativo" se refiere a que no se tomen en cuenta los ceros a la izquierda.

**R:**
```
### Clases de caracteres

| Clase  | Descripción     |
|--------|-----------------|
| dígito | 1,2,...,9       |
| cero   | 0               |
| otro   | EOF o separador |
 

### Tabla de transición
 
| Estado | cero | dígito | otro  |
|--------|------|--------|-------|
| q0     | q1   | q3     | ERROR |
| q1     | q1   | q3     | NUMI |
| q2     | q4   | q3     | NUMP  |
| q3     | q4   | q2     | NUMI  |
  q4       q3     q2       NUMP
```
---

### 4. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos y Letras. Estos son:
- NUMO → Números octales, pueden terminar con la vocal 'O'.
- NUM → Números enteros que no son NUMO. Opcionalmente pueden terminar en 'd'.
- ID → combinación de letras y/o dígitos que no son NUM ni NUMO (debe tener al menos una letra)

**R:**
```
### Clases de caracteres

| Clase     | Descripción              |
|-----------|--------------------------|
| octal     | 0-7                      |
| noOctal   | 8\|9                     |
| 'O'       | letra O especial         |
| 'd'       | letra d especial         |
| otraLetra | A-Z excepto O y d        |
| otro      | EOF o separador          |
 

### Tabla de transición
 
| Estado | octal | noOctal | 'O' | 'd' | otraLetra | otro  |
|--------|-------|---------|-----|-----|-----------|-------|
| q0     | q1    | q2      | q5  | q5  | q5        | ERROR |
| q1     | q1    | q2      | q3  | q4  | q5        | NUMO  |
| q2     | q2    | q2      | q5  | q4  | q5        | NUM   |
| q3     | q5    | q5      | q5  | q5  | q5        | NUMO  |
| q4     | q5    | q5      | q5  | q5  | q5        | NUM   |
| q5     | q5    | q5      | q5  | q5  | q5        | ID    |
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
### Clases de caracteres

| Clase  | Descripción     |
|--------|-----------------|
| letra  | A-Z, a-z        |
| dígito | 0-9             |
| otro   | EOF o separador |

### Tabla de transición

| Estado | letra | dígito | otro   |
|--------|-------|--------|--------|
| q0     | q2    | q1     | ERROR  |
| q1     | q4    | q1     | NUM    |
| q2     | q2    | q3     | ID     |
| q3     | q4    | q5     | IDNUM  |
| q4     | q5    | q3     | IDNUM  |
| q5     | q5    | q5     | ID     |
```
---

### 6. (Diagrama de Transiciones)
Un lenguaje toma como tokens a toda subsecuencia formada con solo Letras. Estos son:
- IDBA → Termina con la subsecuencia "BA".
- IDA → Es 'A' o, termina en 'A' pero el anterior char no es 'B'.
- ID → subsecuencia de Letras que no es IDBA ni IDA.

**R:**
```
### Clases de caracteres

| Clase     | Descripción               |
|-----------|---------------------------|
| 'A'       | letra A especial          |
| 'B'       | letra B especial          |
| otraLetra | C-Z (todo excepto A y B)  |
| otro      | EOF o separador           |
              |

### Tabla de transición

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
### Clases de caracteres

| Clase  | Descripción        |
|--------|--------------------|
| dígito | 0-9                |
| '.'    | punto especial     |
| 'F'    | letra F especial   |
| letra  | A-Z excepto F      |
| otro   | EOF o separador    |
 
### Tabla de transición

| Estado | dígito | '.'   | 'F'   | letra | otro    |
|--------|--------|-------|-------|-------|---------|
| q0     | q1     | q3    | q5    | q5    | ERROR   |
| q1     | q1     | q2    | q5    | q5    | NUM✓    |
| q2     | q4     | ERROR | q6    | q5    | DOUBLE✓ |
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
```
### PASO 0 — Variables
int cp = 0

### Clases de caracteres
| Clase   | Descripción         |
|---------|---------------------|
| espacio | espacio o separador |
| EOF     | fin de cinta        |
| otro    | cualquier otro char | 

### Tabla de transición
| Estado | espacio | otro    | EOF          |
|--------|---------|---------|--------------|
| q0     | q0      | q1 cp++ | q2 return cp |
| q1     | q0      | q1      | q2 return cp |
```
---

### 9. (Aplicación de los dt)
En este contexto, llamamos palabra a toda subsecuencia formada con solamente letras. Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra todas las celdas y devuelva la palabra de mayor longitud que está presente en la Cinta. Este dt toma como separador de las palabras a todo char que no sea Letra.

Si la Cinta no tiene ninguna palabra, su dt devolverá "" (cadena vacía).

Por comodidad, asuma que todas las palabras (si existen) tienen diferente longitud.

**R:**
```
### PASO 0 — Variables
string actual = ""
string mejor  = ""
int longActual = 0
int longMejor  = 0

### Clases de caracteres
| Clase | Descripción     |
|-------|-----------------|
| letra | A-Z, a-z        |
| otro  | todo lo demás   |
| EOF   | fin de cinta    | 
 
### Tabla de transición
| Estado | letra | otro | EOF |
|--------|-------|------|-----|
| q0     | q1    | q0   | q2  |
| q1     | q1    | q0   | q2  |

Acciones:
- q0 --[letra]--> q1 : actual = actual + c;  longActual = 1
- q1 --[letra]--> q1 : actual = actual + c;  longActual = longActual + 1
- q1 --[otro] --> q0 : if(longActual > longMejor){ mejor = actual; longMejor = longActual }
                       actual = "";  longActual = 0
- q0 --[EOF]  --> q2 : return mejor
- q1 --[EOF]  --> q2 : if(longActual > longMejor) mejor = actual;  return mejor
```
---

### 10. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, que recorra todas las celdas y devuelva (return en un estado final) la cantidad de palabras que empiezan con 'A'.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**
```
### PASO 0 — Variables
int cp = 0

### Clases de caracteres
| Clase   | Descripción         |
|---------|---------------------|
| 'A'     | letra A especial    |
| espacio | espacio             |
| EOF     | fin de cinta        |
| otro    | todo lo demás       |

### Tabla de transición
| Estado | 'A'              | otro | espacio | EOF          |
|--------|------------------|------|---------|--------------|
| q0     | q1 / cp = cp + 1 | q2   | q0      | q3 return cp |
| q1     | q1               | q1   | q0      | q3 return cp |
| q2     | q2               | q2   | q0      | q3 return cp |
```

---

### 11. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt que recorra todas las celdas y devuelva la cantidad de palabras que aparecen dentro de los comentarios de línea. Los comentarios que se usan son los usados por JAVA (//….)

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**
```
### PASO 0 — Variables
int cp = 0

### Clases de caracteres
| Clase   | Descripción         |
|---------|---------------------|
| '/'     | barra especial      |
| '\n'    | salto de línea      |
| espacio | espacio             |
| EOF     | fin de cinta        |
| otro    | todo lo demás       |

### Tabla de transición
| Estado | '/'  | '\n' | espacio | otro             | EOF          |
|--------|------|------|---------|------------------|--------------|
| q0     | q1   | q0   | q0      | q0               | return cp |
| q1     | q2   | q0   | q0      | q0               | return cp |
| q2     | q2   | q0   | q2      | q3 / cp = cp + 1 | return cp |
| q3     | q3   | q0   | q2      | q3               | return cp |
```

---

### 12. (Aplicación de los dt)
Asumiendo que el cabezal de la Cinta de Caracteres está en la primera celda, dibuje un dt, de no más de tres estados, que recorra todas las celdas y devuelva (return en un estado final), la cantidad de palabras que hay en la Cinta.

Se considera palabra a toda subsecuencia que no tiene Espacios ni EOF.

**R:**
```
### PASO 0 — Variables
int cp = 0

### PASO Clases de caracteres
| Clase   | Descripción         |
|---------|---------------------|
| espacio | espacio o separador |
| EOF     | fin de cinta        |
| otro    | cualquier otro char | 

### PASO Tabla de transición
| Estado | espacio | otro             | EOF          |
|--------|---------|------------------|--------------|
| q0     | q0      | q1 / cp = cp + 1 | q2 return cp |
| q1     | q0      | q1               | q2 return cp |

Nota: con solo 2 estados activos (q0 y q1) se resuelve todo.
El tercer estado q2 es únicamente el estado final.
La restricción de 3 estados no agrega dificultad porque la lógica es simple.
```