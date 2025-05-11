# Rust Learning Journey

Learning a new programming languages is always exciting with a satisfaction of learning something new and a possibility that this could be the one to rule them all. I started my programming journey with C although exciting at first the pointers and dereferences bought it to a grinding halt and made it daunting to go back to it. Then came java with the rumor that I do not have to see references or pointer while writing code which the naive programmer in me believed and was true to a part that I did not have to dereference a pointer although object had pointers and they pointed to the same value in memory. However, not having to deal with pointers made Java a true safe haven for me and I went in for the ride. Later, I've learned C# which I've heard as a "Brother to Java" and I felt the same so I did not feel like I've learned a new language. Then there was Go back with the concept of pointer, but I have been acquainted with programming so it did not seem so daunting to me this time and I cherished the concept of channels and all the features it bought but not so overwhelming that it was possible to read the Go language spec in few days. While I was sailing through the programming seas I have been hearing the chatter on Rust albeit having a learning curve even after learning a few languages made life easier once you embraced it. One day I serendipitously landed on Rust one page guide https://github.com/amartincolby/Rust-Quick-Guide with a promise of teaching Rust by relating the features to other programming languages whilst still being grounded. 

## Week 1: 
I started reading it since I did not feeling like working on other things and I was dragged into it due to the poetic intro to the the Rust quick guide that enfused me lot of enthusiasm that was needed to see it through to the end of the guide. I read it like a guide that gives provide info the new syntax whilst using the context from existing languages. On the re read, I felt the fog being lifted and me being able to see clearly through the seas. Continuing with the sea analogy at the risk of overusing it, I did not know how to navigate the seas at that point. May be another re-read could've made it much more clearer but like a kid who has lost interest in a toy after playing with it for a while, I was in search of some new guide to charter much deep into the Rust seas. That was when I've stumbled upon https://rust-book.cs.brown.edu/. Being lauded by many Rustaceans as a good learning material and with the site having quiz at the end of each concept serving to reinforce the concept learned by thinking/ solving it but also as a feedback to the site maitainers so as to make the modify content to make the concept more approachable if many participants were getting the quiz qestions wrong.

