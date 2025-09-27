# Optimisation in synthesis

## Lab on imcomplete if case

### incomp_if.v

#### RTL code
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
-

 #### DOT view of incomp_if.v
 
 ![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-11-18.png)
-

* **Conditional assignment**: If the input i0 is true (logic 1), the output y is assigned the value of input i1.

* **Incomplete behavior**: If i0 is false (logic 0), the output y retains its previous value (no assignment), which may cause unintended latch inference due to missing an else branch.


  ---
### incomp_if2.v

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
-

#### Dot viewer of incomp_if2.v

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-24-38.png)
-

1. **Conditional assignments:**

   * If `i0` is true, `y` is assigned the value of `i1`.
   * Else if `i0` is false and `i2` is true, `y` is assigned the value of `i3`.

2. **Incomplete behavior if both `i0` and `i2` are false:**

   * The output `y` retains its previous value because there is no `else` clause, which may cause unintended latch inference.
---
## Lab on incomplete case

 ### incomp_case.v
 
#### RTL code

~~~
module incomp_case (input i0 , input i1 , input i2 , input [1:0] sel, output reg y);
always @ (*)
begin
	case(sel)
		2'b00 : y = i0;
		2'b01 : y = i1;
	endcase
end
endmodule
~~~

#### RTL simulation waveform

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-37-30.png)
-

#### DOT viewer of incomp_case.v

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-30-55.png)
-
1. **Selective output based on `sel`:**

   * When `sel` is `2'b00`, `y` is assigned the value of `i0`.
   * When `sel` is `2'b01`, `y` is assigned the value of `i1`.

2. **Incomplete `case` statement:**

   * For other values of `sel` (`2'b10` and `2'b11`), `y` is **not assigned**, which can cause `y` to hold its previous value, potentially inferring a latch.
---
### bad_case.v

##### RTL code
~~~
module bad_case (input i0 , input i1, input i2, input i3 , input [1:0] sel, output reg y);
always @(*)
begin
	case(sel)
		2'b00: y = i0;
		2'b01: y = i1;
		2'b10: y = i2;
		2'b1?: y = i3;
		//2'b11: y = i3;
	endcase
end
endmodule
~~~

#### RTL simulation waveform

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-46-22.png)
-

#### DOT viewer for bad_case.v

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-50-08.png)
-

#### GLS simulation 

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2022-56-30.png)
-

1. **Output selection based on `sel`:**
   * Assigns `y` based on `sel` values:
     * `2'b00` → `y = i0`
     * `2'b01` → `y = i1`
     * `2'b10` → `y = i2`
     * `2'b1?` (a wildcard covering both `2'b10` and `2'b11`) → `y = i3`

2. **Conflicting case entries and overlap:**
   * The case `2'b1?` overlaps with `2'b10`, causing ambiguity because `2'b10` is matched twice (`y = i2` and `y = i3`).
   * The commented out `2'b11` case is covered by `2'b1?`, but this wildcard usage can cause synthesis issues or unexpected behavior.

---


## Labs for FOR loop

### mux_generate.v

#### RTL code

 ~~~
module mux_generate (input i0 , input i1, input i2 , input i3 , input [1:0] sel  , output reg y);
wire [3:0] i_int;
assign i_int = {i3,i2,i1,i0};
integer k;
always @ (*)
begin
for(k = 0; k < 4; k=k+1) begin
	if(k == sel)
		y = i_int[k];
end
end
endmodule
~~~~


#### RTL simulation wave

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2023-05-56.png)
-

* Implements a 4-to-1 multiplexer using a `for` loop.
* Inputs `i0` to `i3` are combined into a 4-bit wire `i_int` for easy indexing.
* The loop runs from 0 to 3 inside an always block.
* When the loop index `k` matches the select signal `sel`, it assigns `y` to `i_int[k]`.
* This ensures only the selected input is assigned to the output.
* This approach simplifies the code by avoiding multiple nested `if-else` or `case` statements, making it more concise.

  ---

### demux_generate.v
#### RTL code
~~~
module demux_generate (output o0 , output o1, output o2 , output o3, output o4, output o5, output o6 , output o7 , input [2:0] sel  , input i);
reg [7:0]y_int;
assign {o7,o6,o5,o4,o3,o2,o1,o0} = y_int;
integer k;
always @ (*)
begin
y_int = 8'b0;
for(k = 0; k < 8; k++) begin
	if(k == sel)
		y_int[k] = i;
end
end
endmodule
~~~

#### RTL simulation wave 

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2023-13-56.png)
-

* Implements an 8-output demultiplexer using a `for` loop.
* Outputs `o0` to `o7` are combined into an 8-bit register `y_int` for straightforward indexing.
* `y_int` is first cleared to 0, then only the bit corresponding to the select signal `sel` is set to the input `i`.
* This effectively routes the single input `i` to the chosen output, while all others remain zero.
* The loop runs sequentially during simulation, ensuring no latches are created.
* This method offers a compact alternative to writing multiple `if-else` or `case` statements.

## Lab on generate for loop

### 8-bit Ripple Carry Adder

#### RTL code
~~~
module rca (input [7:0] num1 , input [7:0] num2 , output [8:0] sum);
wire [7:0] int_sum;
wire [7:0]int_co;
genvar i;
generate
	for (i = 1 ; i < 8; i=i+1) begin
		fa u_fa_1 (.a(num1[i]),.b(num2[i]),.c(int_co[i-1]),.co(int_co[i]),.sum(int_sum[i]));
	end
endgenerate
fa u_fa_0 (.a(num1[0]),.b(num2[0]),.c(1'b0),.co(int_co[0]),.sum(int_sum[0]));
assign sum[7:0] = int_sum;
assign sum[8] = int_co[7];
endmodule
~~~
#### 1-bit Full adder
~~~
module fa (input a , input b , input c, output co , output sum);
	assign {co,sum}  = a + b + c ;
endmodule
~~~

#### RTL simulation

![tool](https://github.com/thaaroonesaec24-crypto/RISC-V-TAPEOUT-PROGRAM/blob/main/Week_1/DAY_5/pictures/Screenshot%20from%202025-09-27%2023-27-15.png)
-
#### DOT viwer of rca.v
![tool]()
