# 猜数游戏重构版

```rust

use rand::prelude::*;
use std::io;fn main() {
    const MAX_RETRIES: i8 = 3;
    //生成指定范围内的随机数
    let random_number: i8 = thread_rng().gen_range(1..101);
    let mut buffer = String::new();
    let mut retry_count = 0; 
    println!("Guess a number between 1 and 100:"); 
    while retry_count < MAX_RETRIES {
       //清除之前的猜测
       buffer.clear(); 
       io::stdin()
            .read_line(&mut buffer)
            .expect("Failed to read line"); 
            // expect()等同于unwrap()
       // 用于从结果枚举中获取值
       // 当发生错误时，它允许打印一个自定义的信息 
       let guess = buffer.trim()
                         .parse::<i8>()
                         .expect("Failed to parse number"); 
                         if guess < random_number {
            retry_count += 1;
            println!(
                "too low. {}",
                if retry_count < MAX_RETRIES {
                    "Try again:"
                } else {
                    ""
                }
            );
        } 
        if guess > random_number {
            retry_count += 1;
            println!(
                "too high. {}",
                if retry_count < MAX_RETRIES {
                    "Try again:"
                } else {
                    ""
                }
            );
        } 
        if guess == random_number {
            println!("correct!");
            return;
        }
    } 
    println!("random_number was {}", random_number);
}
```

替代解决方案
```rust
use rand::prelude::*;
use std::io;fn main() {
    guess_with_infinite_retry();
}
fn guess_with_infinite_retry() {
    let secret_number = thread_rng().gen_range(1..101);
    println!("Guess a number between 1 and 100:"); 
    loop {
        let mut guess = String::new(); io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line"); 
        let guess: u32 = guess.trim()
                              .parse()
                              .expect("Failed to parse guess"); 
        if guess < secret_number {
            println!("guess too low, guess higher:");
        } else if guess > secret_number {
            println!("guess too high, guess lower:");
        } else {
            println!("you found it!");
            break;
        }
    }
}
```
替代方案，实现了 "继续玩":
```rust
use rand::prelude::*;
use std::io;fn main() {
    guess_with_limited_retry(5);
}
fn guess_with_limited_retry(max_retries: i8) {
    //生成指定范围内的随机数
    let random_number: i8 = thread_rng().gen_range(1..101);
    let mut buffer = String::new();
    let mut new_game = true; while new_game == true {
        println!("Guess a number between 1 and 100:");
        let mut retry_count = 0; 
        while retry_count < max_retries {
            buffer.clear(); 
            io::stdin()
                .read_line(&mut buffer)
                .expect("Failed to read line"); 
            let guess = buffer.trim()
                              .parse::<i8>()
                              .expect("Failed to parse number"); 
            if guess < random_number {
                retry_count += 1;
                println!(
                    "too low. {}",
                    if retry_count < max_retries {
                        "Try again:"
                    } else {
                        ""
                    }
                );
            } 
            if guess > random_number {
                retry_count += 1;
                println!(
                    "too high. {}",
                    if retry_count < max_retries {
                        "Try again:"
                    } else {
                        ""
                    }
                );
            } 
            if guess == random_number {
                println!("correct!");
                break;
            }
        }
        println!("random_number was {}", random_number); 
        println!("do you want to continue playing ? (y/n)");
      let mut try_again = String::new(); 
      io::stdin()
         .read_line(&mut try_again)
         .expect("Failed to read line"); 
      let try_again: char = try_again
         .trim()
         .parse()
         .expect("Failed to parse try again answer."); 
      if try_again == 'y' {
            new_game = true;
      } else if try_again == 'n' {
            new_game = false;
      } else {
            println!("answer with y or n.")
      }
    }
}
```