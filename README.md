# intro
Want to brush up on C++ basic and get ready to code? Read my summary of C++ basic. It covers all the basic concepts of c++.

# C++ basics in 1 hour

### some concepts

####main
The main function must return int:

```
int main() {}
int main(int argc, char* argv[]) {}
int main(int argc, char* argv[], /*other parameters*/) {}
can have return 0 or no return statement (default to 0)

```
`echo $?` will show you what was returned

#### compile
```
g++ -std=c++14 program.cc -o program
```
If no `-o program`, it would be `a.out`

#### I/O
*3 I/O streams*
- cout: printing to stdout
- cin: reading from stdin
- cerr: printing to stderr

*Operation*
<< : B put to A	cout << x
>> : B get from	A	cin >> x
Can be a chain

Note: 
cin ingores whitespaces (space/tab/newline)
`>>` returns cin, so can be in a chain `cin >> x >> y`

- if the read fails: `cin.fail()` will be true
```
cin >> i;
if (cin.fail()) break;
```
- if EOF: then `cin.eof()` and `cin.fail()` are true but not until the attempted read fails
- `cin` can be used as a bool: `if(!cin) break`
- many verions from `io/readInts`
- In C, `>>` can be right shift: `0b010101 >> 3` ->`0b010` right bit shift operation; but when left hand side is cin, it is "get from
- cin >> x returns cin back (type istream)

```
//read all ints and echo back to stout until EOF, skip non-integer input
int i;
while(true) {
	if(!(cin >> i)) {
		if(cin.eof()) break;
		cin.clear(); //clear the fail bit
		cin.ignore(); //ignore the current input
		//to skip invalid value
	} else {
		cout<<i<<endl;
	}
}
```
If I want cin whitespace:

```
getline(cin, s); //reads to the next newline character into s
```

#### inc/dec
++i/--i: increment/decrement i by 1, then fetch the value of i 
i++/i--: feth the value of i, then increment/decrement the value of i by 1

#### size
see intro/size.cc

```
bool: 8 bits
char: 8 bits
short: 16 bits
int: 32 bits
long: 64 bits
long long: 64 bits

float: 32 bits
double: 64 bits
long double: 128 bits

unsigned char: 8 bits
unsigned short: 16 bits
unsigned int: 32 bits
unsigned long: 64 bits
unsigned long long: 64 bits
```


#### hex and dec manipulators
```
<iomanip>
setw: specifies the minimum spaces for the next numeric or string value

cout << i << j << endl; //5109
cout << setw(4) << i << setw(5) << j << endl; // ---5---109


cout << 95 << endl;
cout << hex << 95 << endl; //5F, and will affect future values until you change
```

hex: cause all subsequent integer to be printed in hexadecimal   
To switch back to decimal: cout << dec


#### array
can:

```
cin >> size;
int *arr = new int[size];
```

#### strings in c++
Strings grow as needed  
it is easier to manipulate strings s = "hello"     
Even in C++, the literal "hello" is a C-style string: a null-terminated array of characters
`include <string>`

String operations:

- == != 
- < <= > >=

```
"a" < "b"
"a" < "ab"
"A" < "a"             (Since A has ASCII value 65; a has a higher ASCII value)
"cat" < "caterpillar"
```

- s.length() returns the length of s
- fetch individual chars: s[0], s[1], ... indexed access using [] or at()
- Concatenations: `s3-s1+s2;`  `s3+=s4`

```
//initialize
string s0 ("Initial string");
string s0 {"hey"};
string s1;
string s2 (s0);
string s3 (s0, 8, 3);
```

You can see the library or course note

*string to int*

```
int n;
string s;
n = stoi(s);
```

*substring*

```
std::string str="We think in generalities, but we live in details.";
                                           // (quoting Alfred N. Whitehead)

  std::string str2 = str.substr (3,5);     // "think"

  std::size_t pos = str.find("live");      // position of "live" in str

  std::string str3 = str.substr (pos);     // get from "live" to the end
```

*eliminate*  

```
str.erase(str.end()-1);  
str.erase(begin, len);

//clear: erase the content of the string --> become an empty string

```

*cin >> s*
Read a string, skip leading spaces, stop at next whitespace; read one word

*getline(cin, s)*
Read to the next newline character into s

*to read/write from/to a file* 
- ifstream/ofstream
- you need to include the <fstream>
- c_str: to access a C-style version (const char *1) of the string

```
//read from a file
#include <iostream>
#include <fstream>
usign namespace std;

int main() {
	ifstream file {"suite.txt"}; //declare ifstream and open the file
	//void printSuiteFile(string name = "suite.txt") {
  	//ifstream file(name.c_str()); } //old way
  	
  	//or:
  	ifstream file{name}; // C++11 way
  
	string s;
	while (file >> s) {
		count << s << endl;
	}
} // file is closed when it goes out of scope


//write out to a file:
	ofstream outfile;
   outfile.open("afile.dat");
   ...
   outfile << data << endl;
   
//check if fail in reading from file
ifstream ifs{"file.txt"}; //also accept c++11 + string
string str;
ifs >> str;
int i;
ifs >> i; //can fail
while(ifs.fail()) {
	ifs.clear();
	ifs.ignore();
	ifs >> i; //abc d12 --> check d, 12
}
```

*sstream*
ostringstream: String stream that writes to become a string  
include <sstream>  
istringstream: String stream that reads from a string  
 
Use it to build potential many-steps string

```
ostringstream ss;
ss << "some" << "thing" << "one";
string s = ss.str();
cout << s << endl;
```

To see istringstream(getNum.cc):  
Why we need it? 

