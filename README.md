# JOYA Language

JOYA Language is a lightweight scripting language.

## How to run demos

Clone repository, go to directory ```Joya```, open terminal and
run command:

```sh
java -jar joya-lang-1.0.jar demo1.joya
```

## JOYA Scripting Language Concept

JOYA language provides an easy solution for creating user defined functions, that conforms to the semantics of math expressions. JOYA was created in order to write logics inside other programs.

Following statement demonstrates function declaration in JOYA:

```javascript 
	f (x, y) = x * sin (y) + y * cos (x)
	println (f (pi / 2, pi / 3))
```

Any variable can be declared at any place and it's visible globally within the module:

```javascript
	t = pi^2
	println (f (pi, t))
```

Bellow is the program example written in JOYA Language:


```javascript
	x:[0,4600]: 100
	y:[0,4600]: 100
	F:[0,20]: 1
	
	C = 1.6 * 10^-8
	d33 = 2.1 * 10^-11

	j { C > 0 && d33 > 0 } (p, R) = p / 2 + atan (R*C)
	Uout (p, f, w, R) = d33 * f * j (p, R) * w * R / (1 + j (p, R) * w * R * C) 
	
	F @ (
		sleep (100)
		surface (x, y, 20, Uout (x, F, 200, y))
	)
```

