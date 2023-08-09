.section .data
input_x_prompt  :   .asciz  "Please enter x: "
input_y_prompt  :   .asciz  "Please enter y: "
input_spec  :   .asciz  "%d"
result      :   .asciz  "x^y = %d\n"
.section .text


.global main

#main
main:

#create room on stack for x
sub sp, sp, 8
# input x prompt
ldr x0, = input_x_prompt
bl printf	
#get input x value
# spec input
ldr x0, = input_spec
mov x1, sp
bl scanf
ldr x19, [sp]


#get y input value
# enter y output
ldr x0, = input_y_prompt
bl printf
# spec input
ldr x0, = input_spec
mov x1, sp
bl scanf
ldr x20, [sp]

# add both elements to x27, setting output value to 1, if x27 is 0 then exit with output value of 1
ADD x27, x19, x20
mov x26, 1
CBZ x27, exit


# setting initial value of x26 to 0, if x is 0 exit
mov x26, 0
CBZ x19, exit

# set initial value of x23 to 0 and x26 to 1
# x23 is iteration variable, x26 is holding value of multiplication
mov x23, 0
mov x26, 1

## if y is 0, run exit branch
CBZ x20, exit


# multiply branch we are adding 1 so we know what iteration we are on
MULTIPLY:  ADD x23, x23, 1
    # multiplying x value (x19) by the past value from previous multiplication (x26)
    MUL x26, x26, x19
    # subtracting y by the iterator value
    SUB x25, x20, x23
    # checking if 0, if 0 run exit branch
    CBZ x25, exit
    # if it is not 0, then we branch to the start of multiply
    B MULTIPLY

# branch to this label on program completion
exit:
    mov x1, x26
    ldr x0, =result
    bl printf
	mov x0, 0
	mov x8, 93
	svc 0
	ret

