# Syllabus
1. Lexical analysis
2. Syntax analysis
    - Top-down parser
---
## 1. Lexical analysis
1. Identify Tokens, Lexemes, Patterns
2. Write regular expression
3. Write regular definition
4. Draw transition diagrams
5. Design Non-deterministic finite automation (NFA)
6. Transform Non-deterministic finite automation (NFA) to Deterministic finite automation (DFA)

### Example
1.  stmt  -> *if* R *then* stmt |
            *if* R *then* stmt *else* stmt | E<br>
    E     -> E + T | E - T | T<br>
    T     -> T * F | T / F | F<br>
    F     -> num | id | (E)<br>
    R     -> id relop id<br>
    relop -> > | < | >= | <=<br>
---
1. Solution<br>
    - **Step #1** Identify Tokens, Lexemes, Patterns
        Tokens      | Lexemes       | Patterns
        ----------- | ------------- | --------
        if          | if            | if
        then        | then          | then
        else        | else          | else
        add_op      | '+'           | '+' 
        sub_op      | '-'           | '-'
        mul_op      | '*'           | '*'
        dev_op      | '/'           | '/'
        left_brack  | '('           | '('
        right_brack | ')'           | ')'
        relop       | >, <, >=, <=  | >, <, >=, <= 
        id          | x1, pi,       | letter floowed by letters or digits
        num         | 3.14, 5.1     | digit or digits
    
    - **Step #2** Regular Expression<br>
        id: letter (letter | digit)\* <br>
        num: digit digit*
    - **Step #3** Regular definition
            stmt  -> *if* R *then* stmt |
            *if* R *then* stmt *else* stmt | E<br>
            E     -> E + T | E - T | T<br>
            T     -> T * F | T / F | F<br>
            F     -> num | id | (E)<br>
            R     -> id relop id<br>
            relop -> > | < | >= | <=<br>
        
        ---------------
        if -> if <br>
        then -> then<br>
        else -> else<br>
        num -> digit digit*<br>
        digit -> 0|1|2|3|4|5|6|7|8|9<br>
        id -> letter(letter | digit)*<br>
        letter -> a|b|...z|A|B|...|Z<br>
    - **Step #4** Transition Diagrams<br>
    start -> 
    0. 
        - i
            - f
                - return (if, if)
        - e
            - l
                - s
                    - e
                        - return(else, else)
        - t
            - h
                - e
                    - n
                        -  return(then, then)
        - \<
            - \=
                - return(relop, LE)
            - other 
                - return (relop LT)
        - \>
            - \=
                - return (relop, GE)
            - other
                - return (relop, GT)
    
    - **Step #5** Design Non-deterministic finite automation (NFA)
        - solution
            - 1. #### RE=letter(letter|digit)\*
                - ![RE=letter(letter|digit)\*](./images/RE01.png)<br>
            - 2. #### RE=digit digit\*
                - ![RE=digit digit\*](./images/RE02.png)<br>
    - **Step #6** Transform Non-deterministic finite automation (NFA) to Deterministic finite automation (DFA)
        - [SOLUTION](./Ahmed_Mohamed_Abd_El-Rahman/README.md)
    
---
## 2. Syntax analysis (**Top-down parser**)
1. Eliminate Ambiguity
2. Eliminate Left recursion and left factoring
3. Draw transition diagram (*not necessary*)
4. Apply first/follow operators
5. Get parsing table
6. Parse statement

### Example
1.  E -> E + T | T<br>
    T -> T * F | F<br>
    F -> ( E ) | id<br>
design a non recursive non backtracking predictive parser.<br>
using your design parse the following statment `id + id * id` 

2.  S -> iEtS | iEtSeS | a <br>
    E -> b <br>
design a non recursive non backtracking predictive parser.<br>
using your design parse the following statment `ibtibtaea` 

