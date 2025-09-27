# Combinational and Sequential optimisation


## combination logic optimisation
combinational optimization is about simplifying and improving combinational circuits for better area, speed, and power efficiency,which is critical for building efficient chips.

**why combinational logic optimisation?**


Squeeze and simplify logic to get the most optimized design in terms of area and power.

### Constant Propagation


**Constant propagation** is an optimization technique where known constant values (0 or 1) in the inputs or signals of a combinational logic circuit are substituted directly to simplify the circuit’s logic.


#### How it works

If an input signal is constant, we can replace it with that constant value in the logic expression, which often reduces the complexity of the circuit.


#### Example

Consider the logic expression:

$$
Y = \overline{(A \cdot B + C)}
$$

If the input $A = 0$, then:

$$
Y = \overline{(0 \cdot B + C)} = \overline{C} = C'
$$

This simplification happens because $A \cdot B = 0$ when $A = 0$, so the expression reduces to $C'$.

#### Benefits

* Simplifies logic expressions
* Reduces number of gates and transistors used
* Decreases power consumption and delay
* Helps improve overall circuit efficiency
---
## Boolean Logic Optimisation

**Boolean logic optimization** is the process of simplifying Boolean expressions or logic circuits to make them more efficient. The goal is to reduce the number of logic gates, minimize circuit complexity, improve speed, and lower power consumption without changing the output behavior.


### Why optimize Boolean logic?

* To **reduce hardware cost** by using fewer gates.
* To **improve circuit speed** by minimizing the number of logic levels.
* To **reduce power consumption**.
* To **simplify design and verification**.


### How is it done?

* Using algebraic simplifications (like Boolean identities).
* Applying Karnaugh maps (K-maps).
* Using algorithmic methods like the Quine-McCluskey algorithm.
* Using logic synthesis tools in CAD software.

### Example

Original expression:

$$
F = A \cdot B + A \cdot \overline{B}
$$

Using Boolean laws:

$$
F = A \cdot (B + \overline{B}) = A \cdot 1 = A
$$

The expression is simplified from two terms to just one.




