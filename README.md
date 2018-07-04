I am writing a C compiler first. After it is a working C compiler, I will modify it to become my dream programming language. One being impure functional, containing Rust concepts like `Result` for error handling. Also an intent with the language is going to be for making it a viable option for creating compilers and interpreters.

This language aims to be:
* Modern
* Impure functional
* Simple and bare when it needs to be
* Batteries included when it needs to be
* Able to run on bare metal
* Easy to make fast software

This language **does not** aim to be:
* Garbage collected
* C
* Purely functional
* Inviable for general development

# Where The Language is Right Now
All of tests/stage_1 through tests/stage_4.

# Motivation
The main reason I am making this language is because I am annoyed with how few good options I have when I even set out to make a new programming language; reason two. I don't like when Rust keeps me from making something very dynamic and dangerous. I don't like pure functional programming languages because that's just not how my mind works. I tried Go and liked it until it began presenting values I didn't care for. I thought about using C or C++ but damn those are hard for making compilers. So I settled for Rust as the only con being its safety (of which I don't care for since nothing I write currently requires it). Rust has proven to be really really good for writing compilers or interpreters.

The second reason I am making this language is because I just want a language that patches the cons of the others I use. Garbage collection is annoying. While it makes it easy to write complex programs, it similarly makes it hard to write fast programs. Especially when the garbage collector is basically mandatory (\*cough\* D). I want a programming language that makes it easy to write fast programs so that it is viable for about any type of software including games and scientific applications. I also want a modern C. Things like module-based imports, a little more safety, and ease of development.

# Testing
```rs
$ cargo build
...
./target/debug/oxc ./test/stage_3/valid/div.c
./test/stage_3/valid/div.c:
int main() {
    return 4 / 2;
}

Scanner production:
[Keyword(Int), Id("main"), Symbol(LParen), Symbol(RParen), Symbol(LBrace), Keyword(Return), Integer(4), Operator(Slash), Integer(2), Symbol(Semicolon), Symbol(RBrace)]

Abstract syntax tree:
Func(
    "main",
    Return(
        BinOp(
            Slash,
            Const(
                4
            ),
            Const(
                2
            )
        )
    )
)

Generated assembly:
  .globl _main
_main:
  movl $2, %eax
  push %eax
  movl $4, %eax
  pop %ecx
  xor %edx, %edx
  idivl %ecx
  movl %ecx, %eax
  ret

$ ./div
$ echo $?
2
```
