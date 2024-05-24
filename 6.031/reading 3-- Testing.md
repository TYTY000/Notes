- Test-first programming. Write tests before you write code.
- Systematic testing with partitioning and boundary values, to design a test suite that is correct, thorough, and small.
- Glass box testing and statement coverage for filling out a test suite.
- Unit-testing each module, in isolation as much as possible.
- Automated regression testing to keep bugs from coming back.
- Iterative development. Plan to redo some work.

The topics of today’s reading connect to our three key properties of good software as follows:
- **Safe from bugs.** Testing is about finding bugs in your code, and test-first programming is about finding them as early as possible, right after you introduce them.
    
- **Easy to understand.** Systematic testing with a documented testing strategy makes it easier to understand how test cases were chosen and how thorough a test suite is.
    
- **Ready for change.** Correct test suites only depend on behavior in the spec, which allows the implementation to change within the confines of the spec. We also talked about automated regression testing, which helps keep bugs from coming back when changes are made to code.