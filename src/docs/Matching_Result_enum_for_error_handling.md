# 匹配Result<T,E>枚举错误处理

```rust
use std::fs;
use std::io;
fn main() {
    let result = fs::read_to_string("does-not-exist.txt"); 
    let content = match result {
        Ok(text) => text,  
        //根据类型，处理 I/O 错误
        Err(error) => match error.kind() {
            io::ErrorKind::NotFound => String::from("File not found"), 
            io::ErrorKind::PermissionDenied => {
              String::from("Permission denied")
            }, 
            _ => error.to_string(),
        },
    }; 
    println!("content is \"{}\"", content);
}
```
输出
```
content is "File not found"
```