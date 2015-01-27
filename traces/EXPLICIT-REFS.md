# IMPLICIT-REFS

```racket
let a = newref(newref(newref(0))) in
      let f = proc(l)
          begin
            setref(deref(deref(l)),l);
            deref(l)
          end in
      deref((fa))
```

```racket
(define ex
  (a-program
   (let-exp 'a
            (newref-exp (newref-exp (newref-exp (const-exp 0))))
            (let-exp 'f
                     (proc-exp 'l
                               (begin-exp (setref-exp (deref-exp (deref-exp (var-exp 'l)))
                                                      (const-exp 1))
                                          (list (deref-exp (var-exp 'l)))))
                     (deref-exp (call-exp (var-exp 'f)
                                          (var-exp 'a)))))))
```

 - Evaluating let-exp
 - Evaluating newref-exp
 - Evaluating newref-exp
 - Evaluating const-exp
 - Return 0
 - [(0,0)]
 - Return 1
 - [(1,0),(0,0)]
 - Return 2
 - [(2,1),(1,0),(0,0)]
 - Evaluating let-exp
 - Evaluating proc-exp
 - Return (proc-val 'l (begin-exp (setref-exp (deref-exp (deref-exp (var-exp 'l))) (const-exp 1)) (list (deref-exp (var-exp 'l)))))
 - Evaluating deref-exp
 - Evaluating call-exp
 - Evaluating var-exp
 - Return (proc-val 'l (begin-exp (setref-exp (deref-exp (deref-exp (var-exp 'l))) (const-exp 1)) (list (deref-exp (var-exp 'l)))))
 - Evaluating var-exp
 - Return 2
 - Apply procedure
 - Evaluating begin-exp
 - Evaluating setref-exp
 - Evaluating deref-exp
 - Evaluating deref-exp
 - Evaluating var-exp
 - Return 2
 - Return 1
 - Return 0
 - Evaluating const-exp
 - Return 1
 - Evaluating deref-exp
 - Evaluating var-exp
 - Evaluating 2
 - Return 1