```
//Why 1: to get intermediate value from cin, and consider 
int main () {
  int n;
  while (true) {
    cout << "Enter a number:" << endl;
    string s;
    cin >> s; //note: input 123 can be treated as an int or string, see how you want to place it
    istringstream ss{s};
    if (ss >> n) break; //read one string/integer at a time
    cout << "I said, ";
  }
  cout << "You entered " << n << endl;
}


//Example: continue to read number until you input non-number
int main () {
  string s;
  while (cin >> s) {
    istringstream ss{s};
    int n;
    if (ss >> n) cout << n << endl;
  }
}


//Why 2: convert string to int (a+ib)
//same for ostringstream
string complexNumberMult(string comp_a, string comp_b){
    int real_a, im_a, real_b, im_b;
    char buffer;
    istringstream stream_a(comp_a), stream_b(comp_b);
    ostringstream ans;
    stream_a >> real_a;
    stream_a >> buffer; // buffer is '+'
    stream_a >> buffer; // buffer is 'i'
    stream_a >> im_a;
    stream_b >> real_b;
    stream_b >> buffer; // buffer is '+'
    stream_b >> buffer; // buffer is 'i'
    stream_b >> im_b;
    ans << real_a * real_b - im_a * im_b; // putting in the real part
    ans << "+i"; // putting in "+i"
    ans << real_a * im_b + real_b * im_a; // putting in the imaginary part
    return ans.str();
}

//Why 2 another example:
istringsteram iss{"4 + 3"};
int l, r;
string op;
iss >> l >> op >> r;
iss.str(); //string representation of the stream (c style)

```

Side:  
can use eof fail clear ignore like cin



#### Declare the function at the top

`bool odd(unsigned int n);` at the top
You should declare before you use;  
Can not have void f(int a; int b=1) {...}   void f(int a){...} 
f(5) // invalid   
An entity can be declared several times but defined only once


####Pointers in c++

```
int a[] = {0,1,2};
int a[5] = {1,2,3};
int a[5];
a[0]	*a
a[1]	*(a+1)


int foo = 1000;
int *p = &foo;

int **p = &p;//a pointer pointing to a pointer
```


#### Constants
```c++
const int n = 100;
const int *p = &n; // p is a pointer to an int which is const
//const int must mach with type of n, but note that p can point to a non-const int var
const int m = 20;
p = &m; //fine
*p = 30; //no

int * const p = &n; // p is a const pointing to an int
p = &m; //no
*p = 100; //fine

const int * const p = &n; //p is a const pointer pointing to a const int
p = &m; //no
*p = 100; //no

Node n {5, nullptr};
const Node n2 = n; // a copy of n to n2, n2 is const

//const needs init value when created

```

####nullptr
We use `nullptr` in c++


#### pass by value / pass by pointer

#### pass by reference

- when pass: make a copy of the address
- changes to the variable persist
- faster than passing-by-value, since you don't have to copy the params
- Pass-by-const-reference:

```
int foo(int &x, const int &y) { ... }
int main() {
int a = 42;
foo(a, a);
foo(a, 43);
foo(43, a); // invalid, what does it mean to change a literal?
foo(43, 43); // as above
}
```

#### reference
C++ has another pointer like : reference, similar to pointer
  
```
int y = 10;
int &z = y; //z "point to" y

//reference cannot point to nullptr
//z is like const pointer: z cannot point to a different variable
//z is an alias to y; z and y has the same address
// pass y into a func like `void inc(int &n)`, y value can be changed; so inc(5) will throw an error, 5 is an rvalue
cin >> x; //here x passed by reference

z = 100;
// now y is 100; z is alias to y

// z is lvalue reference, which means:
int &x = 3; //no, 3 is not lvalue
int &x = y+z; // y+z is an expression, no
int &*p; //no pointer to a reference
int *&z; // yes, reference to pointer
int &&r; //right but DO NOT USE
int & r[3] = {n,n,n}; //no

```

```
int f(int &n) {...};
int g(const int &n) {...};
f(5); //not valid
g(5); //valid in c++, ok to pass rvalue here
```



### Dynamic Memory
- use the c++ syntax (we will not use `alloc` and `free`)  

```
struct Node{
	int data;
	Node *next;
}
Node * np = new Node; //np is a look-up variable, on stack
//the "new Node" thing is allocated on the heap, will need to be freed by yourself
delete np;
int *x = new int{5}; // one element
...
delete x;

Node *myNodes = new Node[10];
delete[] myNode; //to free
```

- Note: make sure you always use new and delete together, and use new[] and delete[] (for array)
together.
- Nit: use student environment before summitting to Marmoset: zero tolerance

#### returning by value/ptr/ref

```
Node getMeANode(){
	Node n;
	...
	return n; //return by value
	//expensive
}

Node * getMeANode(){
	Node n;
	return &n;
	//n is defined locally in the stack in the function, 
	//after that &n goes to main, but n on that stack not accessible anymore
}

//right soln
Node * getMeANode(){
	Node *np = new Node; //new Node on heap, remember to free somehow
	return np;
}
delete np;

```

#### operators overloading

*about function overloading*

- in c, the name is the way to identify a function
- in c++, a function is unique by its signature
	- name
	- number of parameters
	- type of parameters  
	So one of them diff -> whole diff
- function overloading: same name different function
- can have multiple functions of same name as long as the *number of params and/or the types* of params are different
- can differ on return type, but they may not differ just on return type
- Functions that differ by a constant parameter are also overloadable only if the parameter
is also a reference. `int foo(char& c, int n); int foo(const char& c, int n);`

```
struct Vec{
	int x, y;
}
Vec v1{1,2};
Vec v2{3,4};
Vec v3 = v1 + v2; //how?

Vec operator+(const Vec &v1, const Vec &v2) {
	Vec v{v1.x+v2.x, v1.y+v2.y};
	return v;
}

//*?
```

##### Overloading << >>


```
struct Grade{
	int theGrade;
}
Grade g;
while (cin >> g) {
	cout << g << endl; //how?
}

ostream &operator<<(ostream &out, Grade &g) {
	out << g.theGrade << "%";
	return out; //return a reference to standard output
}

istream &operator>>(istream &in, Grade &g) {

}
```

- << >> Cascading: deal with a sequence of data, returns a reference given

