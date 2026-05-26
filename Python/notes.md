https://github.com/iam-veeramalla/python-for-devops/blob/main/Day-16/README.md   --> Interview Questions ( Abhishek )

https://github.com/iam-veeramalla/sandbox/blob/main/python/8-python-libraries-for-devops.md --> Python libraries used in your project ( Abhishek )

---
✅ Give example of python scripting used in your project
- static website hosting using Cloudfront . ststic website has sso login for which code was written in Cloudfront edge function 

---
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

Immutable Dta Types -> int , float , bool , str , tuple
Mutable --> list , dict , set

Tuple cant be modified


## 1. Python doesn't has increment (++) or decrement (--) operator . instead use x=x+1 ?

## 1. Python is a tightly typed language . it doesn't do any implicit type conversion . u have to do explicit type conversion  ?

## 1. for else & while else block will run only if it doesn't encounters a break statement

python doesn't has block level varibale scope. only function level scope

## 1. Python used in your Project ?

Used Python Script to disable keys of IAM users which are more than 90 days old & create new keys and inform them to users . Runs once every week using eventbridge . New keys are never mailed to users . instead keys are stored in Secrets Manager & end users can retrieve them using awscli

Non prod EC2 to stop on weekends ( Used Cron Jons ( Runs every Saturday & Monday ) in Eventbridge & lamnda functions )

✅ Lambda Functions

✅ Decorators

✅ list vs tuple vs set vs dictionary

✅ what are generators in python ?

✅ Generator vs iterator

✅ Python File handling 

In Python, files can be processed without loading them into memory by iterating over the file object, reading in chunks, or using generators. This allows streaming large files efficiently with constant memory usage.

with open("/var/log/app.log") as f:
    for line in f:
        if "ERROR" in line:
            send_to_cloudwatch(line)

Always use with open() → avoids file descriptor leaks
Prefer text streaming for logs
Prefer chunked reads for binary data
Combine with generators for clean pipelines

✅ Difference between yield vs return

✅ GIL (Global Interpreter Lock)

✅ Multithreading vs multiprocessing vs asyncio . focus on when to use which

If Python code is CPU-heavy → use multiprocessing
If Python code waits on IO → use multithreading or async

✅ Memory management in Python . with focus on garbage collection

✅ Shallow copy vs Deep Copy

✅ Pickling

✅ Serialization

✅ Monkey Patching

✅ Module vs Package

✅ Call by value vs call by reference

✅ OOPS ( Encapsulation , Abstraction , Inheritance , Polmorphism )

✅ method overloading vs method overriding

✅ exception handling

✅ Duck Typing