## Week 2:
Chapter 2 of the rust-book site had a small programming project that gave hands to the concepts that we are about to cover. At this point already have a understanding on Rust by having read through the quick-guide, I finished the programming assigned as it gave a chance to write code my self that meant having to remember the syntax and concepts back(which I feel is the fundamental part of any learning journy since we are trying to remeber what we've learned and also write code with it that reinforces the concepts.No wonder working on projects is the best way to learn just like solving a Math sum,  you do not truly understand the concept  no matter how many times you read the solution until you engage with it when you try to solve it.) 

I've breezed through the first two chapters reinforcing the concepts I've learned throug the quick guide, but it was when I've started chapter 04, when the book delved deep into references and borrowing which uncover the reasons as to why Rust has enforced the concept of ownership and what borrow checker does in the compile time. It all boils down to enforcing pointer safety principle that avoids alaising and mutating at the same time that is the root cause of many undesired and unexpected behaviours. That was dense and had a lot of Rust concepts that I need to get accilimatized to and understand the reason behind these decisions. It was satisfying to have gone through for a ride and come back more enlightened. Before I forget, I discovered a Rust tool https://cel.cs.brown.edu/aquascope/ through this blog which is used extensively to provide a clear view of what Rust does under the compiler hood.


The below notes serve as a key concept, that I felt need to be documented so that it served as a refernce to me in the future as a Polaris star to guide a sailor on the seas.
```
there are two kinds of references. 
1. shared references (read only) - &
2. mutable references (both read and write) - &mut

During compile time when we want to provide read refernce to a value on heap to other identifier, it provides read permission to the borrower while removing write permission from the owner. This is to ensure that owner does not mutate the value while the borrower tries to read the value through shared reference(aliasing) hence enforcing the pointer safety principle.

Similary, for a mutable refernce the owner loose both read and write permissions and the borrower gains both read and write permissions. Since a mutable reference give the ability to matuate the value to the borrower the owner not reading the value makes it safe and again enforcing the pointer safety principle. Either multiple identifier can read or only one identifier can both read and write.

//snippet 1
let x = 2;
let z = 3;
let mut y:&i32=&x


y in snippet 1 is a mutable identifier of type &i32 meaning it can take refernce from another i32 identifier maybe z.

vs

// snippet 2
let mut x = 2;
let z = 3;
let y : &mut i32=&x;

y in snippet 2 is an identifier of type &mut i32 means it can take a mutable refernce to i32 identifer x and it cannot do it again with z since it was not a mutable identifier. It could have been had y been declared as  (let mut y: &mut i32).

```

# Week 3
I'm back after a week or so, so my writing style might change based on the mood I'm in. Also, I've heard it is not gramatically correct to end a sentence with preposition(the last sentence was ended with an 'in'), but not an issue as long as it is understood as I intended it to be(is be a preposition?). Another pointer safety feature in Rust that I keep encountering in the docs is " references should not behave as if they own the data." Ofcourse, this makes sense becauses references only borrow the data R,W permissions but not the ownership permisssion?? (Rust aquascopt tool says otherwise). If references assumed they owned the data and moved it to other identifier then the original owner and the other identifier that reference moved ownership to , will free the data after going out of scope but that happens twice since we have a faulty state of two identifiers owning the same data now. So, as to avoid the "double free problem" rust does not give ownership permissions to the borrower. 

Other thing, I am noticing on re-read or might not have paid enough attention during the first read is str is not stored on the stack, &str(string slice) is. The reason for this being, rust needs to know the size of data it stores on stack so that it can alloccate exactly that amount of space. However str is a dynamic sized type and rust cannot decide the size of string literal that is going to be stored in str , rust stores the value in str on read only memory of the binary and the pointer to the read only memory and its size is stored in &str , which is in turn stored on stack. Since, rust knows the size of &str (string slice type) type is an address locaiton and the size of str literal it can allocate exactly that amount of space on stack. String is stored on stack, since String type stores address to value of string stored on heap. Since we are storing an address value, rust knows how much size that String type needs on stack(size of an addresss locaiton on heap).


Rust quiz, that brings a point in basics.
```
//Determine whether the program will pass the compiler. If it passes, write the expected output of the program if it were executed.

fn main() {
  let v = vec![String::from("Hello ")];
  let mut s = v[0];
  s.push_str("world");
  println!("{s}");
}

```

if we observe the line "let mut s = v[0]", I felt like, "yeah this could be a valid statement" passing string ownership from v[0] to s. But, what would be in v[0] if ownership is passed to s. For normal identifier it would be ok. 

```
let a = String::from("Hello"); 
let b = a;
```

At first 'a' had the ownership of "Hello " String and later it was passed to b. But in the Rust quiz, code snipped the ownership was part of vector and I haven't see such a code pattern before and is it valid to completely pass the ownership to a different identifier? I thought the program would compile and the output would be "Hello world" , but I was proved wrong . The answer was that the program wouldn't compile and the reasoning was "Non-copyable types cannot be moved out of a vector by indexing. Only methods such as Vec::remove permit moving out of a vector." Is the operation not permitted due to the reason that it could lead to double-free?? ie., both s and v trying to deallocate the memory and the string would be freed twice?? Yeah, that is the right reason. When s and v go out of scope, s frees the String and since v vector has to free its elements as well it will try to free the String at index 0 which would result in an undefined behaviour had the compiler not stopped us.

## Error handling
Rust has non recoverable errors via panic! and recoverable errors via Result. I felt this implementation was similar to Go, where a function returned two or more outputs and an error being one among them. However, in go we have to check if err is not nil and it is not enforced. We could even ignore error checking by using '_' don't remember what the name is? But, Rust uses an enum forcing users to handle the case. Does Result have any explict advantages over exceptions used in OOP languages? Additionally, even Go has panic but it can be recovered unlike in rust and I guess rust way of panic that it cannot be recovered makes more sense to me. unwrap and expect are like place holders that we use while prototyping something in Rust which are fully fleshed out while ironing out the code to make it production ready.

## Data structures (Vectors, Strings and hashmaps):
These are simple enough, just need to worry about primitive types and reference types due to the copy trait implementation. These data structures seem similar to other languages except that we need to be wary of passing references and reading via references.

## Rust Compiler Error and Warning messages:
I still remeber some compilers throwing errors and giving suggestions that neither  make any sense nor help solve the error. On the other side of that helpful compiler message spectrum rests Rust. I was amazed by the detailed error messages and suggestions to fix that Rust compiler produced. It was as if reading a code snippet and its explanation as to why it has failed from a documentation or some notes. It was clear. After looking at these messages, I wonder why don't other compiler produce such concise error messages? Is it due to the way language was designed not permitting the compiler engineers not permitting them to get more information on the error even if they wanted to???

## Tuple Struct 
struct WriteMessage(String);
For some reason, tuple struct always seem to throw me off. I understand the reason for creating tuple struct and its adavantages but the syntax somehow feels different. Shouldn't this be called `Struct Tuple`.

## Defining enum variants
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}

