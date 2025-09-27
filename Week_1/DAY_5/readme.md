# Optimisation in synthesis

## Lab on imcomplete if case

~~~
module incomp_if (input i0 , input i1 , input i2 , output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
end
endmodule
~~~

#### RTL simulation waveform

 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-03-56.png)

 #### DOT view of incomp_if.v
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-11-18.png)


* **Conditional assignment**: If the input i0 is true (logic 1), the output y is assigned the value of input i1.

* **Incomplete behavior**: If i0 is false (logic 0), the output y retains its previous value (no assignment), which may cause unintended latch inference due to missing an else branch.


  ---
### Lab on incomp_if2.v
#### RTL code 
~~~
module incomp_if2 (input i0 , input i1 , input i2 , input i3, output reg y);
always @ (*)
begin
	if(i0)
		y <= i1;
	else if (i2)
		y <= i3;

end
endmodule
~~~
#### RTL simulation waveform

 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-18-20.png)

#### Dot viewer of incomp_if2.v
![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-24-38.png)


1. **Conditional assignments:**

   * If `i0` is true, `y` is assigned the value of `i1`.
   * Else if `i0` is false and `i2` is true, `y` is assigned the value of `i3`.

2. **Incomplete behavior if both `i0` and `i2` are false:**

   * The output `y` retains its previous value because there is no `else` clause, which may cause unintended latch inference.

### Lab on incomp_case.v

#### DOT viewer of incomp_case.v
![tool]()
