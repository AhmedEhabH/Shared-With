## RE=letter(letter|digit)\* 
### NFA:
![RE=letter(letter|digit)\*](../images/RE01.png)<br>

set = {letter, digit}

e-close (0) = {0} A

move(A, l) = {1}<br>
e-close ({1}) = {1, 2, 3, 5, 8} -> B<br>
D-trans[A, l] = B<br>

move(A, d) = {}

move(B, l) = {4}<br>
e-close ({4}) = {2, 3, 4, 5, 7, 8} -> C<br>
D-Trans[B, l] = C<br>

move(B, d) = {6}<br>
e-close ({6}) = {2, 3, 5, 6, 7, 8} -> D<br>
D-trans[B, d] = D<br>

move(C, l) = {4}<br>
e-close ({4}) = C<br>
D-Trans[C, l] = C<br>

move(C, d) = {6}<br>
e-close ({6}) = D<br>
D-trans[C, d] = D<br>

move(D, l)={4}<br>
e-close ({4}) = C<br>
D-Trans[D, l] = C<br>

move(D, d) = {6}<br>
e-close ({6}) = D<br>
D-trans[D, d] = D<br>

state   | l | d
-----   | - | -
A       | B |
B       | C | D
C       | C | D
D       | C | D

### DFA
![DFA\*](../images/RE01-SOL.png)<br>

---
## RE=digit digit\*
### NFA:
![RE=digit digit\*](../images/RE02.png)<br>
Set = {digit}

e-close {0} = {0} -> A

move(A, d) = {1}<br>
e-close ({1}) = {1, 2, 4} -> B<br>
D-trans(A, d) = B<br>

move (B, d) = {3}<br>
e-close({3}) = {2, 4} -> C<br>
D-trans(B, d) = C<br>

move(C, d) = {3}<br>
e-close({3}) = C<br>
D-trans(C, d) = C<br>

state   | d
------- | -
A       | B
B       | C
C       | C
### DFA
![DFA\*](../images/RE02-SOL.png)<br>