Move variant is holding struct type of data but not having the struct keyword before Move makes it seem weird. The Write and ChangeColor variants of type tuple struct which causes dual unusualness. These are some gripes while I try to wrap my head around Rust syntax.

## zero cost abstractions
I have glanced over this before, that abstractions in Rust are zero cost, meaning all the sytactic sugars and language features it provides come at no additional runtime overhead. For example consider, generics. 

```
fn largest<T: std::cmp::PartialOrd>(list: &[T]) -> &T {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest(&number_list);
    println!("The largest number is {result}");

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest(&char_list);
    println!("The largest char is {result}");
}
```

In the above code snippet, the largest function is generic over type T that implements PartialOrd trait. This means the largest function can be called on all the types that implement the partialOrd trait. PartialOrd trait ensures that we can compares the values fo that type using > operator. Now, the largest function can be called over slices of type u8, f32 ... . In the main function we are calling it over i32 and char. During compile time first Rust checks or atleast I think it checks whether largest function implementation does not do anything that is not supported by types that implement PartialOrd trait. If the funciton implementation is logically sound and is valid over all the types that implmenent PartialOrd trait it marks the code as logically sound. Now, coming to the zero cost abstraction part, Rust looks at the code and identified that the largest function is being called over i32 and chart types so it generates implementaitons of the largest function over i32 and char as shown in the code snippet below or atleast something similar so that it does not incur any runtime overhead.

```
fn largest_i32(list: &[i32]) -> &i32 {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn largest_char(list: &[char]) -> &char {
    let mut largest = &list[0];

    for item in list {
        if item > largest {
            largest = item;
        }
    }

    largest
}

fn main() {
    let number_list = vec![34, 50, 25, 100, 65];

    let result = largest_i32(&number_list);
    println!("The largest number is {result}");

    let char_list = vec!['y', 'm', 'a', 'q'];

    let result = largest_char(&char_list);
    println!("The largest char is {result}");
}
```

So generics are replaced with specific definitions created by the compiler. Because Rust compiles generic code into code that speicifes the type in each instance, we pay no runtime cost for using generics.When the code runs, it performs jusst as it would if we had duplicated each definition by hand. The process of monomorpihzation makes Rust's generics extrememly efficient at runtime.

## Lifetimes
so, whenever we use references in fuctions or methods definitoins either in input or output parameters in rust, we need to specify lifetime annotations. This helps rust to verify the lifetime of a variable during compile time. If we use the common example

```
fn largest<'a>(x: &'a str, y: &'a str)->&'a str
```

to find the largest of strings, Rust needs to know the lifetime of each parameter. If a function or method definition falls in to the lifetime elison category, rust annotates it for us automatically.1. Rust annotates each input parameter automatically, 2.  If there is only one lifetime annotation to input parameter the same is automaically applied to output parameter. 3. If it is a method then output parameter gets the same lifetime annotation as the self parameter.

## Trait bounds
While reading through the trait documentation, I understood all the code but forgot what trait bound meant in terms of code. Later, I found it and thought of documenting for my refernce. Trait bound in Rust are a way to specify that generic type must implement a paritucal triat(s) in order to be used. 

```
use std::fmt::Display;

fn print_item<T: Display>(item: T) {
    println!("{}", item);
}
```

