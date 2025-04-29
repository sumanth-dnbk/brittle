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
For some reason, tuple struct always seem to throw me off. I understand the reason for creating tuple struct and its adavantages but the syntax somehow feels different. 

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
While reading through the trait documentation, I understood all the code but forgot what trait bound mean in terms of code. Later, I found it and thought of documenting for my refernce. Trait bound in Rust are a way to specify that generic type must implement a paritucal triat(s) in order to be used. 

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

    Diverging a bit from the testing topic, I've come across the word 'annotation' in multiple programming languages and I remember it as something  I add on top of a method or property as such to indicate the method is different or is to be interpreted differently. The English dictionary meaning of it being "a note of explanation or comment added to a text or diagram".  

Getting back on the testing chain of thought, next were integration tests. As far as I've known, I considered integration tests as testing as a whole but couldn't give a better explanation than that. After reading the Rust docs , my mental model for Integration tests has been made more concise to state, Integration tests call your APIs(in library or server) as if called by a client. Rust suggests to put them in tests directory and it made sense so as to keep them separately and interact with them as if  using a client library. However, for some reason each file under tests directory is considered as a separate crate. Since, setup is common for all integration tests, Rust considers code under common directory(is it just common directory or any directory under tests?) as one whole module??

Since, we cannot technically run integration tests on binary crates since it is an exe and we cannot call it just like a library, we create binary crates with src/main.rs file and src/lib.rs. src/main.rs calls the functionality from src/lib.rs which makes it integration testable like a library .

## closures
as soon as I hear the word closures, it reminds me of some concept in Mathematics that seems arcane to me. That asided, I have heard of closures before but did not try to see what they really are about. I vaguely remember doing so in Go, when threads used closures. Now, that I am reading Rust documentation, I plan to build on my previous knowledge and add some detailed knowledge about closures to my programming mental notes. 

Firstly, I've observed closures do not have a function name, so does this mean closures are same as anonmymous functions. Does Rust even support anonymous functions. Yes, it does and according to The Book, "Rust's closures are anonymous functions you can save in a variable and pass an argument to other functions.". Additionally, the buzz around closures is that they can capture variables in the environment it is declared and since Rust has the concept of ownership, it check whether the variables are borrowed mutably, immutably or entirely moved(as in case of threads and other contenxt). Next, based on how a closure handles the variable ownership a closure is said to implement one or more of the traits Fn, FnOnce and FnMut. 

Besides, all of these I wonder why Rust developers decided to use '||' to capture arguments for a closure. That seems different than any other programming languages , but then again Rust is in a class of its own languages or to be precise one of a kind!

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