---
1. Solution<br>
    - Step **1** Eliminate Ambigutiy (*don't do it in exam*)
    - Step **2** Eliminate Left Recursion and Left Factoring
        - Left Recursion<br>
        E   -> T E'<br>
        E'  -> + T E' | empty<br>
        T   -> F T'<br>
        T'  -> * F T' | empty<br>
        F   -> ( E ) | id<br>
        - No left factoring
    - Step **3** Draw transition diagram (*not necessary*)
    - Step **4** Apply First/Follow operators
        Non-terminal    | First        | Follow
        --------------- | ------------ | ------
        E               | { (, id }    | { $, ) }          
        E'              | { +, empty } | { $, ) }
        T               | { (, id }    | { +, $, ) }
        T'              | { *, empty } | { +, $, ) }
        F               | { (, id }    | { *, +, $, ) }
    - Step **5** Get parsing table <br>
        Non-Terminal    | id            | +             | *             | (             | )             | $
        --------------- | ------------- | ------------- | ------------- | ------------- | ------------- | -------------
        E               | E   -> T E'   |               |               | E   -> T E'   |               |   
        E'              |               | E'  -> + T E' |               |               | E' -> empty   | E' -> empty
        T               | T   -> F T'   |               |               | T   -> F T'   |               |
        T'              |               | T'  -> empty  | T'  -> * F T' |               | T'  -> empty  | T'  -> empty
        F               | F   -> id     |               |               | F   -> ( E )  |               |
    - Step **6** Parse statement
        Stack       | Input     | Output
        ----------- | --------- | ------
        $E          | id+id*id$ |
        $E'T        | id+id*id$ | E   -> T E'
        $E'T'F      | id+id*id$ | T   -> F T'
        $E'T'id     | id+id*id$ | F   -> id
        $E'T'       | +id*id$   |
        $E'         | +id*id$   | T'  -> empty
        $E'T+       | +id*id$   | E'  -> + T E'
        $E'T        | id*id$    |
        $E'T'F      | id*id$    | T   -> F T'
        $E'T'id     | id*id$    | F   -> id
        $E'T'       | *id$      |
        $E'T'F*     | *id$      | T'  -> * F T'
        $E'T'F      | id$       |
        $E'T'id     | id$       | F   -> id
        $E'T'       | $         |
        $E'         | $         | T'  -> empty
        $           | $         | E'  -> empty 
        Stop, Statement parsed well
        - Is this grammar Left scan Left parse (**LL(1)**)?
            - yes, there is no cell have two production

---

2.  SOLUTION<br>
    - Step **1** Eliminate Ambigutiy (*don't do it in exam*)
    - Step **2** Eliminate Left Recursion and Left Factoring
        - There is no recursion
        - Factoring
            - S  -> iEtS*S'* | a<br>
            - S' -> eS | empty<br>
            - E  -> b<br>
    - Step **3** Draw transition diagram (*not necessary*)
    - Step **4** Apply First/Follow operators
        Non-terminal | First            | Follow
        ------------ | ---------------- | ------
        S            | { i, a }         | { $, e }
        S'           | { e, empty }    | { $, e }
        E            | { b }            | { t }
    - Step **5** Get parsing table
        Non-terminal | a      | b     | e                         | i             | t | $
        ------------ | ------ | ----- | ------------------------- | ------------- | - | ----------- 
        S            | S -> a |       |                           | S -> iEtS*S'* |   |  
        S'           |        |       |**S' -> eS<br>S' -> empty**|               |   | S' -> empty
        E            |        | E-> b |                           |               |   |   
    - Step **6** Parse statement
        Stack           | Input           | Output
        --------------- | --------------- | ---------------
        $ S             | ibtibtaea$      |
        $ S'StEi        | ibtibtaea$      | S -> iEtS*S'*
        $ S'StE         | btibtaea$       |
        $ S'Stb         | btibtaea$       | E -> b
        $ S'St          | tibtaea$        |
        $ S'S           | ibtaea$         |
        $ S'S'StEi      | ibtaea$         | S -> iEtS*S'*
        $ S'S'StE       | btaea$          |
        $ S'S'Stb       | btaea$          | E -> b
        $ S'S'St        | taea$           | 
        $ S'S'S         | aea$            |
        $ S'S'a         | aea$            | S -> a
        $ S'S'          | ea$             |
        $ S'Se          | ea$             | S' -> eS
        $ S'S           | a$              | 
        $ S'a           | a$              | S -> a
        $ S'            | $               | 
        $               | $               | S' -> empty
        Stop Statement parsed
        - Is this grammar Left scan Left parse (**LL(1)**)?
            - No, because 2 production in the same cell [S', e]; <br>
            because grammar is ambiguous.
---
        
        