#### the prepocessor
1) include  
\#include<iostream>  --> from /usr/include/c++   
or    
\#include</../../../iostream>  

```
Valid usage:

#include <iostream>

#include<usr/include/c++/7.9.3/iostream>

#include "myfile.h"

#include "/u/vsakhnin/CS246/1185/.../myfile.h"

Not valid cases:

#include <myfile.h>

#include </u/vsakhnin/CS246/1185/.../myfile.h>
```

2) #define VAR VALUE

3) #define FLAG //no value assign to it

```
#define BBOS 1
#define IOS 2
#define OS BBOS
#if OS == BBOS
long long int publickey;
#elif OS == IOS
short int publickey;
#endif
```

comment out some part

```
# if 0
...
#endif
```

g++14 assign

In the implementation:

```
int main(){
	cout << X << endl;
}
```
Call by `g++14 -DX=15`  


```
#ifdef Debug1 //if Debug1 has been defined
...
#endif

#ifdef Debug2
...
#endif
```
call by `g++14 -DDebug1 -DDebug2` //define without value

```
ifdef Name //true if Name has been defined
ifndef Name //true if Name has not been defined
```
To prevent fies from being included more than once USE: #include guard; must be in every `.h` you create!

```
//if you include .h several times, it can deal with it
//In vector.h:
#ifndef _VEC_H_ //a unique name
#define _VEC_H_
...
#endif
```


### modules and compile

c++/separate/example1:  

- vector.h: struct def, function declare (not compile this)
- vector.cc: #include "vector.h"
- main.cc: #include"vector.h"

- compiling separately, do not build executable, produce an object file `.o`:
	- g++14 -c vector.cc
	- g++14 -c main.cc
- g++14 vector.o main.o -o main //link the object files into executable

**Note**:

- Never ever compile `.h` file
- Never ever include `.cc` file
- if you try to add `int globalNum;` in `.h` file:  
	- not allowed, since `.h` is included in different files, define var more than once
	- to make `int global Num;` only a declaration: `extern int globalNum;` in `.h`
	- Then in `vector.cc`: `int globalNum;` //define
	- Then in `main.cc`: `globalNum = 100;...` //give value, not neccessarily main file
	- can be in the same `.cc` file: `int globalNum = 5;`
- if you have `extern const int num;` in `.h`: that `num` must be defined like `const int num = 1`
- never put `using namespace std` in `.h` file (just a programming style, leave it to the client to use it in the `main` if they want)
	- that being said, in the implementation, you should use: `std::cout`, `std::cin`
	- you can put it in the `.cc` file, or like the following: 
	
	```
	using std::cout;
	using std::endl;
	using std::ostream;
	using std::string;
	using std::swap;
	using std::to_string;
	```



###default value

Never assume any default value in primitive type, initialize on your own;  
define your default value in your default ctor

### class
#### struct
c++/classes/constructors

- `float grade()`: methods/memeber functions
- See: you don't even define the vars before you use it in `grade`? Yes, the vars will take the instance's var value
- methods take a hidden extra parameter called `this` which is a pointer to the object upon which the method is invoked
	- see `student2.cc`: `this->assign` 
	- this = &s
	- in the mordern c++, you don't have to use `this` unless there will be a confusion

- initialize objects:
	- default constructor: `Student Billy {1,2,3}` will match things in `{}` to fields in struct
	- or `Student Billy = Student{1,2,3}`
- override constructor:

	```
	//studentCtor.cc
	Student(int assigns, int mt, int final) {
    this->assigns = assigns;
    this->mt = mt;
    this->final = final;
  }
  ```

- initial var in c++

```
int x=5;
string s = "hello";

int x{s};
string s{"hello"};

int x(5);
string s("hello");
```  

#### ctor

Advantage of creating ctor:  

- default parameters (all default vars must come last)
	- default params have to only occur at right of argument
	```
	int foo (string x, int y=5);
	int foo (string x, char y='x'); //invalid, conflict with first one
	```
- over loading
- sanity check
- add logic other than the default ones



#### MIL

```
struct MyStruct{
	const int myConst;
	int &myRef;
}
```
What happens when an object is created?  

- space is allocated
- fields are constructed //MIL
- ctor body runs
- So for this case... Member Initialization List

```
struct S {
	int &x;
	const int p;
	//valid constructor
	S(int &x, ...): x{x}, p{p} {
	}
}
```



**Note:**  
Use MIL whenever possible in ctor!  
You should/can initialize all fields using MIL!
Fields in MIL are initialized in the order in which they are declared in the class, not in MIL
MIL can be more efficient than init in the body of ctor.  
MIL takes precedence over in-class init.  
#### one param ctor
Be careful with ctors that can take one parameter:  
everytime you design a one param ctor you should have `explicit`

```
struct Node {
	Node(int data): data{data}, next{nullptr} {}
}

Node n(4); //ok
Node n=4; //ok, data is 4, next points to nothing

int f(Node n) {...}
f(4); //ok
//but we prefer user to match argument type with param type, so:
exlicit Node(int data): data{data}, next{nullptr} {}
//then the following not valid:
Node n=4; 

int f(Node n) {...}
f(4); 
```

#### no param ctor

```
Node(int data=0, Node *next = nullptr) {}

//or 

Node() : data{0}, next{nullptr} {}
```

#### copy constructor
`Student bobby = billy; //copy constructor`

```
Student(const Student & other): assn{other.assns}, mt{other.mt}, final{other.final}
 //when there is =, invoke copy constructor
```



#### default
Every class comes with: (each one can be overwritten)

- a default ctor
- a copy ctor
- a copy assignment operator
- a destructor
- a move ctor
- a move assignment operator

linkedList.cc: create a linkedlist of nodes -> shallow copy

then shallow copy: (anything that pointers point to [on heap] is shared)

```
Node *n = new Node{1, new Node{2, new Node{3, nullptr}}};
Node m = *n; //copy ctor
Node *p = new Node {*n};
```
```
m 1 ----
	    |
n    1 -> 2 -> 3
		 |
p    1 -
```

