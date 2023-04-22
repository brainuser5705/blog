# How C++ programs get built
- C++ compilers will 
    - 1) check that it sollows the C++ language rules and aborts compilation process if error is found
    - 2) translates C++ source code into machine language into a *object file*
- C++ linkers wlll
    - take all the object files and *library file* (collection of precompiled code) combining them into a single executable program
    - ensures that all cross-file dependencies/references are resolved
- use an IDE that bundles up the compiler, linker, debugger, etc.
    - different run options in the IDE [explained](https://www.learncpp.com/cpp-tutorial/compiling-your-first-program/#:~:text=What%20is%20the%20difference%20between%20the%20compile%2C%20build%2C%20rebuild%2C%20clean%2C%20and%20run/start%20options%20in%20my%20IDE%3F)

# Configuring compiler
- **build configuration (build target)**: collection of project settings that determine how IDE will build project
    - what will executable be named
    - what directories will IDE look in for code or library files
    - how much to optimize code
    - debugging info
    - etc.
- **debug configuration**: type of build configuration to debug program
    - turns off optimization
    - include debugging information
- **release configuration**: type of build configuration when program will be released to public
    - optimized for size and performance
- **compiler extensions**: compiler-specific behavior since some compilers implement changes to the language standards
    - make sure to turn off so that the program can be compile on other compilers
- error vs warning
    - warnings do not halt compilation, but notify programmer of something that might be an error
    - can request compiler to be more assertive about warnings, or to treat warnings as errors
- use the latest language standard
    - code names are replaced by actual name upon finalization of the standard (e.g c++1x => C++11)
    - *standards document*: formal technical document that is the source of truth for rules and requirements (intended for compiler writers)