From the above function, the Generic type T accept any type that implements Display trait.

# Week 4
The excitement for Rust carried me till Week 4 and I still excited about Rust go on. I can do this all week! :)
## Testing
I started reading about testing in Rust. I've written tests in other languages and testing in Rust felt like I was moving the same known path. However, tests in Rust were written in the same file as the business logic with a sub-module called test. I feel that having tests with business logic will make is clumsy, but on the the other hand having unit tests in the same file serves as a part of documentation. Rust uses #[cfg(test)] annotation so as separate the test code during compile time so as to reduce the compile time and output artifact sizes. 

    Diverging a bit from the testing topic, I've come across the word 'annotation' in multiple programming languages and I remember it as something  I add on top of a method or property as such to indicate the method is different or is to be interpreted differently. The dictionary meaning of it being "a note of explanation or comment added to a text or diagram".  

Getting back on the testing chain of thought, next were integration tests. As far as I've known, I considered integration tests as testing as a whole but couldn't give a better explanation than that. After reading the Rust docs , my mental model for Integration tests has been made more concise to state, Integration tests call your APIs(in library or server) as if called by a client. Rust suggests to put them in tests directory and it made sense so as to keep them separately and interact with them as if  using a client library. However, for some reason each file under tests directory is considered as a separate crate. Since, setup is common for all integration tests, Rust considers code under common directory(is it just common directory or any directory under tests?) as one whole module??

Since, we cannot technically run integration tests on binary crates since it is an exe and we cannot call it just like a library, we create binary crates with src/main.rs file and src/lib.rs. src/main.rs calls the functionality from src/lib.rs which makes it integration testable like a library .

## closures
as soon as I hear the word closures, it reminds me of some concept in Mathematics that seems arcane to me. That asided, I have heard of closures before but did not try to see what they really are about. I vaguely remember doing so in Go, when threads used closures. Now, that I am reading Rust documentation, I plan to build on my previous knowledge and add some detailed knowledge about closures to my programming mental notes. 

Firstly, I've observed closures do not have a function name, so does this mean closures are same as anonmymous functions. Does Rust even support anonymous functions. Yes, it does and according to The Book, "Rust's closures are anonymous functions you can save in a variable and pass an argument to other functions.". Additionally, the buzz around closures is that they can capture variables in the environment it is declared and since Rust has the concept of ownership, it check whether the variables are borrowed mutably, immutably or entirely moved(as in case of threads and other contenxt). Next, based on how a closure handles the variable ownership a closure is said to implement one or more of the traits Fn, FnOnce and FnMut. 

Besides, all of these I wonder why Rust designers decided to use '||' to capture arguments for a closure. That seems different than any other programming languages , but then again Rust is in a class of its own languages or to be precise one of a kind!

## iterators
huh, where do I begin. I am kind of torn between fully going into using iterators to make the code more elegant vs sticking with regular 'for loop' for the sake of simplicity and it is easy to set a breakpoint with the debugger(the breakpoint reason might not seem important to other, if you ask me it helps when you need it.). And regarding simplicity of for loops, I don't think iterators are too complicated but somehow I am into the feeling that they are not as performant as for loops. May be that is why The Book of Rust has a section comparing iterators with for loop interms of performance. Although, iterators may be as performant as for loops and even more elegant that for loops, I still have some reservations with using them since they cannot be debugged easily. Lets, see may be I find better ways to debug iterators and I might start to fully embrace that style of programming. 

# Week 5

## cargo
cargo, the name sounds as if it was given a name strictly in sense of work it does and it does sound crude but that is where it ends and it is a refined tool under the hood. I knew that it handles the rust builds and tests, but I was fairly surprised to see that it provides great support for documentation. 
All the function comments and examples shown with /// documentation comment()triple slash comment as I call it) can be viewed on HTML Pages(cool!) by running 
*cargo doc --open* and the cherry on top is that cargo test run the testing assertions provided with code examples via documentation comment.

