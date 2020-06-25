Non-Terminal | First        | Follow
------------ | -----        | ------
E            | {(, id}      | {$, )}
E'           | {+, empty}   | {$, )}
T            | {(, id}      | {+, $, )}
T'           | {*, empty}   | {+, $, )}
F            | {(, id}      | {*, +, $, )}

---

E -> T E'
first
if first have empty
follow

E -> T
follow

---
Non-Terminal | First        | Follow
------------ | -----        | ------
S            | {i, a}       | {$, e}
S'           | {e, empty}   | {$, e}
E            | {b}          | {t}

---
## Nearest get first
## Farest get follow