deep copy:

```
Node (const Node & other): data{other.data}, next{other.next? new Node{*other.next}: nullptr} {}
```

The copy constructor is called:  
when an object is initialized by another object  
when an object is passed by value  
when an object is returned by a function  
(There are exceptions that we will discuss later)




#### destructors
Shallow delete:  

```
Node *np = |1| -> |2| -> |3|
delete np; //only delete the first node; note that we don't use delete[] here
```
Therefore, the orginial delete is not enough.

When an object is destroyed, a method dtor runs:  
1. dtor body runs  
2. dtors are invoked for fields that are objects, in reverse declaration order, continually
3. space is deallocated  

```
struct Node {
  int data;
  Node *next;
  Node(int data, Node *next): data(data), next(next) {}
  Node(const Node &n):
    data(n.data),
    next(n.next ? new Node{*n.next} : nullptr) {}

  explicit Node(int n): data(n), next(NULL) {}

  ~Node() {
    std::cout << "Destructor called" << std::endl;
    delete next;
  }
};
//delete will be continually called
```



#### copy assignment operator

```
//Question: which method is invoked?
Node n {1, new Node{2, new Node { 3, nullptr}}};
//constructor

Node m = n;
//copy ctor

Node w;
//ctor

w = n;
//invoke copy assignment operator! Since w is already constructed; we now assign n to w
```



```
Node &operator = (const Node & other) {
	data = other.data;
	delete next;
	next = other.next ? new Node (*other.next) : nullptr;
	return *this;
}
//result is a copy of type Node  
//why we don't use ctor? because you might cause memory leak: lose connection for old things but have not freed

//self assignment?
//in the original assignment operator, self assignment is valid:
int x = 5;
x = x;
//So we want to fix the above version to allow self assignment:

//support self assignment version:
//&: pass-by-reference & return-by-reference
Node &operator = (const Node & other) {
	if (this == &other) return *this;
	data = other.data;
	delete next;
	next = other.next ? new Node (*other.next) : nullptr;
	return *this;
	//this is an implicit pointer to the stuff in left side of the operator
}

//better: not delete first, delete after copy
Node &operator = (const Node & other) {
	if (this == &other) return *this;
	Node * tmp = next;
	next = other.next ? new Node (*other.next) : nullptr;
	data = other.data;
	delete tmp;
	return *this;
}
```

Copy and Swap Idiom

```
void swap (Node &other) {
	using std::swap; 
	swap(data, other.data);
	swap(next,other.next);
}

Node &operator = (const Node & other) {
	Node tmp = other;
	swap(tmp); 
	return *this;
	//tmp is a local variable on the stack, so tmp will be deleted automatically
}
```

or 

```
BigInteger &BigInteger::operator=(const BigInteger &in) {
  if (this != &in) {
    BigInteger copy = in;

    std::swap(digits, copy.digits);
    size = copy.size;
    capacity = copy.capacity;
  }
```
or

```
void Node::swap(Node &other) {
  int tmpdata = data;
  data = other.data;
  other.data = tmpdata;

  Node *tmpnext = next;
  next = other.next;
  other.next = tmpnext;
}

Node &Node::operator=(const Node &other) {
  Node tmp = other;
  swap(tmp);
  return *this;
}
```

```
Node plusOne(Node n) {
	for (Node *p = &n; p; p=p->next) {
		++ p->data;
	}
	return n; //is a rvalue
}
//since plusOne(n) = ... is invalid

Node m3 = plusOne(n);
//if there is no move ctor: invoke copy constructor
//normally the copy ctor accept lvalue as the parameter
//intermediate values will be temperately on stack, has lvalue, so above code is valid

```

#### move constructor

- in this course, you only need to know one use case of rvalue references: as the only
parameter to move constructors and move assignment operators.  
- similar to copy ctor, but with reference to rvalue, compiler will not need to store intermediate value in the memory
- Idea: we should transfer the ownership of the data from one object to the newly created
object instead of creating an actual (deep) copy of the data

```
Node(Node &&other): //reference to rvalue: &&
data{other.data}, next{other.next} {
	other.next = nullptr; //or the destructor will delete the data we transfered
while the object goes out of scope
}
//make m3 to point to the rvalue, no extra copy
```

#### move assignment constructor

```
Node m4;
m4 = plusOne(n);

Node &operator=(Node && other) {
	using std::swap;
	swap(data, other.data);
	swap(next, other.next);
	return *this;
}
```

or

```
BigInteger &BigInteger::operator=(BigInteger &&rhs) {
// Typically, the self assignment check is not required.
// but added here to prevent someone from actually performing
// a self-assignment
if (this != &rhs){
size = rhs.size;
capacity = rhs.capacity;
delete [] digits;
digits = rhs.digits;
rhs.digits = nullptr;
}
return *this;
}
```

- If you don't define a move ctor/move ass. op, the copying version of these operators will be used instead
- if the move ctor/move ass. op is defined, it will replace all calls to the copy constructor/ copy ass. op where the argument is a temporary rvalue.s

#### side
- functions are lvalue, but they return rvalue
- instance/object: created from the class type
- If you have to write one of the following:
	– copy constructor
	– move constructor
	– copy assignment operator
	– move assignment operator
	– destructor
	Most of the time you should probably write all five.

Review: bigInteger.cc

#### copy/move elision
In some cases, the compiler is allowed to skip calling copy/move ctors (say in `s = func()`). No temp object 
on stack, convert to use ctor.  
Bypass elision: `-fno-elide-constructors`


### const object

**function**  

- constant object cannot be modified
- if the object is const: say `int f(const Student &st)`, we cannot call a method on a const object (even if the actual implementation doesn't mutate anything, the complier can't tell)
- So you need const in funtion signature `float grade() const`

**field for const object specific**

- What if I want to count how many times the const function is involved?
- Note:

```
struct Student {
	int num = 0;
	float grade() {
		++num; //valid here since the func is not const
	}
}
```

