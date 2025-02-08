## 包和 Crate

> [ch07-01-packages-and-crates.md](https://github.com/rust-lang/book/blob/main/src/ch07-01-packages-and-crates.md)
> <br>
> commit c77d7a1279dbc7a9d76e80c5ac9d742dd529538c

模块系统的第一部分，我们将介绍包和 crate。

crate 是 Rust 在编译时最小的代码单位。如果你用 `rustc` 而不是 `cargo` 来编译一个文件（第一章我们这么做过），编译器还是会将那个文件认作一个 crate。crate 可以包含模块，模块可以定义在其他文件，然后和 crate 一起编译，我们会在接下来的章节中遇到。

crate 有两种形式：二进制项和库。*二进制项* 可以被编译为可执行程序，比如一个命令行程序或者一个 web server。它们必须有一个 `main` 函数来定义当程序被执行的时候所需要做的事情。目前我们所创建的 crate 都是二进制项。

*库* 并没有 `main` 函数，它们也不会编译为可执行程序，它们提供一些诸如函数之类的东西，使其他项目也能使用这些东西。比如 [第二章][rand] 的 `rand` crate 就提供了生成随机数的东西。大多数时间 `Rustaceans` 说的 crate 指的都是库，这与其他编程语言中 library 概念一致。

*crate root* 是一个源文件，Rust 编译器以它为起始点，并构成你的 crate 的根模块（我们将在 [“定义模块来控制作用域与私有性”][modules] 一节深入解读）。

*包*（*package*）是提供一系列功能的一个或者多个 crate。一个包会包含一个 *Cargo.toml* 文件，阐述如何去构建这些 crate。Cargo 就是一个包含构建你代码的二进制项的包。Cargo 也包含这些二进制项所依赖的库。其他项目也能用 Cargo 库来实现与 Cargo 命令行程序一样的逻辑。


包中可以包含至多一个库 crate(library crate)。包中可以包含任意多个二进制 crate(binary crate)，但是必须至少包含一个 crate（无论是库的还是二进制的）。

让我们来看看创建包的时候会发生什么。首先，我们输入命令 `cargo new`：

```console
$ cargo new my-project
     Created binary (application) `my-project` package
$ ls my-project
Cargo.toml
src
$ ls my-project/src
main.rs
```

我们运行cargo new my-project后，使用ls查看 Cargo 创建了什么。在项目目录中，有一个Cargo.toml文件，为我们提供了一个包(名)。还有一个src目录，其中包含main.rs。在文本编辑器中打开Cargo.toml，注意其中没有提到src/main.rs。Cargo 遵循一个约定，即src/main.rs是与包(名)同名的二进制 crate 的根目录。同样，Cargo 知道如果包目录包含src/lib.rs，则该包包含一个与包(名)同名的库 crate，并且src/lib.rs是其根目录。Cargo 将 crate 根文件传递给rustc以构建库或二进制文件。

在此，我们有了一个只包含 *src/main.rs* 的包，意味着它只含有一个名为 `my-project` 的二进制 crate。如果一个包同时含有 *src/main.rs* 和 *src/lib.rs*，则它有两个 crate：一个二进制的和一个库的，且名字都与包相同。通过将文件放在 *src/bin* 目录下，一个包可以拥有多个二进制 crate：每个 *src/bin* 下的文件都会被编译成一个独立的二进制 crate。

[modules]: ch07-02-defining-modules-to-control-scope-and-privacy.html
[rand]: ch02-00-guessing-game-tutorial.html#生成一个随机数
