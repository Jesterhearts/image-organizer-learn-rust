# Image Organizer
This repo contains a set of projects that build on each other with the end goal of learning Rust and
having a mini image organizer TUI. It was written with the express purpose of helping a friend of
mine learn Rust, but if you find it useful you are welcome to follow along.

The end application will have the following features:
- Tagging with database persistence and matching moved images by hash.
- Sorting and searching images by tag, name, date, and metadata.
- Viewing images.

Projects are intended to take a week, with stretch goals that may be more difficult to reach each
week. The target audience is beginners to Rust who have some programming experience (e.g. basic
knowledge of control flow and data structures).

Content in collapsed sections are hints to make it easier to solve each problem. Feel free to expand
them if you are stuck.

## Things you will need
- [Rust](https://www.rust-lang.org/tools/install)
- [VSCode](https://code.visualstudio.com/)
- [Rust Analyzer](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer)

You can use cargo to add dependencies to your project. For example, to add the `ratatui` crate, run the following
command in the terminal:
```sh
cargo add ratatui
```

## Projects
### Week 1
- [ ] Create a basic Rust TUI using the `ratatui` library that displays an outlined block with the
  text "Hello, World!".
- [ ] Add the ability to specify a directory in the arguments to the application and display the
  directory in the TUI.
  
  <details> <summary>Hint</summary>
  
  The `clap` crate is a great way to make handling command line arguments easier. Alternatively, you
  can use `std::env::args` to get the arguments passed to the program. </details>
- [ ] Read the directory and display a list of files in the ui.

    <details> <summary>Hint</summary>

    You can use the `std::fs` module to read the directory and get a list of files. </details>

  <details> <summary> Hint 2 </summary>
  
  The `DirEntry` struct has functionality to determine if a particular entry is a file or a
  directory. </details>
  
#### Stretch Goal
- [ ] Add the ability to move up and down the list of files using the arrow keys.

    <details> <summary>Hint</summary>
    
  You can use the `crossterm` crate to handle keyboard input. </details>