- how to do it for const func?
- add mutable: `mutable int num = 0` and `++num` in `float grade() const` so that you can do `st.grade()` for `const Student & st`
- each object has its own `num`
- Therefore, mutable fields can be changed even if the object is const

**track num of calls associated for the whole class**

- `static int numInstanceCreated;` in the field; static means one for the whole class
- in `Student() { ++numInstanceCreated; }`
- define outside the struct definition or in implementation
- Therefore, static members are associated with the class itself and not with a particular object
- static members must be defined outside the class: `int Student:: numInstanceCreated = 0`

**static method**

```
static void printNumInstances() {
	cout << numInstanceCreated << endl;
}
```

- static member methods don't depend on a specific object, they don't have implicit `this`
- they can't access fields that are not static, they can only access only static fields

```
int main() {
	Student Mary {70, 80, 90};
	Student Dave {70, 90 50};
	Student:: printNumInstances();
}
```

- Note: you can access in main `Student::numInstanceCreated` but better to use `Student:: printNumInstances()`


#### standalone function
A stand-alone function is just a normal function that is not a member of any class and is in a global namespace. For example, this is a member function:

```
class SomeClass
{
public:
    SomeClass add( SomeClass other );
};
SomeClass::add( SomeClass other )
{
    <...>
}
```

And this is a stand-alone one:

`SomeClass add( SomeClass one, SomeClass two );`



### class List to regular creating struct Node

*Why encapsulation*

- enforce invariants
- clients can only manipulate objects via provided methods

client may do:
```
struct Node {
	int data;
	Node *next;
	Node(int data, Node *next):data{data},next{next} {}
	...
	~Node(){delete next;}
};

int main() {
	Node n1 {1, newNode{2, nullptr}};
	Node n2 {3, nullptr};
	Node n3 {u, &n2};
	}
```

- dtor will be called 3 times
- if `next` is a pointer to the heap or `nullptr`, dtor works perfectly
- `invariant`

Seeing more:  

```
struct Node {
private:
	int data;
	Node *next;
public:	
	Node(int data, Node *next):data{data},next{next} {}
private:	
	...
	~Node(){delete next;}
};
```

- default: in struct, all fields are public
- but in `class`, by default all fields are private
- the only diff: private/public defualt

Continue to solve the problem:
- 

```
//list.h
class List{
	struct Node;
	Node *theList = nullptr;
public:
	void addToFront(int n);
	int ith(int i);
	~List();	
};

int main() {
	List l;
	l.addToFront(5);
	l.addToFront(10);
	cout << l.ith(0) << endl;
}

//list.c
struct List::Node{
}

List::~List() {delete theList;}

List::addToFront(int n) {
	theList = new Node{n, theList};
}

int List::ith(int i) {
	Node *cur = theList;
	for(int j=0; j<i && cur->next; ++j, cur=cur->next);
	return cur->data;
}
```

- only methods in List class can access the private fields, say Node
- Solve the problem: since user cannot invoke ctor for Node anymore; anything is done throught `List`


##### side note for ctor
in struct, you can do `Simple s1{1,2};` without defining a ctor; but if Simple is a class, you must define a ctor passing in params

### Iterator

See the physical copy code:

- In c++, `auto x = y;` define x as of the same type of the variable of the right hand
- We use `++it` prefix, as we implement `++`
	- postfix increment `it++` needs a copy:
	
	```
	iterator tmp(*this); // copy
	++(*this);           // prefix increment
	return tmp; 
	```
- can replace `List::Iterator` with `auto`
- c++ built-in support iterator pattern : the range-based loop if and only if the class (`List`) has the following properties:
	- end() and begin() are defined
	- the iterator supports ++, !=, *
- From the code we have, can client define its own iterator? `auto it = List::Iterator(nullptr);` ?
	- it is possible, because the Iterator constructor is public
	- but should not allow, since the `Iterator` is customized for `List`
	- So to modify: 
		- consider: if we make constructor private by removing `public`, how can `begin()` `end()` access it?
		- Solution: at the bottom of class Iterator definition, add `friend class List;`, but it is against the principle of encapsulation
		- So make as few friend as possible

		
```
class Vec{
	int x,y;
public:
	//accessor
	int getX() const { //it will also work for constant object
		return x;
	}	
	//mutator
	void setY(int newY) {
		y = newY;
	}
}
```

- instead of getter, we can also have `friend` between the class and a standalone function

		
```
class Vector {
  int x, y;

 public:
  explicit Vector(int x = 0, int y = 0);

  Vector operator+(const Vector &v) const;
  Vector operator*(const int k) const;

  friend std::ostream& operator<<(std::ostream &out, const Vector &v);
  //so that when use <<, it can access v.x v.y which is private
};

Vector operator*(const int k, const Vector &v);
```



### System modelling (UML)

```
Name | Vec
fields (optional) | - x: int
					 | - y: int
methods (optional) | +getX(): int
					  | +getY(): int	
					  
- means private
+ means public				 
```

### Relationship: Composition

- A "owns-a" B:
	- B has no identity outside A
	- if A is destroyed then B is destroyed
	- if A is copied then B is copied
	- A is responsible of deleting member B

```
class Basis {
	Vec v1, v2;
	...
	Basis(): v1{0,0}, v2{0,0} {}
	//note we cant:
	Basis() {
	//fields are init before body, but in Vec, there is no default ctor!
	v1 = Vec(0,0);
	v2 = Vec(0,0);
	}
}

|Basis| <>-----2 |Vec| (black)
```

### Aggregation

```
A<>-----B (white)

```
A has-a B

- B has an existence apart from A
- if A is destroyed, B lives on
- if A is copied, B is not copied
- B doesn't know about the existence of A
- B could belong to more than one A
- A is not reponsible for deleting/destroying B

- i.e. B student, A department

