Spoofax Core Pretty Print fails to add parenthesis
==================================================

Consider the program:

```
(1 + 2) * 3
```

This can be parsed with SDF3 by:

```
module parens-test

imports
  Common

context-free start-symbols

  Expr

context-free syntax

  Expr = <(<Expr>)> {bracket}

  Expr.Plus = <<Expr> + <Expr>> {left}

  Expr.Mult = <<Expr> * <Expr>> {left}

  Expr.Num = INT

context-free priorities
  { left: Expr.Mult } >
  { left: Expr.Plus }
```

The AST will look as follows, which is expected:

```
Mult(Plus(Num("1"), Num("2")), Num("2"))
```

However when running the `pp-debug` rule on this AST, the result is:

```
1 + 2 * 3
```

Which has *not* same meaning as the original expression.
