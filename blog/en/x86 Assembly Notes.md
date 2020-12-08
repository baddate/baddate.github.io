@def title = "x86 Assembly Note"
@def published = "25 July 2018"
@def tags = ["Tips", "Git"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

## Registers

![Registers](/_img/x86-registers.png)

_eight 32-bit general purpose registers_

__When referring to registers in assembly language, the names are not case-sensitive.__

## Memory and Addressing Modes

### Declaring Static Data Regions

>Data declarations should be preceded by the .DATA directive.

__located in memory next to one another.__

>*DB*---one byte  \
>*DW*---two bytes \
>*DD*---four bytes

__Example__

```
.DATA
var     DB 64           ; _Declare a byte, referred to as location var, containing the value 64._
var2    DB ?            ; _Declare an uninitialized byte, referred to as location var2._
        DB 10           ; _Declare a byte with no label, containing the value 10. Its location is var2 + 1._
X       DW ?            ; _Declare a 2-byte uninitialized value, referred to as location X._
Y       DD 30000        ; _Declare a 4-byte value, referred to as location Y, initialized to 30000._
bytes   DB 10 DUP(?)    ; Declare 10 uninitialized bytes starting at location bytes.
arr     DD 100 DUP(0)   ; Declare 100 4-byte words starting at location arr, all initialized to 0
```



### Addressing Memory

Some examples of `mov` instructions using address computations are:

mov eax, [ebx]				; _Move the 4 bytes in memory at the address contained in EBX into EAX_

mov [var], ebx				; *Move the contents of EBX into the 4 bytes at memory address  _var_. (Note,  _var_  is a 32-bit constant).*

mov eax, [esi-4]				; *Move 4 bytes at memory address ESI + (-4) into EAX*

mov [esi+eax], cl			; *Move the contents of CL into the byte at address ESI+EAX*

mov edx, [esi+4*ebx]	; *Move the 4 bytes of data at address ESI+4*EBX into EDX*

---

Some examples of __invalid address__ calculations include:



mov eax, [ebx-ecx]		; *Can only  __add__  register values*

mov [eax+esi+edi], ebx	; *At most  __2__  registers in address computation*

### Size Directives


|instruction|size  |
|:--:|:--:|
|`BYTE PTR`  | 1 byte |
|`WORD PTR`|2 bytes|
|`DWORD PTR`| 4 bytes|



__Example:__
mov BYTE PTR [ebx], 2				; *Move 2 into the single byte at the address stored in EBX.*

mov WORD PTR [ebx], 2			; *Move the 16-bit integer representation of 2 into the 2 bytes starting at the address in EBX.*

mov DWORD PTR [ebx], 2		; *Move the 32-bit integer representation of 2 into the 4 bytes starting at the address in EBX.*

## Instructions

__three categories:__

*data movement*

*arithmetic/logic*

*control-flow*

 __notation:__

```
  <reg32>		Any 32-bit register (EAX,  EBX,  ECX,  EDX,  ESI,  EDI,  ESP, or  EBP)

  <reg16>		Any 16-bit register (AX,  BX,  CX, or  DX)

  <reg8>		Any 8-bit register (AH,  BH,  CH,  DH,  AL,  BL,  CL, or  DL)

  <reg>			Any register

  <mem>		A memory address (e.g.,  [eax],  [var + 4], or  dword ptr [eax+ebx])

  <con32>		Any 32-bit constant

  <con16>		Any 16-bit constant

  <con8>		Any 8-bit constant

  <con>			Any 8-, 16-, or 32-bit constant
```
### Data Movement Instructions

`mov`  --- Move (Opcodes: 88, 89, 8A, 8B, 8C, 8E, ...)

_copy the data item referred to by its second operand into the location referred to by its first operand._

*Syntax*
```
  mov <reg>,<reg>
  mov <reg>,<mem>
  mov <mem>,<reg>
  mov <reg>,<const>
  mov <mem>,<const>
```


`push` --- Push stack (Opcodes: FF, 89, 8A, 8B, 8C, 8E, ...)

*places its operand onto the top of the hardware supported stack in memory. Specifically,  `push`  first __decrements ESP by 4__, then places its operand into the contents of the 32-bit location at address [ESP].*
_Syntax_
```
  push <reg32>
  push <mem>
  push <con32>
```
__`pop`__ — Pop stack

*removes the 4-byte data element from the top of the hardware-supported stack into the specified operand. It first __moves the 4 bytes__ located at memory location [SP] into the specified __register or memory location__, and then __increments SP by 4__.*

_Syntax_
```
  pop <reg32>
  pop <mem>
```


__`lea`__ --- Load effective address

The `lea` instruction places the _address_ specified by its second operand into the register specified by its first operand. Note, the _contents_ of the memory location are __not loaded__, only the __effective address__ is computed and placed into the register. This is useful for obtaining a pointer into a memory region.

_Syntax_
```
  lea <reg32>,<mem>
```

### Arithmetic and Logic Instructions

__`add`__ — Integer Addition

The `add` instruction adds together its two operands, storing the result in its first operand.	__just like `+=`__

_Syntax_
```
  add <reg>,<reg>
  add <reg>,<mem>
  add <mem>,<reg>
  add <reg>,<con>
  add <mem>,<con>
```

__`sub`__ — Integer Subtraction

The `sub` instruction stores in the value of its __first operand__ the __result__ of subtracting the value of its second operand from the value of its first operand.		(i.e. __first = first - second__)

_Syntax_
```
  sub <reg>,<reg>
  sub <reg>,<mem>
  sub <mem>,<reg>
  sub <reg>,<con>
  sub <mem>,<con>
```

__`inc`, `dec`__ — Increment, Decrement

*The `inc` instruction increments the contents of its operand by one.*

*The `dec` instruction decrements the contents of its operand by one.*

_Syntax_
```
  inc <reg>
  inc <mem>
  dec <reg>
  dec <mem>
```

__`imul`__ — Integer Multiplication

The *two-operand* form multiplies its two operands together and stores the result in the first operand. The __result (i.e. first) operand must be a register__.

(i.e. __first = first*second__)

The *three operand* form multiplies its second and third operands together and stores the __result in its first operand__. Again, __the result operand must be a register__. Furthermore, __the third operand is restricted to being a constant value__.

(i.e. __first = second*third__)

_Syntax_
```
  imul <reg32>,<reg32>
  imul <reg32>,<mem>
  imul <reg32>,<reg32>,<con>
  imul <reg32>,<mem>,<con>
```

__`idiv`__ — Integer Division

The `idiv` instruction divides the contents of the 64 bit integer __EDX:EAX__ (constructed by __viewing EDX as the most significant four bytes__ and __EAX as the least significant four bytes__) by __the specified operand value(i.e. the parameter)__. __The quotient result of the division is stored into EAX__, while __the remainder is placed in EDX__.

_Syntax_
```
  idiv <reg32>
  idiv <mem>
```

_Examples_

idiv ebx  --- divide the contents of EDX:EAX by the contents of EBX. Place the quotient in EAX and the remainder in EDX.

__`and`, `or`, `xor`__ — *Bitwise logical and*, *or* and *exclusive or*

*These instructions perform the specified logical operation (logical bitwise and, or, and exclusive or, respectively) on their operands, placing __the result in the first operand location__.*

_Syntax_
```
  and <reg>,<reg>
  and <reg>,<mem>
  and <mem>,<reg>
  and <reg>,<con>
  and <mem>,<con>

  or <reg>,<reg>
  or <reg>,<mem>
  or <mem>,<reg>
  or <reg>,<con>
  or <mem>,<con>

  xor <reg>,<reg>
  xor <reg>,<mem>
  xor <mem>,<reg>
  xor <reg>,<con>
  xor <mem>,<con>
```

__`not`__ — Bitwise Logical Not

__Logically negates__ the operand contents (that is, flips all bit values in the operand).

_Syntax_
```
  not <reg>
  not <mem>
```
_Example_

not BYTE PTR [var] — negate all bits in the byte at the memory location _var_

__`neg`__ — Negate

*Performs the two's __complement negation__ of the operand contents.*

_Syntax_
```
  neg <reg>
  neg <mem>
```
_Example_

  neg eax — EAX → - EAX

---
__`shl`, `shr`__ — Shift Left, Shift Right

*These instructions __shift the bits in their first operand's contents__ left and right, __padding the resulting empty bit positions with zeros__. The shifted operand can be shifted up to 31 places. The number of bits to shift is specified by the second operand, which can be either an 8-bit constant or __the register CL__. In either case, __shifts counts of greater then 31 are performed modulo 32__.*

_Syntax_
```asm
  shl <reg>,<con8>
  shl <mem>,<con8>
  shl <reg>,<cl>
  shl <mem>,<cl>

  shr <reg>,<con8>
  shr <mem>,<con8>
  shr <reg>,<cl>
  shr <mem>,<cl>
```
---

### Control Flow Instructions
notation <label> to refer to labeled locations in the program text.
__`jmp`__ — Jump

Transfers program control flow to the instruction at the memory location indicated by the operand.

_Syntax_
```asm
jmp <label>
```
_Example_
```asm
jmp begin  — Jump to the instruction labeled  begin.
```

__`jcondition`__ — Conditional Jump

_Syntax_
```
  je <label> (jump when equal)
  jne <label> (jump when not equal)
  jz <label> (jump when last result was zero)
  jg  <label> (jump when greater than)
  jge <label> (jump when greater than or equal to)
  jl <label> (jump when less than)
  jle <label> (jump when less than or equal to)
```

__`cmp`__ — Compare

*Compare the values of the two specified operands, setting the condition codes in the machine status word appropriately. The __result of the subtraction is discarded__.*

_Syntax_
```asm
  cmp <reg>,<reg>
  cmp <reg>,<mem>
  cmp <mem>,<reg>
  cmp <reg>,<con>
```

__`call`__,  __`ret`__ — Subroutine call and return

*The `call` instruction first __pushes__ the current code location onto the hardware supported stack in memory, and then performs an unconditional __jump to the code location indicated by the label operand__. Unlike the simple jump instructions, __the call instruction saves the location to return to when the subroutine completes__.*

*The  `ret`  instruction implements a subroutine return mechanism. This instruction first __pops__ a code location off the hardware supported in-memory stack. It then performs an unconditional __jump to the retrieved code location__.*

_Syntax_
```
call <label>
ret
```

## Calling Convention

*To allow separate programmers to share code and develop libraries for use by many programs, and to simplify the use of subroutines in general, programmers typically adopt a common __calling convention__. The calling convention is __a protocol about how to call and return from routines__.*

![Stack during Subroutine Call](/_img/stack-convention.png "Stack during Subroutine Call")


The image above depicts the contents of the stack during the execution of a subroutine with three parameters and three local variables. The cells depicted in the stack are 32-bit wide memory locations, thus the memory addresses of the cells are 4 bytes apart. The first parameter resides at an offset of 8 bytes from the base pointer. Above the parameters on the stack (and below the base pointer), the call instruction placed the return address, thus leading to an extra 4 bytes of offset from the base pointer to the first parameter. When the ret instruction is used to return from the subroutine, it will jump to the return address stored on the stack.
(p.s. *ESP (the stack pointer), EBP(the base pointer)*)


#### Caller Rules
To make a subrouting call, the caller should:

1.  Before calling a subroutine, the caller should save the contents of certain registers that are designated  _caller-saved_. The caller-saved registers are EAX, ECX, EDX. Since the called subroutine is __allowed to modify these registers__, if the caller relies on their values after the subroutine returns, the caller must push the values in these registers onto the stack (so they can be restore after the subroutine returns).

2.  To pass parameters to the subroutine, __push them onto the stack before the call__. The parameters should __be pushed in inverted order__ (i.e. last parameter first). Since the stack grows down, the first parameter will be stored at the lowest address (this inversion of parameters was historically used to allow functions to be passed a variable number of parameters).

3.  To call the subroutine, use the  `call`  instruction. This instruction places the return address on top of the parameters on the stack, and branches to the subroutine code. This invokes the subroutine, which should __follow the callee rules__ below.

After the subroutine returns (immediately following the `call` instruction), __the caller can__ expect to __find the return value of the subroutine in the register EAX__. *To restore the machine state, the caller should:*

1.  *Remove the parameters from stack*. This restores the stack to its state before the call was performed.

2.  *Restore the contents of caller-saved registers (EAX, ECX, EDX) by popping them off of the stack*. The caller can assume that no other registers were modified by the subroutine.

__Example__
```
	push [var] ; Push last parameter first
	push 216   ; Push the second parameter
	push eax   ; Push first parameter last

	call _myFunc ; Call the function (assume C naming)

	add esp, 12
```

__Note that__ after the call returns, the caller cleans up the stack using the add instruction. We have 12 bytes (3 parameters * 4 bytes each) on the stack, and the stack grows down. Thus, __to get rid of the parameters, we can simply add 12 to the stack pointer__.


#### Callee Rules

The definition of the subroutine should adhere to the following rules at the beginning of the subroutine:

1.  Push the value of EBP onto the stack, and then copy the value of ESP into EBP using the following instructions:
        `push ebp`
        `mov  ebp, esp`
    This initial action maintains the  _base pointer_, EBP. The base pointer is used by convention as a point of reference for finding parameters and local variables on the stack. When a subroutine is executing, the base pointer holds a copy of the stack pointer value from when the subroutine started executing. Parameters and local variables will always be located at known, constant offsets away from the base pointer value. We push the old base pointer value at the beginning of the subroutine so that we can later restore the appropriate base pointer value for the caller when the subroutine returns. Remember, the caller is not expecting the subroutine to change the value of the base pointer. We then move the stack pointer into EBP to obtain our point of reference for accessing parameters and local variables.

2.  Next, allocate local variables by making space on the stack. Recall, the stack grows down, so to make space on the top of the stack, the stack pointer should be decremented. The amount by which the stack pointer is decremented depends on the number and size of local variables needed. For example, if 3 local integers (4 bytes each) were required, the stack pointer would need to be decremented by 12 to make space for these local variables (i.e.,  sub esp, 12). As with parameters, local variables will be located at known offsets from the base pointer.

3.  Next, save the values of the  _callee-saved_  registers that will be used by the function. To save registers, push them onto the stack. The callee-saved registers are EBX, EDI, and ESI (ESP and EBP will also be preserved by the calling convention, __but need not be pushed on the stack during this step__).

After these three actions are performed, the body of the subroutine may proceed. When the subroutine is returns, it must follow these steps:

1.  Leave the return value in EAX.

2.  __Restore the old values of any callee-saved registers (EDI and ESI) that were modified__. The register contents are restored by popping them from the stack. The registers should be popped in the inverse order that they were pushed.

3.  __Deallocate local variables__. The obvious way to do this might be to add the appropriate value to the stack pointer (since the space was allocated by subtracting the needed amount from the stack pointer). In practice, a less error-prone way to deallocate the variables is to move the value in the base pointer into the stack pointer:  mov esp, ebp. This works because the base pointer always contains the value that the stack pointer contained immediately prior to the allocation of the local variables.

4.  __Immediately before returning, restore the caller's base pointer value__ by popping EBP off the stack. Recall that the first thing we did on entry to the subroutine was to push the base pointer to save its old value.

5.  Finally, __return to the caller by executing a  `ret`  instruction__. This instruction will find and remove the appropriate return address from the stack.

__Example__

```asm
.486
.MODEL FLAT
.CODE
PUBLIC _myFunc
_myFunc PROC
  ; Subroutine Prologue
  push ebp     ; Save the old base pointer value.
  mov ebp, esp ; Set the new base pointer value.
  sub esp, 4   ; Make room for one 4-byte local variable.
  push edi     ; Save the values of registers that the function
  push esi     ; will modify. This function uses EDI and ESI.
  ; (no need to save EBX, EBP, or ESP)

  ; Subroutine Body
  mov eax, [ebp+8]   ; Move value of parameter 1 into EAX
  mov esi, [ebp+12]  ; Move value of parameter 2 into ESI
  mov edi, [ebp+16]  ; Move value of parameter 3 into EDI

  mov [ebp-4], edi   ; Move EDI into the local variable
  add [ebp-4], esi   ; Add ESI into the local variable
  add eax, [ebp-4]   ; Add the contents of the local variable
                     ; into EAX (final result)

  ; Subroutine Epilogue
  pop esi      ; Recover register values
  pop  edi
  mov esp, ebp ; Deallocate local variables
  pop ebp ; Restore the caller's base pointer value
  ret
_myFunc ENDP
END
```

The subroutine prologue performs the standard actions of saving a snapshot of the stack pointer in EBP (the base pointer), allocating local variables by decrementing the stack pointer, and saving register values on the stack.

In the body of the subroutine we can see the use of the base pointer. Both parameters and local variables are located at constant offsets from the base pointer for the duration of the subroutines execution. In particular, we notice that since parameters were placed onto the stack before the subroutine was called, they are always located below the base pointer (i.e. at higher addresses) on the stack. The first parameter to the subroutine can always be found at memory location [EBP+8], the second at [EBP+12], the third at [EBP+16]. Similarly, since local variables are allocated after the base pointer is set, they always reside above the base pointer (i.e. at lower addresses) on the stack. In particular, the first local variable is always located at [EBP-4], the second at [EBP-8], and so on. This conventional use of the base pointer allows us to quickly identify the use of local variables and parameters within a function body.

The function epilogue is basically a mirror image of the function prologue. The caller's register values are recovered from the stack, the local variables are deallocated by resetting the stack pointer, the caller's base pointer value is recovered, and the ret instruction is used to return to the appropriate code location in the caller.


## Reference

[x86 Assembly Guide](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html )
