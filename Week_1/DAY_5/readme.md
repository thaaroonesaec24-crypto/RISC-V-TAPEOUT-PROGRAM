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

### RTL simulation waveform

 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-03-56.png)

 ### DOT view of incomp_if.v
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-11-18.png)


* **Conditional assignment**: If the input i0 is true (logic 1), the output y is assigned the value of input i1.

* **Incomplete behavior**: If i0 is false (logic 0), the output y retains its previous value (no assignment), which may cause unintended latch inference due to missing an else branch.


  ---
## Lab on incomp_if2.v


### RTL simulation waveform
 ![tool]()