```
class Teacher {
	string name;
public:
	Teacher(string name): name{name} {}
	string getName() {return name;}
}

class Department{
	Teacher * m_teacher[100];
	int t_Num;
public:
	Department(): t_Num{0} {}
	void add_teacher(Teacher *teacher) {
		m_teacher[t_num] = teacher;
		++t_Num;
	}
//shallow destructor: destroy pointer connection
}
```

|Department| <>-----0..100 |Teacher|

```
int main() {
	Teacher * teacher1 = new Teacher("Bob");
	Teacher * teacher2 = new Teacher("Mary");
	{ //a block, everything defined in the block has life inside block
		Department dept;
		dept.add_teacher(teacher1);
		dept.add_teacher(teacher2);
	}
	//now that department object destroyed
	//after the end of that scope:
	cout << teacher1->getName() << endl;
	//when A destroyed, B still alive, so the client need to destroy B (main function destroy B):
	delete teacher1;
	delete teacher2;
}
```
###inheritance

Union
```
class Book{
	string title, author;
	int numPages;
public:
	Book(...);
...
};

class Text{
	string title, author;
	int numPages;
	string topic;
public:
	Text(...);
}

class Comic{
	string title, author;
	int numPages;
	string hero;
public:
	Comic(...);
}

union BookTypes {
Book *b;
Text *t;
Comic *c;
};

BookTypes myBook[20];
```

B is-a A => can use B whenever A is required  
A is called parent /super/base class  
B is called child/sub/derived class

```
class Book{ 
//Base class (superclass)
	string title, auther;
	int numPages;
public:
	Book(...);
...
};

class Text: public Book{
//subclass/derived class
//it doesn't mean Text has access to title, author
	string topic;
public:
//wrong
	Text(string title, ..., string topic): title{title}, topic{topic} {}
//right
	Text(string title, ..., string topic): Book{title, author, numPages} (step2), topic{topic} (step3) {}
}

class Comic: public Book{
	string hero;
public:
	Comic(...);
}
```

- fields and methods inherited from superclass
- any method that can be called on a Book obj can be called on a Text/Comic obj
- when an object is constructed:
 	1. space is allocated
 	2. the superclass fields are constructed (always consider) - call the parent class's corresponding ctor
 	3. fields are constructed
 	4. ctor body runs

*See the sheet*

Solution for the diff pointers cause diff behavior for `isItHeavy` for the same object:  
virtual + override:

```
class Book {
virtual bool istItheavy() const {...}
}

class Text:public Book {
bool isItHeavy() const override {...}
}

Now what is important is the real object, not the pointer pointing at it
```


```
Comic c {"", "", 40, ""};
Book *pb {&c};
Book &rb{c};
Comic *pc {&c};
pc->isItHeavy();
rb.isItHeavy();
pb->isItHeavy();
//all true

//and pb can access public fields methods in Comic
```

```
Book *myBooks[20];
for(int i=0; i<20; ++i) {
	cout << myBooks[i]->isItHeavy() << endl;
}
```


*see the sheet 2*

#### abstract class

- abstract class
	- its purpose is to organize the subclasses
	- if there is pure virtual method it becomes abstract class, no object of its type
	- subclass are also abstract unless they implement all the pure virtual methods
	- It can have some other not pure virtual methods
- classes that can be instantiated (no pure virtual methods) are called concrete classes
- In UML: all virtual and pure virtual methods should be using itatlics, same with the name of any abstract class


#### inheritance copy/move operator
c++/purevirt

```
//copy constructor
Book::Book(const Book &b): title{b.title}, author{b.author}, length{b.length}{};
Text::Text(const Text &other) : Book{other}, topic{other.topic} {} //slicing for Book here, it is what we want

//copy assignment op
Text& Text::operator=(const Text &other) { 
	if (this == &rhs) return *this;
	Book::operator=(rhs); //assign whatever in Book part to the this part
	topic = rhs.topic;
	return *this;
}

//move ctor
Text::Text(Text &&other) : Book{std::move(other)}, topic{std::move(other.topic)} {}

Book::Book(Book &&b): title{std::move(b.title)}, author{std::move(b.author)}, length{b.length} {}


//move assignment op
Text &Text::operator=(Text &&rhs) {
  if (this == &rhs) return *this;
  Book::operator=(std::move(rhs));
  topic = std::move(rhs.topic);
  return *this;
}

Book& Book::operator=(Book &&rhs) {
  cout << "Book move assignment operator running ..." << endl;

  if (this == &rhs) return *this;
  title = std::move(rhs.title);
  author = std::move(rhs.author);
  length = rhs.length;
  return *this;
}

Text t1{};
Text t2{};
 t2 = std::move(t1);  // Force move assignment.  Data will be swapped!
```

```
Text t1{...}, t2{...};
Book *pb1= &t1, *pb2 = &t2;
*pb1 = *pb2; //Book copy assignment op, compiler only know Book type
//the result is a partial assignment

//To fix this:
//virtual constructor / assignment op in Book: but not enough, but we have different type of parameters, we are not actually overriding the mehtods? -> but c++ allows it, but it does not mean it is correct

//suppose we have virtual 
Text t{...};
Book b{...};
Text *pt = &t;
Book *pb = &b;
*pt = *pb; //Text version invoked
```
https://piazza.com/class/jfo1tuk4fht5j1?cid=1105

If I still want to override:
```
class Book {
	virtual Book &operator=(const Book &other) {...}
};

class Text: public Book{
	Text &operator=(const Book&other) override {...}
};
//problem: mixed assignment => really bad, I don't wat compiler to allow the mix
```
To prevent mix assignment: copy assignment in protected in Abstract, `*p1 = *p2` not allowed

So now I still want normal Book, but still want inheritance, so we have `AbstractBook`

**SIDE QUESTIONS**

- What is the point to implement the pure virtual method in .cc?
- Should we always have `override	` for child class for virtual/pure virtual method (in .h)?

###STL (the standard template library)

vectors: dynamic-length arrays
and more...

### exception

