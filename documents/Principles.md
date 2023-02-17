# Principles
When writing code, it is important to ensure that it is simple, understandable, robust and easy to maintain. On the one 
hand, this means that one developer must be able to read and understand someone else's code without their help. On the 
other hand, it must be possible to easily expand the existing code with new or changed requirements without the code 
quality falling.

Various techniques help to improve the readability and maintainability of source code:

- The code design through the code style and the formatting, which makes it easier to "find your way around" in the code
- The code structuring enables the isolation of functionality, which enables reusability, reduces complexity and increases 
  the semantics of the code. Means are:
Outsourcing or division of various functions into individual methods or classes or
Building more complex structures in packages, libraries or frameworks
Unit Testing: enables repeatable tests of the generated code on a small, modular level. Errors can be discovered so early that:
Sources of error can be effectively localized by directly testing the functionality without the influence of the environment.
it can be ensured that bug fixes have no negative side effects.
Tests in the event of changes (e.g. refactorings, replacement of a component or library) that have no impact on the public API ensure that no changed behavior occurs. 
