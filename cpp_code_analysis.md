# Static v.s. Dynamic
- When performing comprehensive source code reviews, both static and dynamic testing should be performed.
- Combining both types of code review should pick up about 95% of the flaws, provided the reviews are done by someone able to understand the source code during static analysis, and that the range of tests for dynamic analysis is sufficiently wide.
- Static code analysis
  - It is done without executing any of the code, collecting information based on source code
  - Static analysis source code testing is adequate for understanding security issues within program code and can usually pick up about 85% of the flaws in the code.
- Dynamic code analysis 
  - It relies on studying how the code behaves during execution. It often uses instrumentation.
  - Dynamic code review has the additional ability to find security issues caused by the code’s interaction with other system components (real input data)
  - Cannot guarantee the full coverage of the source code, as it's runs are based on user interaction or automatic tests.

# Static analysis

## cppchecker
- Official site: https://cppcheck.sourceforge.io/
- It provides unique code analysis to detect bugs.
- Focus on detecting undefined behaviour and dangerous coding constructs.
- Check list: https://sourceforge.net/p/cppcheck/wiki/ListOfChecks/
  - including some memory leak cases

## clang-tidy
- Official site: https://releases.llvm.org/9.0.0/tools/clang/tools/extra/docs/clang-tidy/index.html
- Its purpose is to provide an extensible framework for diagnosing and fixing typical programming errors.
- Like style violations, interface misuse, or bugs that can be deduced via static analysis.
- Check list: https://releases.llvm.org/9.0.0/tools/clang/tools/extra/docs/clang-tidy/checks/list.html

# Dynamic analysis

## Valgrind
- Official site: https://valgrind.org/
- There are Valgrind tools that can automatically detect many memory management and threading bugs, and profile your programs in detail. 
- 8 tools:
  - Memcheck is a memory error detector. It helps you make your programs, particularly those written in C and C++, more correct.
  - Cachegrind is a cache and branch-prediction profiler. It helps you make your programs run faster.
  - Callgrind is a call-graph generating cache profiler. It has some overlap with Cachegrind, but also gathers some information that Cachegrind does not.
  - Helgrind is a thread error detector. It helps you make your multi-threaded programs more correct.
  - DRD is also a thread error detector. It is similar to Helgrind but uses different analysis techniques and so may find different problems.
  - Massif is a heap profiler. It helps you make your programs use less memory.
  - DHAT is a different kind of heap profiler. It helps you understand issues of block lifetimes, block utilisation, and layout inefficiencies.
  - BBV is an experimental SimPoint basic block vector generator. It is useful to people doing computer architecture research and development.
- Check list: https://valgrind.org/docs/manual/manual.html section.4~13

## Sanitizer
- It consists of a compiler instrumentation module and a run-time library.

### AddressSanitizer
- Official site: https://clang.llvm.org/docs/AddressSanitizer.html
- Slow down 2x
- Can only be used independently 
- Fast memory error detector (overflow, leaks, use-after-, init, etc.)
- Check list: https://github.com/google/sanitizers/wiki/AddressSanitizer

### ThreadSanitizer
- Official site: https://clang.llvm.org/docs/ThreadSanitizer.html
- Slow down 5x-15x
- Can only be used independently 
- Detect data races
- Check list: https://github.com/google/sanitizers/wiki/ThreadSanitizerDetectableBugs

### MemorySanitizer
- Can only be used independently 

### UndefinedBehaviorSanitizer
- Can use together with others

### LeakSanitizer
- Can use together with AddressSanitizer

### DataFlowSanitizer
