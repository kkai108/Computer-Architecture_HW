#  Setup Ripes for Windows 

We will use a RISC-V pipeline simulator (Ripes) to observe the working of the pipeline.

## 1. Install Ripes

1. Please download the release version of Ripes from [Ripes Release build](https://github.com/mortbopet/Ripes/releases).
   `Ripes-v2.2.3-win-x86_64.zip` is the current version for Windows platform.
2. Please extract the file. 
   Please right click and extract the file. Here the file was extracted in `D:\ee3450`.

## 2. Install Toolchain

1. Please download the SiFive RISC-V toolchain from https://github.com/sifive/freedom-tools/releases. 
   For example, ```riscv64-unknown-elf-gcc-10.1.0-2020.08.2-x86_64-w64-mingw32.zip``` is the current version for Windows.
3. Please extract the file to a folder.
Please right click and select extract on windows. Location we test in this example is `D:\ee3450`.
5. Please find out from the extract folder where `riscv64-unknown-elf-gcc` is. In this example the file location is`D:\ee3450\riscv64-unknown-elf-gcc-10.1.0-2020.08.2-x86_64-w64-mingw32\bin`. 
6. This folder is denoted as `$env:RV64_GCC_PATH` below. You may use following command to setup environment variable: ` $env:RV64_GCC_PATH=D:\ee3450\riscv64-unknown-elf-gcc-10.1.0-2020.08.2-x86_64-w64-mingw32\bin`.

## 3. Test Setup
      
1. Download cmul.S from PA3.
2. Start a terminal. 
   Please enter "powershell" at folder's address block section and press "enter".
4. Go to the download folder with cmul.S, e.g., ``` cd D:\ee3450\ ```.
5. Generate an rv32im executable:  
    ``` 
    & $env:RV64_GCC_PATH\riscv64-unknown-elf-gcc.exe -march=rv32im -mabi=ilp32 -s -static -nostdlib -o cmul_test.elf cmul.S 
    ```
    Before compiling the file, please double check $RV64GCCPATH is set with the folder location with riscv64-unknown-elf-gcc.

    > * "-march=rv32im" to use an ISA version with 32-bit architecture and integer ("i") and multiply ("m") supports.
    > * "-mabi=ilp32" to specify the language data model. In this setup, integer ("i"), long ("l"), and pointer ("p") are all 32-bit.
    > * "-s" to strip symbols from binary.
    > * "-static" to link statically to produce a complete executable.
    > * "-nostdlib" do no use stdlib.
    > * "-o" specify output name.

5. Start Ripes GUI by double clicking the program
6. File -->[Load Program](figure/01_Ripes_Load_1.png "Load")
   Use [Open](figure/02_Ripes_Load_2.png "Open") to search for cmul.elf.  
   In our example, cmul.elf is in /Users/ee3450/Downloads/.
7. Select [Memory Tab](figure/03_Ripes_Tabs.png "Tab").
   In [Memory Tab](figure/03_Ripes_Memory_Panel.png "Memory Tab"), 
   we check the [data segment base memory address](figure/04_Ripes_Data_Segment_Base.png "Data base").
   Here we find that the base is 0x000110f0.
   Since the assembler assume the global pointer (x3) to be set at the base + 0x800,
   we will use 0x000110f0+0x800=0x000118f0 to setup global pointer.
8. Click on [processor selection button](figure/05_Ripes_Processor_Selection.png "Processor") at top-left
   to [set global pointer](figure/06_Ripes_Set_gp.png "GP") as 0x118F0.
9. After we set the global pointer for the processor, we need to load the cmul.elf again.
10. Click [>> button](figure/07_Ripes_Run_Without_GUI.png "Run") on top-right to run and simulate the binary in Ripes without GUI updates.
11. We should see the program finishes and print ``11 + i* 17`` on [the console](figure/08_Ripes_Console.png "Console").
    Note that if we need to set the global pointer for each new elf program load (since the data segment changes according to the text segment).

