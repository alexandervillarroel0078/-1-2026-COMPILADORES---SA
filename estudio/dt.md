# Los Tokens

**TOKEN** = Lenguaje regular que no tiene λ

**λ** = Cadena vacía

$$NUM = \{ \text{"0"}, \text{"1"}, \ldots, \text{"9"}, \text{"101"} \}$$

- **STRING** → (flecha apuntando al conjunto)
- **LEXEMAS** → (flecha apuntando a los elementos individuales)

---

## Definiciones regulares

**Dígito** → 0 | 1 | 2 | ... | 9

**NUM** → Dígito⁺
- 0
- 1
- 21

---

### Simbología

| Símbolo | Significado |
|---------|-------------|
| →       | Se lee "es" o "puede ser" |
| *       | 0 o más |
| +       | 1 o más |
| .       | Concatenación (no se escribe) |
| ( )     | Signos de agrupación |
| \|      | Se lee "o" |

**Letra** → a | b | c | ... | z

**(a)** Definir el token ID cuyos lexemas se forman así:
- Empiezan con A y terminan con Z

**ID** → A Letra* Z

Ejemplos:
- "AbcZ"
- "AZ"
- "AxyzZ"
- "AbcayaecZ"

**(b)** El token NUMPAR especifica números enteros pares sin signo.

Ejemplos: 0, 2, 4, 6, 8

**dígito** → 0 | 1 | 2 | ... | 9  
**dígitoPar** → 0 | 2 | 4 | 6 | 8

**NUMPAR** → dígito* dígitoPar

## Números reales

> dígito ≠ "3.0"  (dígito es solo 1 dígito, no "3.0")

**punto** → .  
**dígito** → 0 | 1 | 2 | ... | 9

**NUMREAL** → Dígito+ . Dígito+

Ejemplos:
- 7.0


## Diagrama de Transición (DT)

Las definiciones regulares **NO PUEDEN** tener la estrella de Kleene (`*`, `+`)

---

### Reglas para los DT

i) Solo tiene **un estado inicial** →①  
ii) Al menos tenga **un estado final** ②  
iii) Los estados finales no tienen aristas salientes

→(3)──────→  ✗ mal

iv) Todos los estados tienen números diferentes

        ──→(7)
→(0)─┤
        ──→(10)──→((20))    (20) es estado final

v) Siempre que tengamos un estado con arista saliente,
   tiene que tener una arista rotulada con "otro"

          ┌─ letra ─→( )
    →(3)──┤
          └─ otro ──→( )

vi) ambiguedad no puede tener









