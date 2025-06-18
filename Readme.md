# Rust Learning Journey

Learning a new programming languages is always exciting with a satisfaction of learning something new and a possibility that this could be the one to rule them all. I started my programming journey with C although exciting at first the pointers and dereferences bought it to a grinding halt and made it daunting to go back to it. Then came java with the rumor that I do not have to see references or pointer while writing code which the naive programmer in me believed and was true to a part that I did not have to dereference a pointer although object had pointers and they pointed to the same value in memory. However, not having to deal with pointers made Java a true safe haven for me and I went in for the ride. Later, I've learned C# which I've heard as a "Brother to Java" and I felt the same so I did not feel like I've learned a new language. Then there was Go back with the concept of pointer, but I have been acquainted with programming so it did not seem so daunting to me this time and I cherished the concept of channels and all the features it bought but not so overwhelming that it was possible to read the Go language spec in few days. While I was sailing through the programming seas I have been hearing the chatter on Rust albeit having a learning curve even after learning a few languages made life easier once you embraced it. One day I serendipitously landed on Rust one page guide https://github.com/amartincolby/Rust-Quick-Guide with a promise of teaching Rust by relating the features to other programming languages whilst still being grounded. 

## Week 1: 
I started reading it since I did not feeling like working on other things and I was dragged into it due to the poetic intro to the the Rust quick guide that enfused me lot of enthusiasm that was needed to see it through to the end of the guide. I read it like a guide that gives provide info the new syntax whilst using the context from existing languages. On the re read, I felt the fog being lifted and me being able to see clearly through the seas. Continuing with the sea analogy at the risk of overusing it, I did not know how to navigate the seas at that point. May be another re-read could've made it much more clearer but like a kid who has lost interest in a toy after playing with it for a while, I was in search of some new guide to charter much deep into the Rust seas. That was when I've stumbled upon https://rust-book.cs.brown.edu/. Being lauded by many Rustaceans as a good learning material and with the site having quiz at the end of each concept serving to reinforce the concept learned by thinking/ solving it but also as a feedback to the site maitainers so as to make the modify content to make the concept more approachable if many participants were getting the quiz qestions wrong.

