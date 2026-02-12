## 1. Is poython compiler or interpreter ?

Python is both compiled and interpreted - it's a two-step process:

Step 1: Compilation (to bytecode)

Python source code (.py) → compiled to bytecode (.pyc)
Bytecode is platform-independent, lower-level representation
Stored in __pycache__ directory
This happens automatically when you import modules

Step 2: Interpretation (of bytecode)

Python Virtual Machine (PVM) interprets bytecode
Executes instructions line by line
No compilation to native machine code (in CPython)

   <img width="598" height="367" alt="image" src="https://github.com/user-attachments/assets/1c00f9c8-faec-4120-8403-2190c20c7d73" />

## 1. Is poython dynamic type or ststic type language ?

Dynamic Type as type of varibales are determined at run time rather than compile time 