```
/// Adds one to the number given.
///
/// # Examples
///
/// ```
/// let arg = 5;
/// let answer = my_crate::add_one(arg);
///
/// assert_eq!(6, answer);
/// ```
pub fn add_one(x: i32) -> i32 {
    x + 1
}
```

For Example, the above assertion in documentation to explain add_one function, is always checked when we run *cargo test* not only helps verify our documentation, but also helps us keep up to date.

## cargo workspaces
As a project gets larger in complexity, it feels nice to have the option to break it into smaller logical chunks and workspaces provide that ability to the developers. It felt similar(no exactly same, but similar) to creating a Solution and creating projects in .NET Framework. Additionally, the added advantage of having a single cargo.lock file to manage the dependency versioning and cargo automatically resolving if same create with different versions are needed for library/binary according to SemVer(I'm noticing it in lot of places, guess it already a standard huh). 

## cargo install
I've used ripgrep before, but didn't know that it was a Rust crate. With default configuration cargo places the create binaries in *$HOME/.cargo/bin* and once installed it can be called like any other utility(eg. ls) cargo binary path is in $PATH variable. It felt handy and I vaguely(not exactly sure) remember that I can do the same in Go.

## Smart Pointers-Box
Box is a Smart pointer. Firstly, why the name Box, is it because of the concept of boxing in java, where primitive types are boxed as object types? (or) is it because it is a simple name? Those who get the chance to name things have a lot of responcibility on them since it needs to be intuitive to others and have a logical meaning to it.

    There are only two hard things in Computer Science: cache invalidation and naming things.

        -- Phil Karlton

Okay, now back to the topic at hand, Boxes provide the ability to store the data on heap rather than on stack. From the documentation, I can see that Boxes clearly provide answers to some of the use cases that arise due to the way Rust compiler operates and some other use cases that provide performance benefit. (I referred to the concept Box as a plural, Boxes, while writing about it. Now that I see it, it is done with other concepts as well Pointers, variables , strings and data structures. huh. Why is that so?)

```
enum List {
    Cons(i32, List),
    Nil,
}
```

Consider the above code snippet. That does not work in Rust but something similar works in some OOP based languages. Is that List representation intuitive or the one below ? I wonder what would be intuitive to a Mathematician(assuming we can represent Box mathematically).

```
enum List {
    Cons(i32, Box<List>),
    Nil,
}
```

Since, Rust needs to know the size of types during compile time, we can represent recursive types using Smart pointers in Rust. It is not straight forward compared to other language but it seems more logical to me.

## Dereferencing operator(*)
Lets talk semantics first. Before, I go on about dereferencing, I thought to write define referencing in my own way for my mental model. A Pointer *references* to "some value". Applying *dereference* operator (*), which  is an inverse operator to what a *reference* does. Hence applying dereference on reference to "some value" gives us "some value" back.

We know that we can use dereference operator on pointers. However, we can also apply dereference operator on Smart pointers as well given that they implement Deref trait. 

In the code snippet below(Deref trait implementation on MyBox), the line  `impl<T> Deref for MyBox<T>` contains the reserved keyword for used not for writing for loop but in the context of Traits. To be continued after code snippet...
```
use std::ops::Deref;

impl<T> Deref for MyBox<T> {
    type Target = T;

    fn deref(&self) -> &Self::Target {
        &self.0
    }
}
```

The point I am trying to make here is that, are keywords used in different contexts to mean different things in other languages as well? I guess it is a good thing so that I need not remember many keywords and when an existing keyword makes the code readable. 

## Deref Coercion 
"Deref Coercion" , I guess the person who named it has done a great job of using exisiting words instead of going for a entirely new word(deref casting or deref morphing, kidding ofcourse) for naming this concept. Now, for the definition "Deref coercion converts a reference to a type that implements Deref trait into reference to another type" . So, Deref Coercion only deals with references unlike type casting which deals with types. The example given for this behaviour is that deref coercion can convert `&String` to `&str` because `String` implements the `Deref` trait such that it returns `&str`.

### a small detour
Lets take a detour: isn't `&str` a string slice holding reference to string literal type `str` stored on application binary, then how is that `&str` can also store a reference to `String`.Aren't they two different things, one is stored on application binary(str) and other is stored on heap. I mean at the end, they both represent Strings in general, so I guess I shouldn't feel that surprised to find &str can hold a reference a string irrespective of where it is stored. However, in the code snippet below, `slice1` is of type `&String`  and `slice2` is of type `&str` and both are valid. 

```
fn main() {
let s = String::from("hello");
let ref1: &String = &s;
let slice1: &str = &s;
println!("{ref1} {slice1}");
}
```

I guess this is what made me feel not so sure about the `&String` and `&str`. `&String` is simple, it is a pointer to a `String` variable's address on Stack. But what does `&str` do? On going back to the ([documentation](https://doc.rust-lang.org/book/ch04-03-slices.html#string-slices)), I've found string slice is reference to part of a string, ie., it points to String on heap. Now, it makes sense.

## Back to Deref Coercion
```
fn hello(name: &str) {
    println!("Hello, {name}!");
}