```
void f() {
	throw out_of_range("wrong");
}
void g() {f();}
void h() {g();}

int main() {
try {
h();
}
catch(out_of_range r) {
...
}

//in the chain of function call, you throw an error in f, as long as there is a matching handling "catch" in the chain process, it is fine,
which means the program will not terminate because the error is caught
```

Catch anything:

```
try{
//do whatever might raise an error
}
catch(...){ //... means catch anything
}
```
Keep it minimal, too many exception will be slow

```
class BadInput: public runtime_error{
//my class can invoke anything from runtime_error (has_a)
	BadInput(const string & msg):
	runtime_error(msg+"ReallyBad") {}
}
//can catch all runtime_error with BadInput (now you have customized msg as well)
```

Note: never let a dtor throw an exception, since if it is being executed during stack unwinding,
while dealing with another exn, you now have two active unhandled exns and the program will terminate

### template
see physical sheet

### Observer Pattern
=> publish-subscribe model  
=> Behavioral pattern  
=> one class: publisher/subject: to generate data  
=> one or more subscribers/observers: recieve data and react on it

Teacher: always make your super class abstract, technicallly considered as the interface of the concrete class

Sequence of calls:  
1. subject's state is updated
2. subject: notify observers() calls each observer's notify()
3. each observer calls concreteSubject::getState() to query the state and reacts on it

When to use:  
You have a lot of dependencies


### Decorator pattern

- main purpose: to attach additional responibilities to an object dynamically (during runtime)




#### side questions

- write ctor in abstract base class and init in MIL in subclass?

###tutorial

*override*
- override: as long as signature is the same: function name same, params type and num(not return type)
- virtual only serves for pointer or reference 

```
A *pa = new B;
pa->foo(); //with virtual, call B's foo
A a = B(); 
a.foo(); //will call A's foo
```

- Declaring a method virtual means if we override it in a subclass, the subclass version of
the method will be called through polymorphic pointers.
– If we do not override the method, the definition in the closest related ancestor will be
used. For instance, calling fly() on a Cat will return false.

override override
```
struct Animal {
    virtual bool fly() const {
        return false;
    }
};

struct Cat : public Animal {};

struct NyanCat final : public Cat {
    bool fly() const override {
        cout << "NYAN NYAN NYAN NYAN..." << endl;
        return true;
    }
};

struct Bird : public Animal {
    bool fly() const override {
        return true;
    }
};

struct Goose final : public Bird {
    bool fly() const override {
        cout << "THANK MR. GOOSE" << endl;
        return true;
    }
};
```

- no big 5 in inheritance! You should always use pointer in inheritance, so  
no need for copy ctor (otherwise an explicit clone function)



### NVI idiom

Non-virtual interface  

