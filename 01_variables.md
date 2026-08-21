# 1. Variables in JavaScript

the main data decration techniques are var, let and const

1. const 
const is a constant variable decration means it can not be changed in the code

example
const a = 10;
    // a = 20;
    // console.log(a);    // output: Uncaught TypeError: Assignment to constant variable.

2. let
let is the variable declatration technique that can be change later in the code

example
let a = 10;
a = 20;
    // console.log(a);    // output: 20

3. var
var is the old technique of variable declatration that can be change later in the code but it is not block scoped so dont use var its is a old technique of variable decration

example
var a = 10;
a = 20;
    // console.log(a);    // output: 20


4. we can also declare a vailabe without using any declaration techniqe


example
a = 10;
    // console.log(a);    // output: 10




tip: both let and const are block scoped but var is not block scoped so dont use var its is a old technique of variable decration


# 2. Data Types in JavaScript