fn main() {
    let m = MyBox::new(String::from("Rust"));
    hello(&m);
}
```

In the code snippet above, &m which is `&MyBox<String>` was converted to `&str` through Deref coercision. `&MyBox<String>` -> `&String` -> `&str`. Since, these deref coercions are done at compile time these are zero cost as well. 

# Week 6
I was lagging behind in the exercise portion on [Rustlings](https://github.com/rust-lang/rustlings) compared to The Book, so I went back to the exercises. This time around, once I fixed the error I started taking a look at the solution so that I could learn another way(probably more elegant than my solution) of solving it. This is where I've started noticing implementations that are different in Rust.

## type casting

```
    let mut numbers:Vec<i16> = Vec::new();

    // Don't change the lines below.
    let n1: u8 = 42;
    numbers.push(n1.into());
    let n2: i8 = -1;
    numbers.push(n2.into());
```

Consider the code snippet above from generics1 exercise. since i16 type is allocated a size 16 bytes, it can hold smaller integer types i8 and u8 and these can be cast to i16. Now, what got me thinking were two things and the firt being that type casting was done using the `into()`([for more info](https://doc.rust-lang.org/rust-by-example/conversion/from_into.html)) method instead of automatic type casting to higher data types as in C++ and Java. Turns out, Rust does not support implicit type casting and I this helps developers be aware of any castings that are neeeded since they are manual now. `into()` is a method that needs to be implemented as part of `Into` trait and each conversion has its own implementation. In `n1.into()`, Rust infers that I am converting `u8` to `i32` and checks for `Into<u8> for i16` implementation. Rust supports explicit type casting using `as` operator, but this approach can cause truncation.

The second detail that got me thinking was the fact that primitive types have methods `n1.into()` or I could do `n1.mul(2)` or `n1.pow(2)`. Firstly, why do primitive types has methods available on them but I don't think Rust calls them primitive types, they are just types? In java `int` or `float` are called primitive types since they are not Object types.So, yeah `i32` could implement some traits and it can access those methods which it has implemented as part of those traits. However, `n1.pow(2)` seems different to me compared to other languages like C, Java. I mean, why not use `pow(n1,2)` and it turns out there were reason(s) behind it. Rust does not support function overloading(to avoid run time polymorphism?) and since there is no method overloading the alternative would have been `pow_i32` and so on for different types and which would have been not so elegant. 

## mut in trait method declarations
```
trait AppendBar {
    fn append_bar(self) -> Self;
}

impl AppendBar for Vec<String> {
    fn append_bar(mut self) -> Self {
        //        ^^^ this is important
        self.push(String::from("Bar"));
        self
    }
}
```

Again, from the Rustling exercise solution, I saw that the Trait implementation method had `mut` in the function definition while the trait method declaration didn't.

However, `mut` in trait method declaration is allowed for mutable borrows.
```
trait Modify {
    fn modify(&mut self);
}

struct Counter {
    value: i32,
}

