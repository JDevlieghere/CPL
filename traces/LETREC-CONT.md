LETREC-CONT
===========

## Program 1

```racket
2
```
``` racket
(define p1 (a-program (const-exp 2)))
```

 - Evaluating 2 in  [ ]
 - Sending 2 to []
 - End of computation

 ## Program 2

```racket
2 - 4
```

```racket
(define p2 (a-program (diff-exp (const-exp 2) (const-exp 4))))
```

 - Evaluating 2-4 in [ ]
 - Evaluating 2 in ([ ] - <4>)
 - Sending 2 to ([ ] - <4>)
 - Evaluating 4 in (2 - [])
 - Sending 4 to (2 - [])
 - Sending -2 to [ ]
 - End of computation

## Program 3

```racket
10 - (2 - 4)
```

```racket
(define p3
  (a-program (diff-exp (const-exp 10)
                       (diff-exp (const-exp 2) (const-exp 4)))))
```

 - Evaluating 10 - (2 - 4) in [ ]
 - Evaluating 10 in ([ ] - <(2 -4)>)
 - Sending 10 to ([ ] - <(2 -4)>)
 - Evaluating (2-4) in (10 - [ ])
 - Evaluating 2 in (10 - ([ ] - <4>))
 - Sending 2 to (10 - ([ ] - <4>))
 - Evaluating 4 in (10 - (2 - [ ]))
 - Sending 4 to (10 - (2 - [ ]))
 - Sending -2 to (10 - [])
 - Sending 12 to []
 - End of computation

## Program 6

```racket
let x = 0 in x - 4
```

```racket
(define p6
  (a-program (let-exp 'x
                      (const-exp 0)
                      (diff-exp (var-exp 'x) (const-exp 4)))))
```

 - Evaluating let x = 0 in x - 4 in [ ]
 - Evaluating 0 in (let x = [] in <x-4>)
 - Sending 0 to (let x = [] in <x-4>)
 - Evaluating x-4 in [ ]
 - Evaluating x in ([ ] - <4>)
 - Sending 0 to ([ ] - <4>)
 - Evaluating 4 in (0 - [ ])
 - Sending 4 to (0 - [ ])
 - Sending -4 to [ ]
 - End of computation

## Examen 1

```racket
let x=0 in (1-x)
```

```racket
(define ex1
  (a-program
   (let-exp 'x (const-exp 0) (diff-exp (const-exp 1) (var-exp 'x)))))
```

 - Evaluating let x = 0 in (1 - x) in [ ]
 - Evaluating 0 in (let x = [] in <1-x>)
 - Sending 0 to (let x = [] in <1-x>)
 - Evaluating 1 -x in [ ]
 - Evaluating 1 in ([ ] - <x>)
 - Sending 1 to ([ ] - <x>)
 - Evaluating x in (1 - [ ])
 - Sending 0 to (1 - [ ])
 - Sending 1 to [ ]
 - End of computation

## Examen 2

```racket
(15 - if zero?0 then 1 else 2)
```

```racket
(define ex2
  (a-program
   (diff-exp (const-exp 15)
             (if-exp (zero?-exp (const-exp 0))
                     (const-exp 1)
                     (const-exp 2)))))
```

 - Evaluating 15 - (if zero? 0 then 1 else 3) in [ ]
 - Evaluating 15 in ([] - <(if zero? 0 then 1 else 3)>)
 - Sending 15 to ([] - <(if zero? 0 then 1 else 3)>)
 - Evaluating (if zero? 0 then 1 else 3) in (15 - [ ])
 - Evaluating zero? 0 in (15 - if [ ] then <1> else <3>)
 - Evaluating 0 in (15 - if zero? [ ] then <1> else <3>)
 - Sending 0 to (15 - if zero? [ ] then <1> else <3>)
 - Sending True to  (15 - if [ ] then <1> else <3>)
 - Evaluating 1 in (15 - [ ])
 - Sending 1 to (15 - [ ])
 - Sending 14 to [ ]
 - End of computation
