# IMPLICIT-REFS

Manual intepretation of some of the example programs for the language IMPLICIT-REFS (EOPL)

I've employed the following shorthands:

 - value-of-program has been omitted
 - vof = value-of
 - app = apply-procedure
 - r = rho (store)
 - s = sigma (environment)

## Exam

```racket
let d = let f = proc(x) x
      in proc(g) begin f:=g; (f 1) end
in (d (proc(x)2))
```

```racket
(define ex
  (a-program
   (let-exp 'd
            (let-exp 'f
                     (proc-exp 'x (var-exp 'x))
                     (proc-exp 'g (begin-exp
                                    (assign-exp 'f (var-exp 'g))
                                    (list (call-exp (var-exp 'f) (const-exp 1))))))
            (call-exp (var-exp 'd) (proc-exp 'x (const-exp 2))))))
```


 - Evaluating let-exp
 - Evaluating let-exp
 - Evaluating proc-exp
 - Return (proc-val 'x (var-exp 'x))
 - [(0, (proc-val 'x (var-exp 'x)  []))]r and [f = 0]s
 - Evaluating proc-exp
 - Return (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1))))))
 - [(1, (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1)))))) [f = 0]), (0, (proc-val 'x (var-exp 'x)  []))] and [d=1, f=0]r
 - Evaluating call-exp
 - Evaluating var-exp
 - Return (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1)))))) [f = 0])
 - Evaluating proc-exp
 - Return (proc-val 'x (const-exp 2))
 - Apply procedure
 - [(2,  (proc-val 'x (const-exp 2) [d=1]))(1, (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1)))))) [f = 0]), (0, (proc-val 'x (var-exp 'x)  []))] and [g=2, d=1]r
 - Evaluating begin-exp
 - Evaluating assign-exp
 - [(2,  (proc-val 'x (const-exp 2) [d=1])), (1, (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1)))))) [f = 0]), (0, (proc-val 'x (const-exp 2) [d=1]))] and [g=2, d=1]r
 - Evaluating call-exp
 - Evaluating var-exp
 - Return (proc-val 'x (const-exp 2) [d=1]))
 - Evaluating const-exp
 - Return 1
 - Apply procedure
 - [(3, (const-exp 1))(2,  (proc-val 'x (const-exp 2) [d=1])), (1, (proc-exp 'g (begin-exp (assign-exp 'f (var-exp 'g)) (list (call-exp (var-exp 'f) (const-exp 1)))))) [f = 0]), (0, (proc-val 'x (const-exp 2) [d=1]))] and [g=2, d=1]r
 - Evaluating const-exp
 - Return 2

## Program 1

```racket
2
```
``` racket
(define p1 (a-program (const-exp 2)))
```

 - Entering vof cont-exp
 - Return vof  2

## Program 2

```racket
2 - 4
```

```racket
(define p2 (a-program (diff-exp (const-exp 2) (const-exp 4))))
```

 - Entering vof diff-exp
 - Entering vof const-exp
 - Return vof 2
 - Entering vof const-exp
 - Return vof 4
 - Return vof 2-4 = -2


## Program 3


```racket
10 - (2 - 4)
```

```racket
(define p3
  (a-program (diff-exp (const-exp 10)
                       (diff-exp (const-exp 2) (const-exp 4)))))
```
 - Entering vof diff-exp
 - Entering vof const-exp
 - Return vof 10
 - Entering vof diff-exp
 - Entering vof const-exp
 - Return vof 2
 - Entering vof const-exp
 - Return vof 4
 - Return vof 4-2 = -2
 - Return vof 10 - (-2) = 12

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

 - Entering vof let-exp
 - Entering vof cons-exp
 - Return vof 0
 - `[(0, #num-val 0)]r` and `[x=0]s`
 - Entering vof diff-exp
 - Entering vof var-exp
 - Return vof 0
 - Entering vof const-exp
 - Return vof 4
 - Return 0 - 4 = -4

## Program 7

```racket
let x = 0 in let x = 2 in x
```

```racket
(define p7
  (a-program (let-exp 'x (const-exp 0)
                      (let-exp 'x (const-exp 2)
                               (var-exp 'x)))))
```

 - Entering vof let-exp
 - Entering vof const-exp
 - Return vof 0
 - `[(0, #num-val 0)]r` and `[x=0]s`
 - Entering vof let-exp
 - Entering vof const-exp
 - Return vof 2
 - `[(1, #num-val 2), (0, #num-val 0)]r` and `[x=1,x=0]s`
 - Entering vof var-exp
 - Return 2


## Program 8

```racket
let f = proc(x) (x - 11)
in (f (f 7))
```

```racket
(define p8
  (a-program (let-exp 'f
                      (proc-exp 'x
                        (diff-exp
                          (var-exp 'x)
                          (const-exp 11)))
                      (call-exp
                        (var-exp 'f)
                        (call-exp
                          (var-exp 'f)
                          (const-exp 7))))))
```

 - Entering vof let-exp
 - Entering vof proc-exp
 - Return `(#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))`
 - `[(0, (#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))]r` and `[f=0]s`
 - Entering vof call-exp
 - Entering vof var-exp
 - Return `(#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))`
 - Entering call-exp
 - Entering vof var-exp
 - Return `(#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))`
 - Entering vof const-exp
 - Return vof 7
 - Entering app
 - `[(1, (#const-exp 7), (0, (#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))]r` and `[x=1,f=0]s`
 - Entering vof diff-exp
 - Entering vof var-exp
 - Return 7
 - Entering vof const-exp
 - Return 11
 - Return -4
 - Entering call-exp
 - Entering app
 - `[(2, (#num-exp -4))(1, (#const-exp 7), (0, (#proc-val 'x (#diff-exp (#var-exp 'x) (#const-exp 11)) (empty-env))]r` and `[x=2,x=1,f=0]s`
 - Entering diff-exp
 - Entering var-exp
 - Return -4
 - Entering const-exp
 - Return 11
 - Return -15

## Program 9

```racket
(proc (f) (f (f 77))
 proc (x) (x - 11))

```

```racket
(define p9
  (a-program (call-exp (proc-exp 'f (call-exp (var-exp 'f) (call-exp (var-exp 'f) (const-exp 77))))
                       (proc-exp 'x (diff-exp (var-exp 'x) (const-exp 11))))))
```

 - Entering vof call-exp
 - Entering vof proc-exp
 - Return (#proc-val 'f (#call-exp (#var-exp 'f) (#call-exp (#var-exp 'f) (#const-exp 77))))
 - Entering vof proc-exp
 - Return (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11)))
 - Entering app
 - `[(0, (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11))))]r` and `[f=0]s`
 - Entering vof call-exp
 - Entering vof var-exp
 - Return (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11))))
 - Entering vof call-exp
 - Entering vof var-exp
 - Return (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11))))
 - Entering vof const-exp
 - Entering app
 - [(1, (#num-val 77)), (0, (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11))))]r` and `[x=1,f=0]s`
 - Entering diff-exp
 - Entering var-exp
 - Return 77
 - Entering const-exp
 - Return 11
 - Return 77-11 = 66
 - Entering app
 - [(2, (#num-val 66)),(1, (#num-val 77)), (0, (#proc-val 'x (#diff-exp (var-exp 'x) (const-exp 11))))]r` and `[x=2,x=1,f=0]s`
 - Entering diff-exp
 - Entering var-exp
 - Return  66
 - Entering const-exp
 - Return 11
 - Return 55


## Program 10

```racket
let x = 200
in let f = proc(z) (z - x)
   in let x = 100
      in let g = proc (z) (z - x)
         in (f 1) - (g 1)
```

```racket
(define p10
  (a-program (let-exp 'x (const-exp 200)
                      (let-exp 'f (proc-exp 'z (diff-exp (var-exp 'z) (var-exp 'x)))
                               (let-exp 'x (const-exp 100)
                                        (let-exp 'g (proc-exp 'z (diff-exp (var-exp 'z) (var-exp 'x)))
                                                 (diff-exp (call-exp (var-exp 'f) (const-exp 1))
                                                           (call-exp (var-exp 'g) (const-exp 1)))))))))
```

 - Entering vof let-exp
 - Entering vof const-exp
 - Return vof 200
 - `[(0, (#num-val 200))]r` and `[x=0]s`
 - Entering vof let-exp
 - Entering vof proc-exp
 - Return vof `(#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])`
 - `[(1, (#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])),(0, (#num-val 200))]r` and and `[f=1, x=0]s`
 - Entering let-exp
 - Entering vof const-exp
 - Return vof 100
 - `[(2, #num-val 100), (1, (#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])), (0, (#num-val 200))]r` and `[x=2,f=1,x=0]s`
 - Entering vof let-exp
 - Entering vof proc-exp
 - Return vof (#proc-val 'z (#diff-ex (#var-exp 'z') (#var-exp 'x')) [x=1, f=1, x=0])
 - `[(3, (#proc-val 'z (#diff-ex (#var-exp 'z') (#var-exp 'x')) [x=1, x=0])),(2, #num-val 100), (1, (#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])), (0, (#num-val 200))]r` and `[g=3x=2,f=1,x=0]s`
 - Entering diff-exp
 - Entering call-exp
 - Entering var-exp
 - Return vof `(#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])`
 - Entering const-exp
 - Return vof 1
 - Entering app
 - `[(4, (#num-val 1))(3, (#proc-val 'z (#diff-ex (#var-exp 'z') (#var-exp 'x')) [x=1, x=0])),(2, #num-val 100), (1, (#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])), (0, (#num-val 200))]r` and `[z=4,g=3,x=2,f=1,x=0]s`
 - Entering vof diff-exp
 - Entering vof var-exp
 - Return 1
 - Entering vof var-exp
 - Return 100
 - Return -99
 - Entering call-exp
 - Entering var-exp
 - Return (#proc-val 'z (#diff-ex (#var-exp 'z') (#var-exp 'x')) [x=1, x=0])
 - Entering const-exp
 - Return vof 1
 - Entering app
 - `[(5, #num-val 1)(4, (#num-val 1))(3, (#proc-val 'z (#diff-ex (#var-exp 'z') (#var-exp 'x')) [x=1, x=0])),(2, #num-val 100), (1, (#proc-val 'z (#diff-exp (#var-exp 'z) (#var-exp 'x)) [x=0])), (0, (#num-val 200))]r` and `[z=5,z=4,g=3,x=2,f=1,x=0]s`
 - Entering vof diff-exp
 - Entering vof var-exp
 - Return vof 1
 - Entering vof var-exp
 - Return vof 200
 - Return 199
 - Return 99-199
 - Return -100
