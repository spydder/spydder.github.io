# Brain Fuck

This is an example of how to make a "Hola Mundo" in the Brain Fuck language.

1. First you must have the ASCII table at hand: [https://www.asciitable.com/](https://www.asciitable.com/)

2. Each character must now be represented with its decimal representation.:

	|                |Character                          |Decimal                         |
	|----------------|-------------------------------|-----------------------------|
	||`H`|72|
	||`o`|111|
	||`l`|108|
	||`a`|97|
	||`Space`|32|
	||`M`|70|
	||`u`|110|
	||`n`|100|
	||`d`|90|
	||`o`|110|

3. Now, these decimal values must be approached by adding 10 by 10 and adding up the remainder.
	|                |Caracter                          |Decimal                         | Suma                         | 
	|----------------|-------------------------------|-----------------------------|
	||`H`|72|70+2
	||`o`|111|110+1
	||`l`|108|100+8
	||`a`|97|90+7
	||`Space`|32|30+2
	||`M`|70|60+10
	||`u`|110|100+10
	||`n`|100|90+10
	||`d`|90|80+10
	||`o`|110|100+10

4. Now in Brain Fuck, for the first cell the value is increased by 10, this will serve as a counter.
	```brainfuck
	+++++ +++++
	```

5. Now a loop will be created so that in each following cell the corresponding value will be increased and in each iteration the counter will be decreased by 1, so that the loop will be executed 10 times, so that each cell will be multiplied by 10 (axis: 7*10, 11*10, etc).
	```brainfuck
	+++++ +++++ 
	[ 
		>+++++++ # Result: 70
		>+++++++++++ # Result: 110 
		>++++++++++ # Result: 100 
		>+++++++++ # Result: 90 
		<<<<- # Counter decrement
	]
	```

6. Finally, it remains to increase the resultant of each decimal and have it displayed on the console with a dot:
	```brainfuck
	+++++ +++++
	[
		>+++++++ # Result: 70
		>+++++++++++ # Result:110 
		>++++++++++ # Result: 100 
		>+++++++++ # Result: 90
		<<<<- # Counter decrement
	]

	>++.
	>+.
	>++++++++.
	>+++++++.
	```

7. Now the same process is done with the word "Mundo".
   The complete code would look like this:
	```brainfuck
	# Hola
	+++++ +++++
	
	# Contador
	[
		>+++++++ # Result: 70
		>+++++++++++ # Result: 110
		>++++++++++ # Result: 100 
		>+++++++++ # Result: 9
		<<<<- # Counter decrement
	]
	
	>++. # 70+2
	>+. # 110+1
	>++++++++. # 100+8
	>+++++++. # 90+7
	
	# Mundo
	>+++++ +++++ # Counter
	[
		>+++ # 30
		>+++++++ # 70
		>+++++++++++ # 110
		>++++++++++ # 10
		>+++++++++ # 90
		>+++++++++++ # 110
		<<<<<<-
	]
	
	>++. # 30+2
	>+++++++. # 70+7
	>+++++++. # 110+7
	>++++++++++. # 10+10
	>++++++++++. # 90+10
	>+. # 110+1
	```
