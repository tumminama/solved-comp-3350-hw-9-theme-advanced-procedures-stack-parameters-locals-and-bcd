Download Link: https://assignmentchef.com/product/solved-comp-3350-hw-9-theme-advanced-procedures-stack-parameters-locals-and-bcd
<br>
<ol>

 <li>Write a procedure named <em>ArraySeries</em> that fills an array of ten (10) numbers with the Fibonacci series. The procedure receives two arguments: the first is the offset of an array, and the second is an integer that specifies the array length. The first argument is passed by value and the second is passed by reference. In the main program, you should set the parameters of the array and print the array values before and after call to the procedure.</li>

</ol>




Please embed your code into your homework solution along with a screen shot of the run of the program.




TITLE

INCLUDE Irvine32.inc

.data count = 10

fibArray WORD 0 , 9 DUP(?)

.code main PROC push OFFSET fibArray push count call DisplayArray




push OFFSET fibArray push count call ArraySeries




push OFFSET fibArray push count call DisplayArray exit main ENDP




ArraySeries PROC push ebp mov ebp, esp mov esi, [ebp+12] mov ecx, [ebp+8] mov ebx, 0 L2:

mov eax, [esi + ebx] add eax, [esi + ebx – 2] add ebx, TYPE fibArray mov [esi + ebx], eax .IF ecx &gt; 9     add eax, 1     mov [esi + ebx], eax

.ENDIF loop L2 pop ebp ret

ArraySeries ENDP




DisplayArray PROC push ebp mov ebp, esp mov esi, [ebp+12] mov ecx, [ebp+8] mov ebx, 0 L1: movsx eax, WORD PTR [esi + ebx] add ebx, TYPE fibArray call Crlf call WriteInt Loop L1 call Crlf pop ebp ret 8

DisplayArray ENDP




END main




<ol start="2">

 <li>Draft a program that adds two BCD numbers (10-digits each). The first BCD number is stored in an array named <em>myAuburnID</em>, and the second in an array named <em>myAurbunIdRev</em>. The first number is your actual Auburn ID (with a prefix single zero digit and the remaining digits as the 9digits of your <em>Auburn ID</em>); the second is the value of <em>MyAuburnId </em>written backwards. Your program should do the following:</li>

</ol>




<ul>

 <li>Use shifts/rotates using <em>myAuburnID</em> to fill the array <em>myAuburnIdRev </em></li>

 <li>Display contents of the memory locations in question</li>

 <li>Add <em>myAuburnID and myAurbunIdRev </em>using BCD arithmetic 4) Store the sum in a variable named <em>Result</em>, and 5) Display contents of memory post execution.</li>

</ul>




Please embed your code into your homework solution along with a screen shot post execution.

TITLE

INCLUDE Irvine32.inc

.data

myAuburnID BYTE 0h, 9h, 0h, 3h, 9h, 0h, 7h, 9h, 7h, 7h myAuburnIDRev BYTE 10 DUP(?)

Result BYTE 10 DUP(0)

MSG1 BYTE “Result”, 0

MSG2 BYTE “MyAuburnID”, 0

.code main PROC mov esi, 0 mov ecx, 10 L3:

mov al, myAuburnID[esi]  mov edx, 9 sub edx, esi

mov myAuburnIDRev[edx], al inc esi loop L3

mov edx, OFFSET MSG2 call WriteString

mov edx, 0 call Crlf push OFFSET myAuburnID push 10 call DisplayArray call Crlf  mov edx, OFFSET MSG1 call WriteString mov edx, 0

call Crlf mov ecx, 10 mov edx, 9

mov esi, OFFSET myAuburnID mov edi, OFFSET myAuburnIDRev mov ebx, OFFSET Result

L1:

mov al, [esi + edx]        mov ah, [edi + edx]        add al, ah

daa

mov byte ptr [ebx + edx], al

dec edx

loop L1

push OFFSET Result push 10

CALL DisplayArray exit main ENDP




DisplayArray PROC push ebp mov ebp, esp mov esi, [ebp+12] mov ecx, [ebp+8] mov ebx, 0  L2:

movzx eax, BYTE PTR [esi + ebx]

add ebx, TYPE BYTE CALL WriteHex call Crlf Loop L2 pop ebp ret 8

DisplayArray ENDP




END main




<ol start="3">

 <li>Consider an isosceles triangle A with base 12 and height 20. Consider another triangle B formed using vertices which are the center of the sides of triangle A. Consider another triangle C whose vertices are similarly formed from B. Repeat this process ad infinitum.   Express the sum of the areas of all such triangles using a series and its closed form sum.  Compute the areas (a) by using only the first two terms of the series and (b) by using the closed form of the series sum.  Write a program to find the sums and use shifts to compute. What is the difference in the two computed sums?</li>

</ol>




Please embed your code into your homework solution along with a screen shot post execution.




TITLE

INCLUDE Irvine32.inc

.data height  DWORD   20 base    DWORD   12

msg1    BYTE “The sum of the first two of the series is: “, 0 msg2    BYTE “The sum of the series is: “, 0 msg3    BYTE “The difference in the two sums is: “, 0

.code

main PROC

call SumOfA call SumOfB call Diff main ENDP




SumOfA  PROC mov eax, height mov ebx, base

mul ebx

shr eax, 1  ; A = bh / 2 mov ebx, 5 mul ebx

shr eax, 2  ; A + A / 4 = 5 * A / 4 mov edx, OFFSET msg1 call WriteString call WriteDec call Crlf SumOfA ENDP

SumOfB PROC push eax mov ebx, 5

xor edx, edx div ebx

shl eax, 2  ; A = ((5 * A / 4) / 5) * 4

shl eax, 2 mov ebx, 3 xor edx, edx

div ebx     ; A / (1 – 1 / 4) = 4 * A / 3

mov edx, OFFSET msg2 mov ebx, eax call WriteString call WriteDec call Crlf pop eax SumOfB ENDP

Diff PROC sub ebx, eax mov eax, ebx mov edx, OFFSET msg3 call WriteString call WriteDec call Crlf Diff ENDP

Exit END main