## Week 2:
Chapter 2 of the rust-book site had a small programming project that gave hands to the concepts that we are about to cover. At this point already have a understanding on Rust by having read through the quick-guide, I finished the programming assigned as it gave a chance to write code my self that meant having to remember the syntax and concepts back(which I feel is the fundamental part of any learning journy since we are trying to remeber what we've learned and also write code with it that reinforces the concepts.No wonder working on projects is the best way to learn just like solving a Math question,  you do not truly understand the concept  no matter how many times you read the solution until you engage with it when you try to solve it.) 

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

Other thing, I am noticing on re-read or might not have paid enough attention during the first read is str is not stored on the stack, &str(string slice) is. The reason for this being, rust needs to know the size of data it stores on stack so that it can alloccate exactly that amount of space. However str is a dynamic sized type and rust cannot decide the size of string literal that is going to be stored in str , rust stores the value in str(ie., a string literal present in the code) on read only memory of the binary and the pointer to the read only memory and its size is stored in &str , which is in turn stored on stack. Since, rust knows the size of &str (string slice type) type is an address locaiton and the size of str literal it can allocate exactly that amount of space on stack. String is stored on stack, since String type stores address to value of string stored on heap. Since we are storing an address value, rust knows how much size that String type needs on stack(size of an addresss locaiton on heap).


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

## message passing between threads
Here Rust uses channels for message passing and quotes the famous line “Do not communicate by sharing memory; instead, share memory by communicating.” from Go. This makes me wonder whether channels in Rust are inspired from Go. When I started looking into it, I now remember about communicating sequential processes(CSP) which is a formal language for describing patterns of interactions in concurrent systems. Message passing via channels for communication is one of its core ideas. However, on thing that is different from Go is the distinctions in channels such as mpsc(multiple producer and single consumer). Adding ownership to channels, once we send a value in a channel the owner ship is transferred to the receiver(move operator) which makes it more like a real world letter, once you send it you no longer have it.

## Shared state concurrency
Shared memory concurrency is like multiple ownership since multiple threads can accesss same memory location at the same time. Since, Rust already is good at handling multiple ownership it should be good at handling it across threads. They a good logical point in The book's intro paragraph. Rust has `Mutex<T>` (the concept of mutual exclusion). Mutex<T> is a smart pointer , which on call to `lock` returns a smartpointer called `MutexGaurd` wrapped inside `LockResult`. `MutexGaurd` implements `Deref` to point to the data gaurded by `Mutex<T>` and as an additional bonus, once `MutexGaurd` goes out of scope the lock is released automatically so no need to worry about releasing the lock. Less chances to create bugs. Additionally, `Mutex<T>` implements interior mutability allowing us to mutate the value inside `Mutex<T>` even though the its identifier itself is not mutable.

## Send and Sync
Wow, and here comes few more traits to the list. I have been reading a lot about the traits in the last few sections. Looks like Rust uses traits a lot! `Send` marker indicates that ownership of values of the type implementing `Send` can be transferred between threeads. Since, `Rc<T>` did not implement `Send` we couldn't use it across multiple threads. With the help of `Send` trait Rust keeps track that it is only sending types that can be used in multi-threaded scenarios across the threads. 

Only the types implementing `Sync` can be references by multiple-threads, meaning the type is thread safe. `T` is `Sync` if `&T` is `Send`. ok, ok, a lot to digest.
Primitive types implement both `Send` and `Sync`. Additionally, types entirely composed of types that are `Sync` are also `Sync`. This is similar to `Copy` trait, if all the types a particular type is composed of implement `Copy` then that particalur type can as implement `Copy`.

## Concurrency
Defining concurrency and parallellism with an example, just like in the OS class. Speaking of the definition of concurrency and parallelism, that I often hear "I've been working on it parallelly", but after how could they, unless the tasks are scribbling with left and right hand it can't be parallel.. The right phrase should have been "I've been working on it concurrently", I will try this the next time. Additionally, the documentation, draws parallels between parellism(threads) and concurrency(async/await) which I felt was very well put.

## Futures
Rust has `Futures` trait that is implemented by types the return `async`. Drawing paralles Futures is to Rust, what Promises is to JavaScript and Task is to C#. However, one thing that is different in Rust is that calling an async function does not trigger it, unless we add await chaining to it. Additionally, I've found that we need a runtime to run a async function , which might have been the case in JS and C# but I might not have known since I did not dig deep into the docs for those. Next, in an async function whenever we have an await it hands the control back to the runtime and once the event returns the control then the runtime resumes the execution. Since the control switches between runtime and a specific async function, the state is stored during the context switch , which is handled automatically.

```
1.        let fut1 = async {
2.             for i in 1..10 {
3.                 println!("hi number {i} from the first task!");
4.                 trpl::sleep(Duration::from_millis(500)).await;
5.             }
6.         };
7. 
8.         let fut2 = async {
9.             for i in 1..5 {
10.                 println!("hi number {i} from the second task!");
11.                 trpl::sleep(Duration::from_millis(500)).await;
12.             }
13.         };
14. 
15.         trpl::join(fut1, fut2).await;
```

Consider the code snippet above, If I remove the `await` chaining at the end of line 4 and 11, the code would run the entire `fut1` first and then move on the `fut2`, since without await the control is never passed to runtime until `fut1` is completed. 


```
1.        let fut1 = async {
2.             for i in 1..10 {
3.                 println!("hi number {i} from the first task!");
4.                 trpl::sleep(Duration::from_millis(500)).await;
5.             }
6.         };
7. 
8.         let fut2 = async {
9.             for i in 1..5 {
10.                 println!("hi number {i} from the second task!");
11.                 trpl::sleep(Duration::from_millis(500)).await;
12.             }
13.         }.await;
14.         fut1.await;
```

In the code snippet above, fut1 would not start executing even if it was declared first and fut2 would finish first since we added await in line 13 and then fut1 would start and finish.

# Week 8
I see that I am almost approaching the 2 month mark. It has been a good learning journey on my Rusty boat. With no destination ahead just enjoying the journey for now.

## Working with multiple futures
The title of the section feels like someting that is said in a scifi movie. Looks like it is not as easy as I thought it would be for working with multiple(dynamic number) futures. We have to add the `pin!` macro and the vector that holds these futures is of the type `Vec<Pin<&mut dyn Future<Output=()>>>` while working with `join_all` which requires all the futures return same type Output. And then we have the ability to yeild control to the runtimeusing `trpl::yield_now` other than `trpl::sleep`. And then we can use `trpl::race` to implement the `timeout` utility block which is great. I tried to recollect `trpl::race` that I read in the previous section. This took me on a detour about remembering and recollecting. I do not know for sure that I remember something until I am able to successfull recall it. 

So, far we have tried explore the functionalities with futures which are similar to what we've explored in threads. Drawing parallels b/w parallelism and concurrency.

## streams

stream of data that can arrive at any point with any amount of delays in between them, is what I remember when I try to recollect about streams. In Rust these are designed to be similar to iterators with similar adapter since an iterator and stream are similar except that data in stream arrives asynchronously and data in iterators arrive synchronously. Then we have `timeout` adapter method over streams that will wait for a certian while to read a single event from the stream and returns an Err if an event does not arrive within that time. This was similar to `timeout` method that was designed for future events in the last section. Next, in the `get_messages` method in section 17-35, we wanted to add sleep and await on it but did not want to make get_messages async, because if we make `get_messages` async we have to `await` on it and when the result is return the stream is similar to an iterator since all the results are available. Instead, we use trpl::spawn_task similar to trpl::run in main method without making main async. What I've felt different was that we called get_messages method in line 10 below and created a stream out of it. Here get_messages is a sync method as it spawn a task and returns the receiver channel as a stream. Why do we wrap the output from timeout in line 10 inside a pin, is it because it is a future? 

```
1.  extern crate trpl; // required for mdbook test
2.  
3.  use std::{pin::pin, time::Duration};
4.  
5.  use trpl::{ReceiverStream, Stream, StreamExt};
6.  
7.  fn main() {
8.      trpl::run(async {
9.          let mut messages =
10.             pin!(get_messages().timeout(Duration::from_millis(200)));
11. 
12.         while let Some(result) = messages.next().await {
13.             match result {
14.                 Ok(message) => println!("{message}"),
15.                 Err(reason) => eprintln!("Problem: {reason:?}"),
16.             }
17.         }
18.     })
19. }
20. 
21. fn get_messages() -> impl Stream<Item = String> {
22.     let (tx, rx) = trpl::channel();
23. 
24.     trpl::spawn_task(async move {
25.         let messages = ["a", "b", "c", "d", "e", "f", "g", "h", "i", "j"];
26.         for (index, message) in messages.into_iter().enumerate() {
27.             let time_to_sleep = if index % 2 == 0 { 100 } else { 300 };
28.             trpl::sleep(Duration::from_millis(time_to_sleep)).await;
29. 
30.             tx.send(format!("Message: '{message}'")).unwrap();
31.         }
32.     });
33. 
34.     ReceiverStream::new(rx)
35. }
```

Next, this section delves in to merging streams , throttling them and limiting the number of items we accept from a stream using `take` method.

#  Week 10
Took a break during week 9 and back on week 10.

## Deeper dive in to Futures, Pin and Stream
This section was too dense especially the section about Pin. So, in summary Future either have the result ready or Pending. If the status is pending the control goes back to runtime and if it is ready the control is given back to async block. Runtime handles the check for whether it is ready or pending.

Now, Pin was introduced as a means of pinning the underlying data at a specific memory location since Futures have self referential properties(not exactly sure as to what these properties might be) and managing ownership. 

Finally, Steams is an iterator on Futures so we combine both for Stream definitions.

Traits, Traits,  Traits! In Rust, I feel like I am looking at lots of traits and having to remember what each trait specifically does.

## Concluding the concurrency using threads and tasks
tasks are well suited for io/network bounded operations where we do not know when the response might arrive and threads are well suited for compute bound tasks that can be sliced into mulitple parts without dependency on one another. On some hardware that do not have OS, tasks provide a way for concurrency. Moreover, they can also be  used together.

## Rust OOPS
I know that there is Object oriented programming paradigm and then there is functional programming pardigm. Object oriented design seems intuitive since implementation(using Object oriented programming languages) most of the times resembles very close to the design. Even the the languages that aren't object oriented like C , the design of the project resembles to that of Object oriented programming with entities and some relations between them. Even Go and Rust with their structures and methods resemble close to a OOP languages, however not all concepts are available like inheritance. I've felt that OOP design gets bit messy on larger projects but thats just my opinion and I haven't worked on projects that haven't used OOP design on larger scale.

## Trait objects
traits refer to behaviors and Rust does not have interfaces but traits ie., structures and their trait implementations add behaviours to structuures. Now, we come across trait objects, does that mean objects that implement traits, that was my layman interpretation of adding one plus one, but it not what I've expected. From the definition of trait object it is **"A trait object points to both an instance of a type implementing our specified trait and a table used to lookup trait methods on that type at run time."** I understand the first half of the definition that specifies that the trait object points to an instance implmenting specified trait but I am not sure as to why the trait object also points to a table used to lookup trait methods. And next, **" We create trait objects by specifying some sort of pointer, such as an & reference or a Box<T> smart pointer, then the dyn keyword , and then specifying the relevant trait."**. Not a great fan of the trait object syntax, but I am sure people has reasoned a lot and finally come to a conclusion to reach at this syntax, so I will suffer in silence for now :) . 

If I take a step back and think about the reasoning as to why the concept of trait object was needed in the first place, it uses the use case for gui library developers. The client users can create different variations of gui objects that cannot be known at the time of creating the library and the libary needs to call some methods on those custom gui classes created by client users to providing the relevant gui output. With OOP based languages, I could think of doing the same by providing client users an interface that they have to implement on their gui objects and send the send the newly created gui object to the gui library, where we call call the method as an interface object using `Interface interfaceObject = new ObjectImplementingInterface()` syntax or through inheritance where gui library provides a parent or base class which gui library clients inherit into their custom gui classes through this syntanx `Parent parent = new Child();` which I didn't know was called upcasting. Ater a search, I came to know that both are concepts are called upcasting and it comes under polymorphism(which provides great advantage whiel developing the gui library we are using for discussion).

```
//java interface + upcasting
interface Flyer {
    void fly();
}

class Bird implements Flyer {
    public void fly() {
        System.out.println("Bird flying");
    }
}

Flyer f = new Bird();
f.fly();

```


 Rust cannot implement the above two solutions, but I immediately, though of Triat bounds but trait bounds vectors that can store multiple objects can only store struct or enums of single type in a single vector ie., if first object is of type Button, then second has to be of type button as well. This was explained earlier in Rust where it monomorphizes the generic code form vec<T: guiObject> to vec<Button> during compile type and there are zero cost abstractions which I've praised. so, I cannot use that vector to capture multiple gui classes implemented by gui library clients although all the custom gui classes implement the common trait. Finally, that is why Rust suggests to use trait objects.

```
// Rust trait object
trait Animal {
    fn speak(&self);
}

struct Dog;
impl Animal for Dog {
    fn speak(&self) {
        println!("Woof!");
    }
}

fn make_it_speak(animal: &dyn Animal) {
    animal.speak();
}

fn main() {
    let dog = Dog;
    make_it_speak(&dog);
}

```

In later parts of this section, I came across duck typing, some dev or author who knows how to elicit interest through great fun naming. So, I went ahead and searched around a bit as to what this meant and in dynamically typed languages like Python if an object implements certain methods we can consider it similar. ie., if it walks like a duck and quacks like a duck then it is a duck. ie., if the object has methods needed by gui class then it is a gui class. And agian this makes it easier while developing libraries similar to the gui libary under discussion. 

```
# python duck typing
class Bird:
    def fly(self):
        print("Bird flying")

def make_it_fly(thing):
    thing.fly()  # No type check

b = Bird()
make_it_fly(b)

```

A junior developer writes code and a senior developer writes code but a senior dev developes libraries and a junior dev develops applications using those libraries. This is something that I've felt after reading this section on using trait objects while developing gui library and my statement might not stand true in all the case but is true in most.

Now, coming back to the Rust world, ofcouse compiler cannot do type inference incase of triat objects which is an inconvenience. Another performance hit would be due to usage of `dynamic dispatch` for trait objects. The reasoning I got from search was that, if we consider a vector of trait objects each type has different implementation of the the trait method so the compiler in runtime has to check the type of the instance and then call the methods of that instance which obiously incurs a runtime cost.

# Week 11
I have to admit the enthusiasm started to fade, but I am much wiser now.

## object oriented design pattern
This section was more about implmenting State pattern in Rust as if in a OOP language and then switching gears a bit an using Rust's features to implement state pattern in a better way that looks like Rust code. This is to say that Rust can implement most of the patterns but if you embrace Rust you can do better.And then came the dreaded ownership inventory. Too much dependent on lifetime annotations and I forgot what `+ a` meant, where a is lifetime annotation. I shouldn't even get started about design trade offs that is in a different level humbling me to the threshold gap between current knowledge of Rust and the one required for libraries in Rust.

## Pattern matching
irrefutable (impossible to deny) and refutable patterns. `let` and `for` declarations need irrefutable patterns because they cannot handle the else case by them selves. if we think it might be refutable the go with `if let` or `while let` expressions or `let else` statement(yeah, one is an expression and other is a statement(recollected it hence I remember it!)).

## matching literals
seen the title, I was trying to recollect what a literal was and couldn't exactly place it. I had a fuzzy definition in mind, now I remember it as literal values(literals are literally nothing but values).

```
    let x = 1;

    match x {
        1 => println!("one"),
        2 => println!("two"),
        3 => println!("three"),
        _ => println!("anything"),
    }

```

## moving on with pattern matching
I have seen the `..=` range operator before and understood it but this is where Rust book formally defined it. Inclusive range of values 1 to 5 is defined as 1..=5. Later, we see destructuring structs and enums and Javascript and Go support similar destructuing syntax. 

`_` is a wildcard pattern that will match any value but not bind to any value. Okay, where did the phrase wildcard pattern emerge from and if it is a wildcard isn't it supposed to be unpredictable and not match with anything, huh. For any wondering where did that phrase came to be, it has its origins from card games(ofcourse ) where it can represent any card and it was borrowed to computing to describe special characters like (* and ?). One point to remember is that values in variable do not get moved in to `_` so we can use the variable again since it is still the owner. 

And then the nameless pattern `..`, because I couldn't see a name for that syntax. Maybe it was defined elsewhere but I did not find it here. This could be the first nameless syntax or concept I am encountering. For all my mania with naming, I found the nameless. I call it the so on syntax, kidding ofcourse. 

And then we have `match guards` to guard the match expression's arms. So it gaurds the arm then why not call it arm guard, may is it because some thing else has an arm?? Guarded arms(Some(x) if x % 2 == 0) don't count towards exhaustiveness and so while checking for exhaustiveness Rust only considers the arms without match guards. I am almost certain that the reasoning behind it being that compiler cannot check the exhaustiveness of the guarded arm ie., how much of the range of values for the variable we are trying to match is being covered based on conditional that would be hard to calculate. 

Finally, we get the `@ bindings` that let us create a variable that holds a value at the same time lets us test that value for a pattern match. The below is the arm that uses `@ bindings`

```
  Message::Hello {
            id: id_variable @ 3..=7,
        } => println!("Found an id in range: {id_variable}"),
```

and its equivalent using `match guards`

```
  Message::Hello {
            id: id_variable,
        } if id_variable>=3&& id_variable<=7  => println!("Found an id in range: {id_variable}"),
```

I don't know, maybe there exists a better use case for `@ bidings` or I am missing something here.

## unsafe Rust
For Rust to be a systems language it requires certain features where Rust cannot gurantee the safety of the program, so it is left to developer in such cases to weild that great power very responsibly,  and the Rust book calls them the SuperPowers. 

Firstly, we have raw pointers, the kind of pointers we have in C, where we can specify a memory location. They can point to invalid memory, be null and don't implement automatic cleanup. Similar to references they can mutable or immutable and are written as `*const T` and `*mut T` and * is part of the syntax and not the dereference operator. Just like with any superpower, you need to careful only while we use it , we need only need to put the parts of the code that derefernce the raw pointer to read the data inside it inside the `unsafe` block.

```
    let mut num = 5;

    let r1 = &raw const num;
    let r2 = &raw mut num;

    unsafe {
        println!("r1 is: {}", *r1);
        println!("r2 is: {}", *r2);
    }

```

Next, we have `unsafe` functions, which have unsafe prepended to the function definition and while calling this function, we assure Rust that we know what we are doing and also keep the function call inside a unsafe block.

Then, we have `extern` Functions to call external code, which through the use of `extern` keyword allow creation and use of a *Foreign Function Interface(FFI)* . This is cool and I haven't got to using this in any code that I've developed and awaiting for that day. 

Then, we have static variable, which are known as global variables in other lanugages is unsafe in Rust when they are mutable since usage across multiple thread could case race condition, hence modifying it should be done in an unsafe block.

Then we have unsafe traits which is to assure Rust that they are safe and implement Send and Sync marker traits when they have raw pointers in them.

Finally, we have unions as Rust mentions it , these are primarliy available to interface with unions in C.

# Advanced Traits
With the number of traits already covered the advanced ones might have already been done, but I was wrong and we do have some more information about traits left to covers. Okay, I will read it for the sake of completeness.

# Associated Types
I understand that they have a subtle and nuanced differences with using Generics over traits, I really can't see why they are needed. I understand that in case of generics over Iterator example specified, we'd have to specify the output type when we call the next method which wouldn't be needed when used via Associated types? From the defintion in the book, *In Listing 20-13 with the definition(of Iterator) that uses associated types, we can choose what the type of Item will be only once, because there can be only one impl Iterator for Counter. We don’t have to specify that we want an iterator of u32 values everywhere that we call next on Counter.* I wish there was a code example to make it clearer to understand.

```
// Listing 20-13
pub trait Iterator {
    type Item;

    fn next(&mut self) -> Option<Self::Item>;
}
```

It's implementation on Counter

```
struct Counter {
    count: u32,
}

impl Counter {
    fn new() -> Counter {
        Counter { count: 0 }
    }
}

impl Iterator for Counter {
    type Item = u32;

    fn next(&mut self) -> Option<Self::Item> {
        // --snip--
        if self.count < 5 {
            self.count += 1;
            Some(self.count)
        } else {
            None
        }
    }
}

```

# Default Generic Type Parameters and Operator Overloading
As the title suggests it is about declaring defaults to Generic type definitions and the other ting is operator overloading. I have to say that, I have read about  operator overloading in C++ and had to write a program once for a programming lab and this is it. I haven't had to implement it after that and maybe I might have used the overload by being blissfully ignorant, but never had to implement it again. 

This is the example of overloading + plus operator in Rust and ofcourse it uses traits to do so. Looks like Traits are part of the building blocks in Rust.

```
use std::ops::Add;

#[derive(Debug, Copy, Clone, PartialEq)]
struct Point {
    x: i32,
    y: i32,
}

impl Add for Point {
    type Output = Point;

    fn add(self, other: Point) -> Point {
        Point {
            x: self.x + other.x,
            y: self.y + other.y,
        }
    }
}

fn main() {
    assert_eq!(
        Point { x: 1, y: 0 } + Point { x: 2, y: 3 },
        Point { x: 3, y: 3 }
    );
}
```

From the definition of Add trait, we could see that RHS operand type was defaulted to same as LHS, using default generic Type Syntax. 

```
trait Add<Rhs=Self> {
    type Output;

    fn add(self, rhs: Rhs) -> Self::Output;
}
```

This in turn means we could specify a different type for RHS and add two different types together.

```
use std::ops::Add;

struct Millimeters(u32);
struct Meters(u32);

impl Add<Meters> for Millimeters {
    type Output = Millimeters;

    fn add(self, other: Meters) -> Millimeters {
        Millimeters(self.0 + (other.0 * 1000))
    }
}
```

# disambiguating between same methods and same associated types in traits and implmenttations

jogging my memory: In Rust, associated functions are functions that are associated with a type (like a struct, enum, or trait) but do not operate on an instance of that type, ie., if there is no self as first parameter then it is an associated function, eg. new function for creating instances.
If Human type implemts Pilot and wizard trait which contain fly method while also having a fly method of its own, this is how we disambiguate them. Using the assocaited method syntax to let the compiler know which method we want to call.
```
fn main() {
    let person = Human;
    Pilot::fly(&person);
    Wizard::fly(&person);
    person.fly();
}

```

For associated functions it is 

```
<Type as Trait>::function(receiver_if_method, next_arg, ...);
```

# Supertraits and NewType pattern
A trait Y implementing another trait X and the parent trait X is called super trait. For a type to implement the trait Y, it also needs to implement X. 
New Type pattern to implment trait on a type where both an not defined in the package, then we use this pattern.

# Advanced Types

In this section, we start with New Type pattern right of the bat. I felt like there should have been atleast some separation between the sections if they were going to reintroduce it back or they could have finished everything in a single go, but this makes sense in case of someone solely refering back to this section. That aside, it was a easy read, turns out NewType pattern is similar to the concept of abstraction and this is called NewType Pattern. Not sure why, why couldn't we just call it abstraction and leave it at that, is it that since abstraction is an oop concept, Rust designers wanted to provide some way to distinguish it since there are some subtle differences?

And then we move into Dynamically Sized Types which made worry a bit, since it felt that allowing such types Rust is showing some cracks that would make it not so perfect anymore, but it is about the str type. Since, strings can be variable sized based on their context, str needs to be a DST, however Rust does not let us use str but &str ie., through a pointer to str along with its size. As &str has a definite size(size of two u32s), we can use it to write code. Rust prevents us from using DST alone by themselves and only allows us to use them through a reference or through Smart Pointer types.