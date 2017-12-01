# Clojure Style Guide

## Summary

Observe but one command, and also one teaching:

**Know thine audience.** Terms such as "obvious" and "confusing" are subjective, but far from being thereby meaningless or useless, the source of the subjectivity is instructive. The terms become clearer given an assumed *shared perspective*: so identify the common perspective of your audience and write to that. If every other member of your team can be expected to understand a certain concept, whether that be optimistic concurrency, idempotency, or something else, then it is unnecessary in your documentation to describe the concept in detail rather than simply applying the correct label and moving on. If every other member of your team can be expected to be familiar with certain stylistic idioms in your language community, then reserve your code comments for the places where you depart from common idiom. In short, good style is a balancing act, wherein you do your best not to waste your time or your colleagues'. Don't underdocument and don't write line noise; don't overdocument, or simplify your code to a tedium.

**The Style Guide was made for humans, not humans for the Style Guide.** Or, in the words of modern sagery, "They're more like guidelines anyway." Yet again, "Break any of these rules sooner than say anything outright barbarous" (George Orwell). Don't get hung up on these; apply the rules with expedience and prudence.

## General Principles

**1. Follow community idioms.** The Clojure community has developed idioms of its own. We cover many in this document, overlapping somewhat with [this summary](https://github.com/bbatsov/clojure-style-guide). It's generally a good idea to abide by them, unless you have good reason not to. In that case, don't!

**2. Observe short-term memory constraints.** Our short-term memories can hold roughly three to six things at once. This is of particular interest in linguistics and neuroscience: there is a correlation between native language structural complexity and short-term memory capacity. In our case, this is of interest because Clojure is a LISP, and LISPs are infamous for deeply-nested clauses, the source of that common acronym expansion: "Lots of Irittating, Stupid Parentheses!" Generally, our pitiful short-term memories can explain many of our specific rules concerning indentation, code structure, or clause ordering.

Clojure's built-in arrow macros are a useful tool for dealing with the most common cases of heavy indentation:

```clojure
;; (x+2)^2 - 1 = 0
(= (- (Math/pow (+ x 2)
                2)
      1)
   0)
```

This can be vastly improved with [`->`](https://clojuredocs.org/clojure.core/-%3E):

```clojure
(-> (+ x 2)
    (Math/pow 2)
    (- 1)
    (= 0))
```

In other cases, the solution may simply be to restructure your code slightly, as in the following nonsense example:

```clojure
(if (even? a)
  (if (even? b)
    (let [c (Math/abs (- a b))]
      (/ c 2))
    (throw (IllegalStateException. "b is not even")))
  (throw (IllegalStateException. "a is not even")))
```

This is rather difficult to read, as it forces you to keep the first line in your mind all the way through to the last line, tracking the entering and exiting of multiple scopes. Restructuring a little, you get the following equivalent code that we can naturally read more easily:

```clojure
(if (odd? a) ;; or (not (even? a)), more generally
  (throw (IllegalStateException. "a is not even"))
  (if (odd? b)
    (throw (IllegalStateException. "b is not even"))
    (let [c (Math/abs (- a b))]
      (/ c 2))))
```

**3. Favor descriptive names.** Avoid abbreviations, unless they are standardized somehow. Especially avoid `x` or nonsense names unless truly to refer to Things of Indeterminate or Irrelevant Type. Avoid using the same name over and over within the same scope, especially if there is no relationship between the distinct assignations. Prefer `overly-long-but-clear-appellation` to the utterly confusing acronym `olbca`, though one of course prefers `enormous-clear-name` to `overly-long-but-clear-appellation` when possible. As a compromise, a comment can be added to explain an abbreviated name.

**4. Document your code.** Use Clojure's docstrings to document whole functions, macros, or namespaces. When documenting functions, ensure that these three at least are explained: purpose, guarantees, and expectations. Use comments to distinguish "sections" of a file, to clarify symbol names or unusual code structure, to warn future maintainers, etc. Not *all* functions need be documented, but *most* should. A function such as `(defn double [integer] ...)` probably doesn't need one. If a function and its argument signature are well-named, then there may be very little remaining to document: err on the side of clarity, but do avoid unnecessary verbosity.

**5. Be precise.** Anticipate questions concerning and misapprehension or misapplication of your code, and do what you can to head off ambiguity.

**6. But be concise.** Wherever possible without sacrificing clarity or utility, be concise.

**7. Be respectful.** The raison d'etre for all of this. You can do whatever you want in your own projects. This author habitually uses Unicode symbols in his private projects. But at work, other people have to read and work with your code. Be respectful of their time and attention.

## Community Idioms

### Syntax & Formatting

#### Indentation

* Use spaces for indentation.

* When using [macros](https://clojure.org/reference/macros) or [special forms](https://clojure.org/reference/special_forms) that provide for code bodies (variadic-length forms evaluated in an "implicit `do`"), indent the bodies by 2 spaces:

```clojure
(let [x 3]
  (* x 2))
```

* When a line break interposes between a function and its first argument, indent that argument (and all following) by one space:

```clojure
(+
 x
 3)
```

* When a function and its first argument occur on the same line, then all further arguments must occur on that same line, or else each successive argument must occur on a new line, indented to align with the first argument:

```clojure
(apply foo arg-1 arg-2 arg-3)

;; OR:

(apply foo
       arg-1
       arg-2
       arg-3)
```

* An optional exception to the previous rule occurs when the arguments passed to a function are logically paired (e.g., keys and values). In that case, you follow the same rules as above, but may choose to treat each pair as though it were a single item:

```clojure
(hash-map :key-1 :val-1, :key-2 :val-2, :key-3 :val-3)

;; OR:

(hash-map :key-1 :val-1,
          :key-2 :val-2,
	  :key-3 :val-3)
```

* When writing a succession of logically paired (or tupled) values, use commas (they are [treated as whitespace](https://clojure.org/reference/reader)) to separate the tuples:

```clojure
(triple-store apple dog blue, banana horse yellow, orange cat orange)

(hash-map k1 v1, k2 v2, k3 v3)
```

* Never insert whitespace between a symbol and a succeeding closing bracket/parenthesis. Never insert whitespace between an opening bracket/parenthesis and a succeeding symbol. Never insert whitespace between two opening brackets/parentheses or between two closing brackets/parentheses:

```clojure
;; BAD:
(+ 8 4 )
( (partial + 3) 4)
[ 0 1 2 ]

;; GOOD:
(+ 8 4)
((partial + 3) 4)
[0 1 2]
```

#### Line Breaks

* Function definitions may be written on a single line if short enough:

```clojure
(defn sum [& args] (reduce + args))
```

* If the body of a function spans only a single line, or if the body of a function consists of a single form that *does not* provide scoped bindings (e.g., `let`, `binding`, `for`), then the function name and argument vector may occur on the same line:

```clojure
;; FINE:
(defn sum [& args]
  (reduce + args))

;; FINE:
(defn n-fibs [n]
  (->> [0 1]
       (iterate (fn [[a b]] [b (+ a b)]))
       (map first)
       (take n)))

;; NOT FINE:
(defn n-fibs [n]
  (let [lazy-seq (iterate
                  (fn [[a b]] [b (+ a b)])
		  [0 1])]
    (->> lazy-seq (map first) (take n))))
```

* For functions of one arity, put a line break between the argument vector and the body, unless the entire definition is on a single line:

```clojure
;; BAD:
(defn sum
  [& args] (reduce + args))
```

* For functions of multiple arities, put a line break between an argument vector and its corresponding body, unless the body fits on a single line:

```clojure
;; FINE:
(defn sum
  ([x] x)
  ([x & args] (reduce + x args)))

;; FINE:
(defn sum
  ([x] x)
  ([x & args]
    (reduce +
            x
	    args)))

;; BAD:
(defn sum
  ([x] x)
  ([x & args] (reduce +
                      x
		      args)))
```

* If a function definition has multiple arities, the function name must always be followed by a line break:

```clojure
(defn fibs
  ([] (map first (iterate (fn [[a b]] [b (+ a b)]) [0 1])))
  ([n] (take n (fibs))))
```

* Never insert line breaks between consecutive brackets/parentheses of the same type (open/close):

```clojure
;; TERRIBLE:
(inc (* x (get-multiplier account-id)
     )
)
```

If you believe that such line breaks and indentation may be necessary in order to be able to read the code block in question, instead consider restructuring (e.g., factoring out a function) instead:

```clojure
(defn foo [item data]
  (update-in item
             [(cond (centric? data) :centric-data,
	            (extensional? data) :extension-data,
		    (syllogistic? data) :logic-data)
	      (label data)
	      (decoration data)
	      (signature data)]
             (fn [old]
               (if-not old
	         (extract-terms data)
	         (into old (extract-terms data))))))

;; Use `let` and/or factored-out functions to simplify nesting:

(defn- appropriate-key
  [data]
  (cond (centric? data) :centric-data
        (extensional? data) :extension-data
        (syllogistic? data) :logic-data))

(defn foo
  [item data]
  (let [updater (fn [old]
                  (if-not old
                    (extract-terms data)
                    (into old (extract-terms data))))
        compound-key [(appropriate-key data)
	              (label data)
		      (decoration data)
		      (signature data)]]
    (update-in item compound-key updater)))
```

* Insert line breaks between top-level forms:

```clojure
;; BAD:
(def x 3) (def y 4) (def sum (+ x y))
```

* Consecutive single-line top-level forms should have exactly 0 or 1 blank lines between them. 0 if they are related; 1 if not:

```clojure
(def PI 3.14159)
(def e 2.71828)

(def max-attempts 3)
```

* If a top-level form spans more than one line, it should have at least 1 blank line separating it from the following top-level form:

```clojure
;; Three logically-related library wrappers:
(defn foo [x y] (lib/foo x y))
(defn bar [x y]
  (-> (lib/bar x y)
      (post-process-bar))))

(defn blah [x y] (lib/blah x y))
```

* Top-level forms should never be separated by more than 2 blank lines, and 2 blank lines should be reserved for delineating sections of a file.

### Defining vars

* Mark vars as private wherever appropriate:

```clojure
(def ^:private private-constant 10)

(defn- private-function [& args] ...)

(defn ^:private private-function [& args] ...)
```

* Avoid defining vars outside of top-level forms:

```clojure
;; BAD:
(defn init-my-global [value]
  (def my-global value))

;; BETTER:
(def my-global) ;; nil
(defn init-my-global [value]
  (alter-var-root! #'my-global (constantly value)))
```

* Prefer `defn` over `def` + `fn`:

```clojure
;; ANNOYING:
(def foo (fn [x] (inc x)))

;; FINE:
(defn foo [x] (inc x))
```

### Naming

* Even when defining functions in-line, consider using named functions over anonymous ones:

When dealing with many programmatically generated functions, this can turn out to be very helpful for examining stacktraces or using a profiler.

```clojure
;; Anonymous:
(fn [x] ...)

;; Named:
(fn funky-name [x] ...)
```

* Macros
* defmethod
  "Having defmethods strewn around several files contributes far more to unclarity than macros ever have, but that's a different diatribe." -- (The Common Lisp Cookbook)

### Naming
* lisp-case and CamelCase
* abbreviations only common ones in your domain
* Namespaces
* Functions (side effects)
* Dynamic Vars

### Literals

* Collections
* Functions
* Regexes

## Documentation
* Document everything (only excuse: it's already utterly obvious *to your audience*)
* The three/four big concerns:
  * intent (helps reviewers & maintainers -- what if code begins to evolve away from design?)
  * expectations (arguments, appropriate scope & use patterns, etc.)
  * guarantees (return value, side effects, etc.)
  * (when appropriate) call out risks
* ALWAYS DOCUMENT SIDE-EFFECTS (e.g., idempotent?)
* English style: mainly, be consistent
  * Clojure docs as baseline
* optional for args: `- argN (type hint) explanation`