![Joya](https://github.com/stowage/Joya/raw/master/joya.gif "JOYA Demo")


## Feature Overview

### Variables

JOYA Variables are all global within one module and defined at compile time. Here is an example of variable declaration:

```javascript
	t1 = 5
	t2 = 10
```
It's also possible to declare numeric variable without the initialization:

```javascript
	var
```
Variable can be checked if it has a value:

```javascript
	var = if ( var == ? , 100)

```
This ```if``` statement checks whether variable has been initialized or not.
Question-mark ```?``` symbol means that variable has unknown or infinite numeric value.
Text variable can't be set to unspecified value while numeric variable can be reset to unspecified value at any time

#### Numeric Variable

Numeric variables have always floating point values or unknown value ```?```. Here are examples of numeric variables declarations:

```javascript
	a = 10
	b = 5.5
	c = ?
	d = a + b
	...
	
```

#### Numeric Range Variables

Range variables are important part of language. They allow to specify loop parameters

```javascript
	x:[0, 100):1
```

Variable ```x``` can change its value in between 0 <= x < 100. Brackets signal the interpreter whether upper or lower bounds should be included or not into a range:

```javascript
	t:(?, ?]:1
	T:[0, ?]:1
	S:(?, 100):1
	x:[5, 20]: 1
	y:(-10, 10): 2
```
Using of round ```)```, ```(``` brackets means that bound is not included within the range, while using of square ```[```, ```]``` brackets means that bound should be inside the range.

 ```?``` symbol specifying in upper or lower bound means that the lower or upper part of the range is infinite.

Last parameter represents step value, which is used to iterate over the variable within the range, 
Here is an example how to apply a range variable to the function:

```javascript
	x:[5, 20]: 1
	y:(-10, 10): 2
	f(x, y) = x^2 + y^2
	x @ y @ println(f(x,y))
```
```@``` operator iterates variables x and y and passes their values to the function f(x,y).

Ranges and steps can be redefined:

```javascript
	x:[10, 50]: 1
```
Step redefinition:
```javascript
	y:: 4
```


#### Text Variables

Text variables can be specified in the following way:

```javascript
	str = 'The quick brown fox jumps over the lazy dog'
	println(str)
	pattern = 'Value of %s is %d'
	println(format(pattern, { str, 5 }))
```
Text variable concatenation example

```javascript
	name = 'John'
	text = concat('Hello, ', name)
	
	message = iftext (equals(name , John), 'Important message to John')
```

#### Array Variables

Array variable can be initialized using braces ```{}```:

```javascript
	arr = {}
	items = {10, 3, 78, 59.5}
	strings = {'One', 'Two', 'Three'}
	matrix = { {1, 0, 0}, { 0, 1, 0 }, { 0, 0, 1} }
```

An array variable should be consistent to the initial type or to the type of the first value added to it:

```javascript
	add (arr, 12.5)
	add (items, 1.2)
	insert (items, 0, 2.3)
	add (strings, 'Four')
	add (matrix[0], 1)
	add (matrix[1], 1)
	add (matrix[2], 1)
	add (matrix, {0, 0, 0})
	
```

To access a variable in array use following expression:

```javascript
	println(arr[0])
	println(arr[size (arr) - 1])
	arr[0] = 5.5
	matrix[0][2] = 0
```

## Functions

### Internal Functions

JOYA has internal set of standard functions like sin(x), cos(x), floor(x), ceil(x) and etc. ```If``` statement is also a function:

```javascript
	k = 10
	x:[1,5]:1 @ 
		if (k == 0, 
				k = k + 1, 
				k = k - 1 )
	println (k)
```

or

```javascript
	k = 10
	x:[1,5]:1 @ 
		k = if (k == 0, 
					k + 1, 
					k - 1 )
	println (k)
```

It is possible to perform multiple operations in a function, for example:

```javascript
	y = 2
	x = abs( m2 = y * y * y; -m2 / y)
```
only last result of an expression delimited by the semi-colon symbol will be applied to a function. So we can easily perform multiple operation in one function call even if it has multiple parameters:

```javascript
	b = 0
	k = 10
	x:[1,50]:1 @ 
		k = if (k == 0, 
			k + 1, 
			b = b + 2; k - 1 )
	println (b)
```
or

```javascript
	b = 0
	c = 0
	k = 10
	x:[1,50]:1 @ k = if (k == 0, 
		c =  c + 3; k + 1, 
		b = b + 2; k - 1
	)
	println (c)
```

### If Statement

```if``` statement is a function of three arguments:

```javascript
	[ <variable> = ] if (<condition>, <then> [ , <otherwise> ])
```
to perform multiple operations in ```<then>``` or ```<otherwise>``` statement operations can be split by semi-colons or line breaks. 
Last expression should not contain semi-colon sign:

```javascript
	if (i < 0,
			s = s + f(i)
			println (s)
		)
	if (i < 0,
			s = s + f(i);
			println (s)
		)
```


```if``` statement can return a value because of its functional behaviour, f.e.:

```javascript
	m = 1
	s = 3
	i = -10
	m = m + if (i < 0,
					s = s + f(i)
					println (s)
					s
					,
					s = s - f(i)
					println (s)
					s
			)
```

last expression in a block is treated as a return statement.


### User Defined Functions

#### Simple Function

Simple functions can be created in the following way:

```javascript
	f (x) = x^2 + x - 1
	U (t) = t/2 + sin (t)
	F (x, y) = x^2 + y^2
```
Or with multiple lines:

```javascript
	f (a, b, t) = {
		sum = 0
		y:[a,b]:1 @ sum = sum + y^t
	}
	println (f (0, 10, 2))
```

Last result in last expression of a function body will return function result:

```javascript
	f (a, b, t) = {
		sum = 0
		y:[a,b]:1 @ sum = sum + y^t
		if ( sum > 100, sum, y)
	}
	println (f (0, 10, 1))
	println (f (0, 10, 2))
	
```

output will be:

```
10.0
385.0
```

#### Abstract Functions


```javascript
	f (a, b, fn (x)) = {
		sum = 0
		y:[a,b]:1 @ sum = sum + fn(y)
	}
	println (f (0, 10, fn (x) = x^2 + x * 2))
	println (f (0, 10, fn (x) = x * 2      ))
```

in this example function ```f``` has a parameter ```fn(x)``` which is a reference to an abstract function

#### Conditional Function

Conditional function executes only when it meets specified criteria, f.e.:

```javascript
	f {x == 5} () = {
		println ('x == 5')
	}
	fn {x == 1} () = {
		println ('x == 1')
	}
	x = 1
	f()
	fn()
	x = 5
	f()
	fn()
```
function ```f``` will be called as long as variable x will be equal to 5 and 
function ```fn``` will be called if x is equal 1, so the output will be:

```
	x == 1
	x == 5
```
or more complex example:

```javascript
calculate { abs(a - b) > eps } (fn (arg)) = { 
	c = (a + b) / 2
   if ( fn (c) * fn (a)  <= 0, b = c, a = c )
	root = c
}
bisection (x, y, function (arg)) = { 
	eps = 0.001
	a = x
	b = y
	root = ?
	t:[?, ?] @ if (calculate (fn(arg) = function(arg)) == ?, ?)
	root
}

println (bisection (-5, 5, function (arg) = arg^2 - 4))
```

It's also possible to define condition in statement where function is called:

```javascript
	f {x == 5} () = {
		println ('x == 5')
	}
	fn {x == 1} () = {
		println ('x == 1')
	}
	x = 1
	f {x == 1} ()
	fn()
	x = 5
	f {x == 1} ()
	fn()
```
outputs:

```
	x == 5	
	x == 1
```

#### Progression

Progression can be defined as follows:

```javascript
	z (n) = z (n)^2 + C
```
First value initialization:

```javascript
	z (0) = 0
```

When performing a function call, interpreter records the value after the first call and adjusts value on all future calls, f.e.:

```javascript
  f (x) = f(x) + x
  f (0) = 0
  println( f (1) )
  println( f (2) )
  println( f (3) )
  
```

will print out:

```
	1.0
	3.0
	6.0
```


#### Recursive Operations

To call function recursively wrap user defined function call with```recursive``` function.

```javascript
	traverse (x) = {
		if ( x < 5 ,
			x = x + 1
			println(x)
			traverse(x)
		)
	}

	println('Recursive')
	recursive (traverse (0))

	println('Progression')
	traverse(0)
	traverse(1)
	traverse(2)
```

Code above will produce the following output:

```
	Recursive
	1.0
	2.0
	3.0
	4.0
	5.0
	Progression
	1.0
	2.0
	3.0
```

#### Functions Variables Livecycle

Variables declared inside the function are visible globally. However variables defined as an arguments restore their values after a function call.
F.e.:

```javascript
	x = 5
	f(x) = {
		y = 10
		x = x^2
	}
	println (f(x))
	println (x)
	println (y)
 
```
will output:

```
	25.0
	5.0
	10.0
```

this is not related to array variables, so f.e.:

```javascript
x = {5, 8, 7, 1, 4}
y = {3, 1, 9, 7}
f(x, length) = { 
	sum = 0
	i:[0, length): 1 @ {
		sum = sum + x[i]
	}	
	i:[0, length): 1 @ {
		x[i] = x[i] / sum
	}
}

f(x, size(x))

println(x[0])

f(y, size(y))

println(y[0])
```

### Loop Statement

In order to create a loop you need a range variable. Loop breaks only if last statement returns unknow or infinite value:

```javascript
bisection (x, y, function (arg)) = { 
	eps = 0.001
	a = x
	b = y
	root = ?
	t:[?, ?] @ {
		if ( abs(a - b) > eps ,
		 	c = (a + b) / 2
    	 		if ( function (c) * function (a)  <= 0, b = c, a = c )
			    root = c,
    	 		?
    	 	)
	}
	root
}

println (bisection (1, 20, function (arg) = arg^2 - 4))
```
If symbol ```?``` is assigned to last statement, loop interrupts

```javascript
f() = {
	i = 0;
	t:[?,?]:1 @ {
		i = i + 1
		if (i > 10, ?)
	}
}

f()

println(i)
```

It's also possible to change the range at the runtime

```javascript
BubbleSort (number[]) = {
	length = size (number)
 	i:[0, length):1 @ 
 		j:[i + 1, length):1 @ {
                if (number[i] > number[j],
                    a =  number[i]
                    number[i] = number[j]
                    number[j] = a 
                )
    }
    i:[0,length):1 @ println (number[i])
}

n = {2,20,3,23,42,34,53,45,3,45,
34,5,34,5,3,45,3,45,3,45,6,76,87,
8,678,6,78,6,6,8,7,8,67,34,23,42,
34,235,4,5,34,5,43,45,3,45}
BubbleSort (n)

```

### JOYA Modules

JOYA Modules are simple files. Each module is stored in a file with extension ```*.joya```.
Functions or variables sharing can be done by switching context between modules.
For example there is a file named ```CustomModule.joya``` containing expressions bellow:

```javascript
	d33 = 2.1 * 10^-11
	S = 27*10^-3
	amp (t, w, f0, n) = -2 * f0 * t * cos(w * t) / (pi*w*n) 
	F (L,A) = pi * pi * h * c * A / (240 * L^4) 
	j (p, R) = p / 2 + atan(R*C)
	Uout (p, f, w, R) = d33 * f * j(p, R) * w * R / (1 + j(p, R) * w * R * C) 
	Uf (Fo) = Fo * d33 / C
	Ff (Uo) = Uo * C / d33
	Lf (Fo, A) = (pi*pi * h * c * A / (240 * Fo))^0.25
	W (p, f, R) = Uo / (j(p, R) * R * (d33 * f - Uo * C))
		
```

To use specific variable or a function in current module use following statement:

```javascript
	number = 20
	
	time:[1, 10]:1 @
		$CustomModule (time, number) = `
			println (amp (time, 200, 103, number))
			println (amp (5, 200, 103, number))
		`
	
```

```$``` symbol works as an address space switch.

Pass variables or functions which need to be shared between modules via arguments:

```javascript
	$CustomModule (var1, var2,...) ``
```
It's also possible to read results from an external module by assigning shared variable:

```javascript
	d33 = 390 * 10^-12
	C = 7 * 10^-8
	$CustomModule (result) `
		d33 = 180 * 10^-12;
		result = d33 * 10^12;
	`
	println (result)
```

#### Conditional Module

Modules may have a behavior similar to conditional functions,
f.e. there is a module named ```bisection.joya```

```javascript
	bisection (x, y, function (arg)) = { 
		eps = 0.001
		a = x
		b = y
		root = ?
		t:[?, ?] @ {
			if ( abs(a - b) > eps ,
		 		c = (a + b) / 2
    	 		if ( function (c) * function (a)  <= 0, b = c, a = c )
			    root = c,
    	 		?
    	 	)
		}
		root
	}
```

conditional module call will look as follows:

```javascript
	from = -5
	to = 5
	$bisection {from < to} (from, to, ret) = `
		ret = bisection (from, to, function (arg) = arg^2 - 4)
	`
	println(ret)

	from = 25
	to = 5
	$bisection {from < to} (from, to, ret, fn1(s) = s^2 - 4) = `
		ret = bisection (from, to, function (arg) = fn1(arg))
	`
	println(ret)

```

the output will be:

```
-2.0001220703125
-2.0001220703125
```

Copyright (c) 2018 Sergey Fedoseev



 

