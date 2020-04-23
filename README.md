# rollin'

[![LICENSE](https://img.shields.io/github/license/prodrive11/rollin)](LICENSE)

Keep your stuff rollin' with stdin log shredder utility. 


## Description

The `rollin` app reads lines from standard input and writes them to managed set of shredded files. Rolling is triggered based on **size** or **time**. It can be also enforced with `SIGHUP`.  
Output file name pattern can be set in configuration or input parameters, default will create `RFC8601` timestamped name with `.(s|u).out` extension.  
There is an option to compress filles on roll with one of available algorithms `gzip`, `snappy` or external compression programm.


## Installation

1. **Binaries**

   Binaries are available for download [here][releases]. Make sure to put the
   path to the binary into your `PATH`.

2. **From Crates.io**

   This requires at least [Rust] 1.35 and Cargo to be installed.

   ```
   cargo install rollin
   ```

   This will download and compile `rollin`. Make sure you add Cargo bin directory to your `PATH`.

3. **From Git**

   ```
   cargo install --git https://github.com/prodrive11/rollin.git rollin
   ```

4. **From sources**

   ```
   git clone https://github.com/prodrive11/rollin.git
   ```

   `cd` into `rollin/` and run

   ```
   cargo build
   ```

   The resulting binary can be found in `rollin/target/debug/`.


## Usage

`rollin` will primarily be used as a command line tool, although it can be also integrated in other projects as Rust crate.

Here are the main command examples

- `anyapp | rollin [dir] `

    Pipe `stdin` to `rollin` which will forward and manage logs with default settings in `dir`.
    Below simulates running application with random log outputs.

    ```console
    do echo "INFO $i $(openssl rand -base64 30)" ; done | rollin ./tmpdir
    ```

    `rollin` will create new or open existing `current` file in `./tmpdir` and forward lines from `anyapp`.  
    First log roll will be triggered by one of default threshold, either **24hrs** or **100mb**. 

- `app 2>&1 | rollin -z snappy -s 1G`

    This is the command will trigger log roll when size of current file reach 1GB, moved file will be renamed according to default pattern and compressed with snappy algorithm.
