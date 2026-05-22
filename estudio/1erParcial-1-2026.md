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
 i=0
Ei:
 C3-ExprBoole(tk)
 if (tk=1) GOTO E1
 C3-Sentencia2
 GOTO Ef

E1:
 C3-Sentencia1
 INC i
 t2 = NUM
 t3 = (i=t2)
 if (T3=0) GOTO E1

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






### 3. (Diagrama de Transiciones)
Un lenguaje usa Tokens que son formados con solamente Dígitos:
- NUMP → Números enteros cuya cantidad de dígitos significativos es par
- NUMI → Números enteros cuya cantidad de dígitos significativos es impar

Dibuje un dt para reconocer a (los nombres de) estos Tokens.

Por "dígito significativo", en este caso, se refiere a que no se tomen en cuenta los ceros a la izquierda del NUM.

**R:**
### PASO 1 — Clases de caracteres
```
| Clase      | Descripción                        |
|------------|------------------------------------|
 digito       {1,2,...,n+1}        
 otro         EOF O separador
 cero         {0}
```
### PASO 2 — Estados (trabajos)
```
| Estado | Significado                  | Observación          |
|--------|------------------------------|----------------------|
q0 → "no sé nada todavía"
q1 → "ceros a la izquierda"
q2    digito par
q3 digito impar        **R:**
### PASO 1 — Clases de caracteres
```
| Clase      | Descripción                        |
|------------|------------------------------------|
 digito       {1,2,...,n+1}        
 otro         EOF O separador
 cero         {0}
```
### PASO 2 — Estados (trabajos)
```
| Estado | Significado                  | Observación          |
|--------|------------------------------|----------------------|
q0 → "no sé nada todavía"
q1 → "ceros a la izquierda"
q2    digito par
q3 digito impar            


### PASO 4 — Tabla de transición

Estado | cero | dígito | otro
q0     | q1   | q3     | ERROR
q1       q1    q3   digimpar
q2       






