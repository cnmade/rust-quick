# 字符串

Rust has two kinds of string types.

```rust

fn main() {
    
// Two types of string representation:

    
// - string literals: hard coded into the executable.
    
// these are immutable and must be known before compilation

    
// - String type: allocated data on the heap, \n\tmutable and dynamically generated at runtime 
// string literal stored on heap 
// String::from() creates a String type from a string literal 
// the sequence [m,a,r,s] will get stored on the heap 
// to access the string stored on heap, program holds a pointer to it on the stack (message variable) 
// that pointer on the stack includes first char memory address, length of string and the capacity so you know how much memory s allocated for it on the heap 
let mut message = String::from("Jupiter");
    println!("message is {}", message); 
// append string to original 
// if more memory need than capacity, pointer address updated as well as length and capacity to reflect new location in memory
 message.push_str(" is smoke and mirrors");
    println!("message is {}", message); 
// pushing a char
    message.push('!');
    println!("message is {}", message); 
// get length
   println!("message lenght is {}", message.len()); 
// get capacity in bytes
   println!("message capacity is {}", message.capacity()); 
// check if empty
  println!("Is empty: {}", message.is_empty()); 
// substring search
  println!("Contains smoke: {}", message.contains("smoke")); 
// replace substring
  println!("message is {}", message.replace("smoke","gaz")); 
// loop over words in string (split by white space)
  for word in message.split_whitespace() {
    println!("word is {}", word);
  } 
// create string with capacity
 let mut s = String::with_capacity(4); 
// 4 bytes capacity
  println!("s capacity is  {} bytes", s.capacity()); 
// 1 byte consumed
  
// Latin alphabet letters usually have 1 byte size
  
// remember Unicode supports 4-byte characters
  s.push('Q'); s.push('W'); 
// 1 byte consumed
  s.push_str("er"); 
// 2 bytes consumed 
// exceeding string capacity (automagically increased and reallocation in memory) 
s.push('T'); 
// 1 byte consumed
  println!("s capacity is  now {} bytes", s.capacity());
}
```
输出
```
message is Jupiter
message is Jupiter is smoke and mirrors
message is Jupiter is smoke and mirrors!
message lenght is 29
message capacity is 56
Is empty: false
Contains smoke: true
message is Jupiter is gaz and mirrors!
word is Jupiter
word is is
word is smoke
word is and
word is mirrors!
s capacity is  4 bytes
s capacity is  now 8 bytes
```