impl Modify for Counter {
    fn modify(&mut self) {
        self.value += 1;
        println!("Counter is now: {}", self.value);
    }
}
```

Luckily, there was a [stack overflow post](https://stackoverflow.com/questions/66700509/why-is-mut-self-in-trait-implementation-allowed-when-trait-definition-only-uses) about the same question.

## iterators
just finished the iterator section of exercises in rustlings and I feel that Rust documenation is trying to nudge developers towards using iterators a bit more than the regular for/while loops since code looks consise.

## Rc<T>, the reference counted smart pointer

To represent a graph for view only purposes, we can use immutable references but we cannot edit the nodes while still having immutable references stored by other nodes. Most of the graphs represented via an adjacency list or a diagonal matrix so it wouldn't be much of an issue. However, I see problems implementing Trie data structure in Rust since it is hard if the linked nodes are read only. But, I don't think Rc<T> can solve our problem since they are read only references? If Rc<T> is immutable by the owners if so why not just use immutable references. Maybe I missed somethign here. Turns out someone had the same question after reading this section in documentation and it was ansered on [stack overflow](https://stackoverflow.com/questions/67747657/why-do-we-need-rct-when-immutable-references-can-do-the-job). Longstory short, if you do not want to deal with different lifetimes when using immutable references and if becomes even more complex when we have to pass them around. Okay, sold I need no more convicing. I just need to make sure that I do not overuse Rc<T> and keep using immutable references where it makes sense.

## Interior mutability pattern
It is a design pattern in Rust that allows you to mutate data even when there are immutable references to that data. It has become a habit to me to try and understand why the design pattern is named interior mutability. I understand mutability but not sure as to what 'interior' keyword has to do with mutating immutable references. Is it so because we are mutatiting values inside(interior) an immutable object. Now, back to the what it does. Okay, honestly I wished Rust didn't allow this and it could check everything at compile time but ofcourse a compiler cannot check all kinds of programs, hence the need to have some exceptions like this. I only wish  nothing more of this sort are added to the language in the future. I ask of it since with RefCell<T>, that follows interior mutability, the borrowing rules are checked during runtime. Things to remember about RefCell<T>, use borrow and borrow_mut to access the value stored inside it.

## Combining Rc<T> and RefCell<T>
This sounded similar to a game combo move, you can jump and you can dash, now jump and dash at once. It was used this way , `Rc<RefCell<i32>>`, to have multiple owners to i32 using Rc<T> and all of them can modify the value through RefCell<T> even though Rc<T> is immuatable. Although, we need to make sure that we are still adhering to the borrow checker rules since it is still enforced at runtime.Now, What happens when I create a type by interchanging the before to create a type `RefCell<Rc<i32>>`, now what does it do? Huh, turns out the next section of The book does use `RefCell<Rc<T>>` to demonstrate memory leak by creating cycles using linked lists. 
```
enum List {
    Cons(i32, RefCell<Rc<List>>),
    Nil,
}

impl List {
    fn tail(&self) -> Option<&RefCell<Rc<List>>> {
        match self {
            Cons(_, item) => Some(item),
            Nil => None,
        }
    }
}