- all public methods should be non-virtual  
- all virtual methods should be private, or at least protected
- Use public methods to access those "virtual" functions in inheritence
- except for the dtor: make it virtual, but not private
- (we don't make constructor virtual)
- interface for user is only for user, interface for subclass is only for subclass


### STL: map

```
#include<map>
map<string, int> m;
m["abc"] = 1;
m["def"] = 4;
cout << m["abc"] << endl;
cout << m["ghi"] << endl; //a key doesn't exist, added with a value 0

//update or add
m["abc"] = 2;

//delete
m.erase("abc");

//exist?
if(m.count("def")) { //0 not exist, 1 exist
	cout << "found" << endl;
}

//map supports iterator
for (auto &p: m) {
	cout << p.first << ' ' << p.second << endl; //first and second is attribute
}

output:
1 
0
found
def 4
ghi 0
```
we don't care about the order 



*factory method pattern*
*template method pattern*
*visitor pattern*


se/visitor1/2

### circular dependency

```
#inlcude "B.h"
class A{
public:
	B* b;
};

#include "A.h"
class B{
public:
	A* b;
};
```

The compiler doesn't need to know specific size B A;  
So to fix the problem, and get rid of circular dependency:  
(Note that it is a pointer use)

```
class B;
class A{
public:
	B* b;
};

class A;
class B{
public:
	A* b;
};
```

But what if we want to have the object instead of a pointer?  

- Advice 1: include a few .h files as possible in .h files
- Advice 2: include .h files into .cc files whenever possible

Now let's consider:  

```
class XWindow{
Display *d;
window w;
int s;
GC gc;
...
public:
...
};
```	
Problem: everytime I change the method, I need to complie all fields that user can't access to
*pImp idiom*

### Measures of design quality

**coupling**

- def: the degree to which the program modules depend on each other
- want low coupling
- minimize friend -> low coupling
- sharing global data -> increase coupling
- minimize trying to pass different structures between different models -> low coupling
- if one model changes doesn't affect others: low coupling

**cohesion**

- How closely elements of a specific module are related to each other (high is good)
- One module one role only
- related specific classes in one place, bound together 


### MVC - model view controller pattern

Bad example: 

```
class chessBoard{
...
cout << "your move";
...
};

//this class has 2 responsibilities; bad
```

Principle: single responsibility  
A class should only has one reason to change  

- do not want to print in main, since we want other projects can also use my components as well
- should seperate the role between different models
- Thus MVC pattern:
	- seperate the distinct notations of the data("model"), the presentation of the data("view"), and the control
	or manipulation of data("controller")


### Exception Safety & RAII(Resource Acquisition Is initialization)

```
void f() {
	Myclass *p new Myclass;
	Myclass mc;
	g();
	delete p;
}
```

- when an exception is raised: what is guaranteed? 
	- stack unwinding: all stack allocated data is cleaned up; dtors run, memory reclaimed
		- problem: heap-allocated memory is not freed; so if we don't handle exception in g, compiler will not free stuff on heap

```
//working one possible solution
try{
	g();
}
catch(...) {
	delete p; //but there might be many many more lines
	...
}	
```

RAII: every resource should be wrapped in a stack-allocated object, whose destructor deletes it

```
ifstream f{"file.txt"};
//whenever f is popped out of the stack, the file is closed (with its special dtor)
//this is a kind of wrapping object
```

So we will be using: `class std::unique_ptr<T>` is class that holds T*,  
which provides a dtor that will delete the ptr. In between you can dereference   
the object like any regular ptr.

Note in RAII2:
if you do this:

```
class C{...};
unique_ptr<C> p {new C{...}};
unique_ptr<C> q = p; //ERROR: if working, deleting q first on stack, freeing the object on heap
//and then when p is popped(or p is further used), p is not pointing in valid data anymore
```
Thus, copy ctor not allowed   

- There are 3 levels of exception safety for a function f:  (other than no safety)
	1. Basic guarantee: 
		- in some valid state, but which one isn't known
		- class invariants still hold (t should hold true from the end of the constructor to the start of the destructor whenever the object is not currently executing a method that changes its state.)
		- no memory leak 	
		- exception still propagates
	2. strong guarantee: if f throws an exception, when caught, it will be like f is not invoked
		- puts state back to before f executed (+basic guarantee)
		- exception still propagates
	3. no-throw guarantee: f always accomplishes its task and never raises(or propagates) the exception
	If an exception occurs, it will be handled internally and not observed by clients.

Example 1:

```
class A{
public:
	g(); //strong guarantee
};
class B{
public:
	h(); //strong guarantee
};
class C{
A a;
B b;
public:
	void f() {
	a.g();
	b.h();
	}
};
```

Q: What sort of safety does C::f provide?

- no handler code, so not "no-throw"
- if a.g raises an exception: 
	- since strong guarantee, it's as if a.g never executed, as if C::f hadn't run(revert code in g, h will not run)
- if b.h raises an exception: need to undo changes made by a.g if want to provide strong guarantee,
 => may not be possible
	- not enough info to know if can provide either strong or basic
- if change global info, or had non-local side-effects, may not be able to undo change => probably not exception safe

- if A::g and B::h do not have non-local side effects, can use "copy and swap"

```
void C::f() {
A atemp = a;
B btemp = b;
atemp.g(); //if throws, original a untouched
btemp.h(); //if throws, origina b untouched
a = atemp; //what if copy raises an exception? 
b = btemp;
}

//if copy can raise an exn => not exception safety
//would like to use a method that can't fail, which means we leverage the fact that
copying a pointer (memory address) can't fail
//use pimpl idiom
//so exception safety relies on providig a no-throw swap
```

```
struct cImpl{
	A a;
	B b;
};
class C{
	std::unique_ptr<CImpl> pImpl; //assume ctor initializes
public:
	void f(){
		auto temp = make_unique<CImpl>(*pImpl);
		temp->a.g();
		temp->b.h();
		std::swap(pImpl, temp); //no-throw for this step
	}
};
//if neither A::g nor B::h offer exception safety, neither can C::f
```

#### STL vector and exception safety

- STL vector uses heap-allocated memory but uses RAII to ensure vector itself isn't leaked
	=> but makes no guarantee about contents  
	`std::vector<C> myVector; //runs dtor for all C obj`  
	`std::vector<C*> myVector2; //leaks pointers  for(auto it:myVector2) delete it;`  
	but dtor for loop not executed if exn raised! 
	- can fix problem by having a vector of smart pointers  
	`std::vector<unique_ptr<C>> myVector3;` => now don't need to explicit deallocation
- `std::vector::emplace_back` offers strong guarantee  
	- when underlying array needs to be resized:
		- allocate new, larger array
		- use copy ctor to copy objects
		- if copy ctor throws exn, throws away new array; original still intact, so it is strong guarantee;  
		else(no exn): delete old array
	- but copying is slow and old data thrown away
		- can we use move semantics? (still need to allocate new array + delete old array)
		- maybe, but what if move ctor raises an exn emplace_back can no longer offer strong guarantee if original array modified   
		If move ctor offers no-throw guarantee, `emplace_back` will use it; otherwise `emplace_back` will use the copy ctor
		- make move ctor with "noexcept" keyword to show makes no_throw guarantee
		
		```
		class C{
		public:
			C(C && other) noexcept {...}
			C &operator=(C && other) noexcept {...}
		};
		```
		
		- guideline: if you know a method will never throw or propagate an exception, mark it as "noexcept"   
		Lets the compiler make some optimizations; at a minimum, moves and swaps should be noexcept


### pointer

To sum up:  
We shared-ptr and unique-ptr instead of raw pointers as much as possible.   
They will dramatically reduced the chance of memory leaks.  
For more complicated resources, you might need to create your own wrapper (a class with pointer to the resource type   
with appropriate ctor, dtor, etc.)   


### Casting

- converts from one type to another
- in C: 

```
Node n;
int* ip = (int*) &n; //&n is (Node*)
```

- do not use C-style casting in this course!
- C++ has 4 different ways to cast:
1. static_cast: is for sensible casts where there is a well-defined behaviour

```
//double to int
double d;
void f(int x);
void f(double x);
f(static_cast<int>(d));
...
Book *b;
Text *t = static_cast<Text*>(b);
```

2. reinterpret_cast: for unsafe, implementation-dependent, "weird" conversions. Most uses result in undefined behavior.
`student s; Turtle *t = reinterpret_cast<Turtle*>(&s);`

3. const_cast: used to add or remove "constness"
- only c++ that can remove constness(beware of const poisoning)
- change the const int to an int type (you can modify it but you should not)

```
g(int *p);//only do if g really doesn't change p
void f(const int *p) {
	g(const_cast<int*>(p));
}
```

```
int i = 0;
const int& ref = i;
const int* ptr = &i;

const_cast<int&>(ref) = 3; //valid, but not supposed to
*const_cast<int*>(ptr) = 3;
```

4. dynamic_cast: convert from superclass pointer to subclass pointer(can use static_cast if sure conversion will work)  
If conversion fails, pointer set to nullptr
```
Book *pb;
Text *pt = dynamic_cast<Text*>(pb);
if(pt) cout<<pt->getTopic();
else cout<<"not a text";
```




















