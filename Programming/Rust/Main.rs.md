
### Cargo:

```bash

cargo new //создать новый проект
cargo run
и флаги к ним --debug  --relesase
cargo build
cargo check //для проверки скомпилируется ли код
cargo update //для обновления библиотек
cargo doc --open
```

Самый простой код. Всё что заканчивается знаком ! это макрос.

```rust
fn main() {
    println!("Hello, world!");
}
```

Импорт стандартной библиотеки ввода вывода. mut делает переменную изменяемой. Except вернет ошибку в случае неудачи.

```rust
use std::io;

fn main() {
    println!("Guess the number!");

    println!("Please input your guess.");

    let mut guess = String::new();

    io::stdin()
        .read_line(&mut guess)
        .expect("Failed to read line");

    println!("You guessed: {guess}");
}
```

Добавляем библиотеку генерации рандомных чисел в cargo.toml. И используем в main.

```rust
use rand::Rng;

let secret_number = rand::rng().random_range(1..=100);
```


```rust
use std::cmp::Ordering;
use std::io;

use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::rng().random_range(1..=100);

    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");
            
		//интересно, лаконично, и blazing 
        
        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
}

```