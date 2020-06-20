# Syllabus
1. Lexical analysis
2. Syntax analysis
    - Top-down parser
---
## 1. Lexical analysis
1. Identify Tokens, Lexemes, Patterns
2. Write regular expression
3. Write regular difiniation
4. Draw transition diagrams
5. Design Non-deterministic finite automation (NFA)
6. Transform Non-deterministic finite automation (NFA) to Deterministic finite automation (DFA)

### Example
stmt -> *if* expr *then* stmt |
        *if* expr *then* stmt *else* stmt | other<br>
expr -> term *relop* term | term <br>
term -> *id* | *num*<br>

---
## 2. Syntax analysis (**Top-down parser**)
1. Eliminate Ambigutiy
2. Eliminte Left recursion and left factoring
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
        Non-terminal | First        | Follow
        ------------ | ------------ | ------
        S            | { i, a }        | { $, e }
        S'           | { e, empty } | { $, e }
        E            | { b }        | { t }
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
        - Is this grammar Left scan Left parse (**LL**)?
            - No, because 2 production in the same cell [S', e]; <br>
            because grammar is ambiguous.
---
        
        
