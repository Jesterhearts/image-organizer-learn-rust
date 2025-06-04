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

## Tips

You can use cargo to add dependencies to your project. For example, to add the `ratatui` crate, run the following
command in the terminal:
```sh
cargo add ratatui
```

Many libraries can be found by searching [crates.io](https://crates.io/) or
[lib.rs](https://lib.rs/). The libraries recommended in this project also tend to have good
documentation available on [docs.rs](https://docs.rs/). You can also find the github repos for the
libraries linked on the crates.io page. Each library will have an `examples` folder with example
code you can play with to get a feel for how the library works.

## Projects
### Week 1
- [ ] Create a basic Rust TUI using the `ratatui` library that displays an outlined block with the
  text "Hello, World!".

  <details> <summary>Hint</summary>
  
  A lot of tutorials will push for using an `App` struct to handle the state of the TUI. This can be
  useful for more complex applications, but for this project it is far simpler to just do everything
  in a main loop. For example:

  ```rust
  fn main() {
      let terminal = <create terminal here>;

      loop {
          terminal.draw(|f| {
              f.render_widget(<paragraph widget with border>, f.area());
          }).unwrap();

          if crossterm::event::poll(Duration::from_millis(100)).unwrap() {
              match event::read().unwrap() {
                  Event::Key(KeyEvent {
                      code: KeyCode::Char('q'), ..
                  }) => {
                      break; 
                  }
                  _ => {}
              }
          }
      }

      Ok(())
  }
  ```
  </details>
  
- [ ] Add the ability to specify a directory in the arguments to the application and display the
  directory in the TUI.
  
  <details> <summary>Hint</summary>
  
  The `clap` crate is a great way to make handling command line arguments easier. Alternatively, you
  can use `std::env::args` to get the arguments passed to the program. </details>

  <details> <summary>Hint 2</summary>
  
  You can think of `clap` as just another library. The `#[derive(Parser)]` procedural macro is just
  like a function call to a normal library that happens to generate code at compile time rather than
  executing code at runtime. The fields of your struct are like arguments to this function, and the
  `#[clap(...)]` attributes are like options you can pass to the function. </details>

- [ ] Read the directory and display a list of files in the ui.

    <details> <summary>Hint</summary>

    You can use the `std::fs` module to read the directory and get a list of files. </details>

    <details> <summary> Hint 2 </summary>
  
    The `DirEntry` struct has functionality to determine if a particular entry is a file or a
    directory. </details>

    <details> <summary>Hint 3</summary> 

    You can either use a `List` widget to display the list of files, or you can use the `Layout`
    builder to specify the layout of the list, and specify a `Paragraph` widget for each file.
    </details>
  
#### Stretch Goal
- [ ] Add the ability to move up and down the list of files using the arrow keys.

    <details> <summary>Hint</summary>
    
    You can use the `crossterm` crate to handle keyboard input. </details>

    <details> <summary>Hint 2</summary>

    You can use the `Style` struct and `set_style` method to change the color of the selected line. </details>

### Week 2
- [ ] Implement the stretch goal from Week 1.
- [ ] Add the ability to open a selected file using the `enter` key and display the image in the UI.
  The `escape` key should exit the image viewer and return to the file list. There is a crate called
  `ratatui-image` that can be used to display images in the TUI. You may need to manually specify
  `Sixel` as the format for the image on Windows.
- [ ] Make sure the image is captioned with the file name and path. The caption should be displayed
  at the bottom of the image.

  <details> <summary>Hint</summary>

  You may need to use the `Layout` struct to create a layout for the image and caption. </details>

#### Stretch Goal
- [ ] Add a text input field (you can use e.g. `ratatui_textarea`) which allows a directory to be
  specified and make displayed list of files update to reflect the new directory. The text input
  field should be displayed at the top of the TUI. Pressing `enter` should commit the text in the
  input field and update the UI.

### Week 3
- [ ] Implement the stretch goal from Week 2.
- [ ] Update the handling of the text input field to allow filenames to be entered. If the filename
  is listed in the UI, it should be highlighed as selected and the `enter` key should open the
  image. It should still be possible to enter a directory and have the list of files update
  accordingly.

#### Stretch Goal
- [ ] Get `tantivy` up and running. Add a command line argument for the search query, and have
  `tantivy` do the matching against filenames for you. You do not need to support search in the TUI
  itself.


### Week 4
- [ ] Implement the stretch goal from Week 3.
- [ ] Change the UI so that text entry uses `tantivy` for searching to match against filenames. The
  UI should still handle directory matching and loading.
- [ ] Change the input handling to be debounced so that after a delay in typing, the search is
  automatically updated.

#### Stretch Goal
- [ ] Make the search and directory loading run in a background thread and send changes to the main
  thread when processing finishes so the UI isn't blocked. You should have some sort of loading
  indicator to animate while work is happening in the background.

  <details> <summary>Hint</summary>
  You can use `std::sync::mpmc` to send messages between threads. </details>

### Week 5
- [ ] Implement the stretch goal from Week 4.
- [ ] Add the ability to tag images. The tags can be stored in memory for now, but should update
  `tantivy` so that images are searchable by tag.
- [ ] Add the ability to view tags to the UI. A select number of tags should be displayed in the
  search results, with a longer list of tags visible when the image is selected. The tags should be
  displayed in a different color to the filename.

#### Stretch Goal
- [ ] Persist the tags to a database. You can use `redb` to handle this persistence.

### Week 6
- [ ] Implement the stretch goal from Week 5.
- [ ] Add the ability to remove tags from images. This should update the database and `tantivy`
  index. You should be able to hit the tab key on the image viewing screen to cycle between tags and
  remove them with `delete`.

#### Stretch Goal
- [ ] Add the ability to hash and persist file hashes to the database. I'd recommend using `blake3`
  for file hashing.

### Week 7
- [ ] Implement the stretch goal from Week 6.
- [ ] Use the hashes you generate for files to lookup info in the database. This should apply to
  every file loaded so that you can still retrieve metadata for moved files. Make sure to update the
  hash information for edited files if the file at a known path location has changed.

#### Stretch Goal
- [ ] Add the ability to view metadata for images. This should be displayed in a separate screen that
  can be accessed by pressing `m` while viewing an image. The metadata should be displayed in a
  scrollable list.

### Week 8
- [ ] Implement the stretch goal from Week 7.
- [ ] Add mouse handling to the TUI. You should be able to click on images to open them, and click
  on tags to remove them. You should also be able to click on the text input field to focus it. The
  scroll wheel should also scroll the list of images and tags.