fn main() {
    let a = Rc::new(Cons(5, RefCell::new(Rc::new(Nil))));

    println!("a initial rc count = {}", Rc::strong_count(&a));
    println!("a next item = {:?}", a.tail());

    let b = Rc::new(Cons(10, RefCell::new(Rc::clone(&a))));

    println!("a rc count after b creation = {}", Rc::strong_count(&a));
    println!("b initial rc count = {}", Rc::strong_count(&b));
    println!("b next item = {:?}", b.tail());

    if let Some(link) = a.tail() {
        *link.borrow_mut() = Rc::clone(&b);
    }

    println!("b rc count after changing a = {}", Rc::strong_count(&b));
    println!("a rc count after changing a = {}", Rc::strong_count(&a));

    // Uncomment the next line to see that we have a cycle;
    // it will overflow the stack
    // println!("a next item = {:?}", a.tail());
}
```
To create a cycle we need to create the lists first and then modify them their tail to one another. To do that, we need to modify the tail node from nil to point to another node.Had I used just Rc<List> instead of RefCell<Rc<List>> I wouldn't be able to do so. On a tangent, why do we call it a memory leak if the memory allocated is not deallocated properly, why not memory freeze since after it is allocated it does not get deallocated while the program still runs? 


## Weak<T>
Some times we need cycles but using Rc<T> is causing memory leaks since the strong count does not go to 0, so Rust offers a solution though Weak<T> which are created by passing a reference to Rc<T> to the Rc::downgrade() method. Calling Rc::downgrade on Rc<T> increasee the weak_count on that Rc<T> by 1 and weak_count need not be 0 for an instance of Rc<T> to be deallocated. Since, Rc<T> is deallocated even if there are some Weak<T> pointing to it, Rust returns it wrapped in Option<Rc<T>> when we try to access the value Weak<T> is pointing to via upgrade method.

In the quiz reveiw invloving Rc<T> and Weak<T>, I felt it was a good code snippet for grasping the concepts and thinking them through.

```
use std::rc::Rc;
fn main() {
    let r1 = Rc::new(0);
    let r4 = {
        let r2 = Rc::clone(&r1);
        Rc::downgrade(&r2)
    };
    let r5 = Rc::clone(&r1);
    let r6 = r4.upgrade();
    println!("{} {}", Rc::strong_count(&r1), Rc::weak_count(&r1));
}
```

In the above code snippet, we knwo the r1 creates a Rc<i32> and it increase the strong_count to 1. Next, inside the expression that needs to be evaluated to get the result for r4, we are creating r2 and it points to r1 so the strong count to Rc that r1 points to increases by 1 making it to 2. Then we create a weak refernce to Rc<T> pointed by by r1 and r2 and store it in r4 while increasing the weak_count to 1. Since r2 is out of scope it is dropped and the strong_count decrease by 1 making it 1(pointed by 1) and weak_count as 1(pointed by r4). Next, we create another reference and store it in r5 which increases the strong_count to 2(r1 and r5). Next, we get the value pointed by weak_pointer r4 and store it in r6. Since r4 points to Rc<T> that is pointed by r1 and r5 and it still exists since r1 and r5 havent been dropped yet since r1 is used in the print, we return Rc<T> and r6 becomes another owner along side r1 and r5 while increasing the strong_count to 3.

# Week 7
This week, I dive into the concurrency aspect of Rust. I have worked with threads on Go, so I am excited to see how Rust's threads and concurrency handling compares to Go. Maybe I learn a new way style programming that Rust uses for concurrency. Threads can maintain a shared state either by sharing messages(distributed system style, channels in Go) or by sharing memory(share variables and use mutex locks while doing it). I prefer the former, due to its clean approach and fewer chances of race conditions. Ofcourse, one sharing messages might not solve all the problems, but let me see.

## Fearless Concurrency
I have seen the title of the section long ago and been meaning to get to it. Now, that I've reached I am curious as to how Rust encourages concurrency and how many classes of runtime error can it catch during the compile time with its design. 

This section started with the familiar concepts of spawning a thread with thread::spawn and using a handler returned by spawn method to wait on it (handler.join().unwrap()) as needed.

In the below, code snippet when asked about whether the program would compile?
```

use std::thread;
fn main() {
    let mut n = 1;
    let t = thread::spawn(move || {
        n = n + 1;
        thread::spawn(move || {
            n = n + 1;
        })
    });
    n = n + 1;
    t.join().unwrap().join().unwrap();
    println!("{n}");
}
```

I was thinking that it would not since the main program is trying to increment the value of n even after n was moved by the thread. However, I forgot the fact the move on primitive types would copy the value and n would still be valid to use in main. So, Rust decides to Copy a value if it implements the Copy trait and if it does not it moves the ownership to a different identifier.

So, I went back to Copy and Clone section in the book to re-read the definition as to what Copy and Clone trait would do. `Copy` trait allows you to duplicate a value by only copying bits stored on the stack. `Clone` trait allows you to explicitly create a deep copy of a value and since it is a deep copy, we do not simple copy the memory references, instead we duplicate the value that the reference is pointing to and we stored the reference to the copied value in the clone. From the book, it points that we can derive `Copy` on any that whose parts all implement `Copy`. Similary, we can derice `Clone` on any that only if all of its parts support `Clone`.Finally, a type that implements `Copy` must also implement `Clone`, because a type that implements `Copy` has trivial implementation of `Clone` that performs the same task as `Copy`. Since, `Clone` involves deep copy it involves running arbitrary code making it slower that `Copy`.