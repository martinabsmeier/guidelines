# Programming specifications
## Guidelines
### Class names
Class names consist of nouns in whole words, which always start with a capital letter and are combined to form a word without separating spaces (CamelCase), e.g. ProcessManagementService. Frequent abbreviations such as HTTP or XML may also be used as part of the name. The first letter of an abbreviation is capitalised, the other letters are lower case to make names like ***XMLRPCService*** more readable, i.e. ***XmlRpcService***. Digits in class names are allowed as in ***Http11Service***.

Abstract classes are to be given the prefix Abstract as in ***AbstractProcess***. Classes that are to serve as the basis for the implementation of an interface are named with Base plus interface names, e.g. ***BaseProcessManager*** for the implementation of the ***ProcessManager*** interface.

The creation of new classes that have the same (simple) names as existing classes and only differ in the package should be avoided. An example would be to create a new class called ***StringUtils*** in an application specific package and at the same time to use the frequently encountered Apache Library Commons Lang, which also contains a class ***StringUtils***. A name that can easily be confused with another name, such as ***StringUtil***, should also be avoided.

The meaning and purpose of a class must at least be indicated by the name of a class. Classes and interfaces with names that are too general are to be avoided: Class called ***Session*** not only overlaps with the corresponding Hibernate interface, but also gives no idea of what type of session it is; Better would be e.g. ***UserSession***, ***EmailSession*** or similar, depending on what exactly is meant.

### Interface- and implementation name
The same rules apply to the interface names as to the class names. No special prefixes like the ***I*** in ***IProcessManagementService*** are used. The name of a class that implements an interface must indicate the specifics of the implementation, such as in the ***PlainTextMessage*** and ***HtmlMessage*** implementations of the hypothetical interface ***Message***.

### Variable, field and parameter names
Variable, field and parameter names must indicate their purpose, e.g. ***messageCount*** is better than ***count***, since it is immediately apparent what is being counted in the variable. Particularly cryptic names such as ***mdc*** for an object instance of the ***MessageDrivenContext*** type should be avoided. A name like ***messageDrivenContext*** or ***messageContext*** is much clearer. ***Context*** is again too general, since it is not clear what kind of context it is.

Exceptions to this rule are allowed if the purpose of a variable can be clearly deduced from the code context. Common abbreviations and single letters are allowed in some cases, e.g. with loop variables ***i***, ***j*** or with the number ***n*** of elements in a collection.

No special prefixes are used as in ***_name*** or ***mName*** or similar. The use of the prefix ***this*** in front of the field name is only permitted in setters or constructors with the parameter of the same name. Variable names ***l*** (lower case L) and ***l*** to identify long values (as in 100l) are not permitted, since l can easily be confused with 1 (number one).

### Code style and format
The specifications for the code style and the formatting were taken from the [Java code conventions of Oracle](http://www.oracle.com/technetwork/java/codeconventions-150003.pdf).

## Declarations
### Order of declaration in a class
The order of the declarations in a class is defined as follows:
1. Static variables and constants in descending order of visibility
2. Instance variables and constants in the descending order of visibility
3. Constructors
4. Static and instance methods without arrangement according to visibility but grouped according to functionality
5. Declarations of inner types

### Number of declarations per line
Only one declaration per line is allowed.

**Correct:**
```java
public class User {
    private Date creationDate;
    private Date modificationDate;
    ...
}
```

**Wrong:**
```java
public class User {
    private Date creationDate, modificationDate;
    ...
```

### Declaration hiding
Local declarations that other declarations would hide (at a higher level) are not allowed. **Exception:** Parameter names in constructors and setters can have the same names as the corresponding fields, in these cases the fields are referenced with the prefix ***this***.

**Correct:**
```java
public class User {
    private Date creationDate;
    private final Date modificationDate;

    public User(Date creationDate, Date modificationDate) {
        this.creationDate = creationDate;
        this.modificationDate = modificationDate;
    }

    public void setCreationDate(Date creationDate) {
        this.creationDate = creationDate;
    }

    public void printCreationDateWithoutHiding() {
        DateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String formattedDate = formatter.format(this.creationDate);
        System.out.println(formattedDate);
    }
}
```

**Wrong:**
```java
public class User {
    private Date creationDate;

    public void printCreationDateWithHiding() {
        DateFormat formatter = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        // Variable creationDate hides the field with the same name
        String creationDate = formatter.format(this.creationDate);
        System.out.println(creationDate);
    }
}
```

### Variable initialisation
The declared variables are initialised in the same statement as the declaration. Exceptions are allowed if the value to be set depends on other values or calculations or is initialised via a constructor or setter parameter. The dependencies on other components are to be initialised via dependency injection. Instance variables do not have to be initialised with default values (***null***, ***0*** or ***false***), as this is already taken over by the Java runtime environment.

**Correct:**
```java
public class User {
    private Date creationDate = new Date(); 

    public User(Date creationDate) {
        this.creationDate = creationDate;
    }

    public void setCreationDate(Date creationDate) {
        this.creationDate = creationDate;
    }
}
```

**Wrong:**
```java
public class User {
    private Date creationDate;

    public User() {
        // Initialisation NOT in the declaration statement
        creationDate = new Date(); 
    }

    public User(Date creationDate) {
        this.creationDate = creationDate;
    }

    public void setCreationDate(Date creationDate) {
        this.creationDate = creationDate;
    }
}
```

### Final declaration
Classes, methods, instance variables, local variables and method parameters do not have to be declared as final across the board. The only place where the Java compiler requires the use of final is to access local variables and parameters from an anonymous class. Otherwise, it makes sense to use ***final*** in the following cases:
- Classes that are not designed to be inherited must be declared final. The private, non static methods of a class that are intended for inheritance must be declared as final if they are not abstract or empty, i.e. if they are not intended as hooks for the subclasses.
- Classes that are intended for concurrent use by several threads must, if possible, be immutable, i.e. their fields must be declared final if possible.
- Everywhere where unintentional changing of the variable value could lead to subsequent errors.

## Statements
### Number of statements per line
Only one statement per line is allowed.

**Correct:**
```java
a++;
b--;
```

**Wrong:**
```java
a++; b--;
```

### Else-if and else statement
The else-if and else statements must be on the same line as the closing bracket of the preceding if block. The try-catch-finally statements are to be designed in the same way.

**Correct:**
```java
if ((id >= 0) && (id < 100)) {
    // ...
} else if ((id >= 100) && (id < 200)) {
    // on the same line
} else {
    // on the same line
}

try {
    // ...
} catch (Exception ex) {
    // ...
} finally {
    // ...
}
```

**Wrong:**
```java
if ((id >= 0) && (id < 100)) {
    // ...
}
else if ((id >= 100) && (id < 200)) {
    // NOT on the same line
}
else {
    // NOT on the same line
}

try {
    // ...
}
catch (Exception e) {
    // NOT on the same line
}
finally {
    // NOT on the same line
}
```

### Switch statement
All case branches of a switch statement must be terminated with the ***break-***, ***return-*** or similar (***continue***, ***throw***) statement. If the same functionality is to be carried out for several values (fall through), this fact must be marked with the special comment "***// fall through***". The default branch must always be present. If the case branches cover all known values, the default branch should throw an exception that matches the use case with a message that contains the illegal value and the corresponding information. The default branch must always be the last branch in the switch statement.

**Given enumeration:**
```java
public enum FruitType {
    APPLE, ORANGE, BANANA, LEMON
}
```

**Correct:**
```java
/**
 * Switch example with a fall-through for a special case and default behavior for everything else.
 */
public static void printFruitType(FruitType fruitType) {
    switch (fruitType) {
        case ORANGE:
        // fall through
        case LEMON:
            System.out.println("CITRUS");
            break;
        default:
            System.out.println("Unown fruit type: " + fruitType.name());
    }
}
```

**Wrong:**<br />
Fall through comment and the default branch is missing
```java
public static void printFruitType(FruitType fruitType) {
    switch (fruitType) {
        case APPLE:
            System.out.println("APPLE");
            break;
        case ORANGE:
        case LEMON:
            System.out.println("CITRUS");
            break;
        case BANANA:
            System.out.println("BANANA");
            break;             
    }
}
```

### For statement
When iterating over arrays and collections, the foreach iteration is preferable to the usual for loop. This improves readability (thanks to the omission of auxiliary variables for the iterator or for the array index) and reduces the susceptibility to errors, especially in the case of nested loops, where there is a risk of catching the wrong counting variable.

The following enum class, which represents the two sides of a coin, should serve as an example. The task is to list all possible combinations when flipping two coins.

**Given enumeration:**
```java
public enum CoinFace {
    HEAD, TAIL
} 
```

**Correct:**
```java
CoinFace[] faces = CoinFace.values();
for (CoinFace face1 : faces) {
    for (CoinFace face2 : faces) {
        System.out.println(face1 + " " + face2);
    }
}
/*
 * Output:
 * HEAD HEAD
 * HEAD TAIL
 * TAIL HEAD
 * TAIL TAIL
 */
```
This code is compact, meaningful, and less error-prone than the following variants.

**Wrong:**
```java
CoinFace[] faces = CoinFace.values();
for (int i = 0; i < faces.length; i++) {
    for (int j = 0; j < faces.length; j++) {
        System.out.println(faces[i] + " " + faces[i]);
    }
}
/*
 * Output:
 * HEAD HEAD
 * HEAD HEAD
 * TAIL TAIL
 * TAIL TAIL
 */
```
The two iteration variables are easy to mix up and make reading difficult. The (intentionally introduced) bug is still easy to find here.

**Wrong:**
```java
Collection<CoinFace> faces = Arrays.asList(CoinFace.values());
for (Iterator<CoinFace> i = faces.iterator(); i.hasNext();) {
    for (Iterator<CoinFace> j = faces.iterator(); j.hasNext();) {
        System.out.println(i.next() + " " + j.next());
    }
}
/*
 * Output:
 * HEAD HEAD
 * TAIL TAIL
 */
```
Here the intentional bug can no longer be found immediately.

### Blocks
The following rules apply to code blocks:
- Avoid empty blocks. If a block does not contain any instructions, a comment in the block must explain the reason for this. In particular a catch block cannot be empty. The reason why an exception is ignored must be documented. Exceptions can be made for constructors without parameters, as these are required by different frameworks and often do not contain any functionality. Another exception is a method that is required by an interface, but for which no meaningful implementation is possible.

**Correct:**
```java
public class ContextListener {

    // ...
    public void shutdown() {
        try {
            cleanup();
        } catch (IOException e) {
            // ignore exceptions during shutdown
        }
    }
    // ...
}
```

**Wrong:**<br />
Empty, undocumented catch block
```java
public class ContextListener {

    // ...
    public void shutdown() {
        try {
            cleanup();
        } catch (IOException e) {
        }
    }
    // ...
}
```
- The opening bracket is in the first line of the block declaration, i.e. in the same line as the method or class name or the control flow statement if, while etc. The closing bracket is on its own line and is indented on the same level as the beginning of the block. The block content begins on the next line after the opening bracket and is indented one level to the right with respect to the first line.

**Correct:**
```java
public String getFirstName() {
    return firstName;
}
```

**Wrong:**<br />
Block content not on the next line after the opening bracket
```java
public String getFirstName() { return firstName; }
```
- In the case of control flow statements (if, while, etc.), the associated statements must always be enclosed in a block with curly braces, even if it is only one statement. This increases readability and reduces the likelihood of errors.

**Correct:**
```java
if (user.isAdmin()) {
    log(adminMessage);
} else {
    sendEmail(userMessage);
}
// ...
while (!queue.isEmpty()) {
    processQueueEntry(queue.remove());
}
```

**Wrong:**<br />
Missing block brackets
```java
if (user.isAdmin()) log(adminMessage);
else  sendEmail(userMessage);
// ...
while (!queue.isEmpty()) processQueueEntry(queue.remove());
```

### Return values / parameter
Return values and parameters from a collection or array type must not be null. Any exception to this rule must be specified explicitly in the method comment. This is to avoid unnecessary checks of return objects and parameters for null.
Long parameter lists (with more than four parameters) should be avoided. When it comes to initializing a JavaBean, the builder pattern must be used, see [Builder pattern](http://en.wikipedia.org/wiki/Builder_pattern).

### Compound boolean expression
In the case of compound boolean expressions, individual clauses must be placed in round brackets if these clauses themselves contain an additional operator. This improves readability and eliminates the uncertainties associated with the precedence rules for operators.

**Correct:**
```java
if ((condition1 && condition2) || (condition3 && condition4)) {
    // ...
}
```

**Wrong:**<br />
Missing brackets around boolean clauses that contain an operator
```java
if (condition1 && condition2 || condition3 && condition4) {
    // ...
}
```
With such complex expressions, check the possibility of expressing the conditions on a higher semantic level, e.g. instead of:
```java
if ((user.creationDate.after(discountStart)
        && user.creationDate.before(discountEnd))
        || ((user.balance >= minBalance) && (user.balance <= maxBalance))) {
    // user gets discount
}
```
the restructured variant is preferable:
```java
if (user.eligibleForDiscountByCreationDate(discountStart, discountEnd)
        || user.eligibleForDiscountByBalance(minBalance, maxBalance)) {
    // user gets discount
}
```
with the corresponding methods in the user class:
```java
public final boolean eligibleForDiscountByCreationDate(Date discountStart, Date discountEnd) {
    return creationDate.after(discountStart) && creationDate.before(discountEnd);
}

public final boolean eligibleForDiscountByBalance(float minBalance, float maxBalance) {
    return (balance >= minBalance) && (balance <= maxBalance);
}
```

## Formatting
The entire source code is to be created in the UTF-8 character encoding to support various development and build environments.

### Line length and breaks
The maximum line length must not exceed 120 characters. The longer lines are broken, preferably after a comma or before an operator. The broken lines are indented two levels lower than the original line. Statements that are logically related should not be separated by a line break if possible.

**Correct:**
```java
public void methodWithLongParameterList(int a, int b, int c,
        int d) throws IOException { // line break after comma

    if (((a == b) && (c == d))
            || (a > b)
            || (c == 0)) {
        // line breaks before operators; cohesive statements not parted
    }
    // ...
```

**Wrong:**<br />
Line breaks according to operators, related statements separated
```java
    if (((a == b) &&
        (c == d)) ||
        (a > b) ||
        (c == 0)) {
    // line breaks after operators; cohesive statements parted
}
```

### Spaces
The following rules apply to spaces:

- No space between the method name and the following opening bracket.

**Correct:**
```java
public void methodWithoutSpace() {  
    // ...
```

**Wrong:**
```java
public void methodWithSpace () {
    // ...
```

- Always a space between a keyword and the following bracket.

**Correct:**
```java
while (!queue.isEmpty()) {
    // ...
```

**Wrong:**
```java
while(!queue.isEmpty()) {
    // ...
```

- Always a space after a comma in a listing

**Correct:**
```java
int[] weekDays = {1, 2, 3, 4, 5, 6, 7};
// ...
methodWithManyParameters("s1", "s2", 1, 2);
```

**Wrong:**
```java
int[] weekDays = {1,2,3,4,5,6,7};
// ...
methodWithManyParameters("s1","s2",1,2);
```

-All binary operators (except the dot operator) should be separated from their operands by spaces. The unary operators must not be separated from their operands.

**Correct:**
```java
a += b + c;
a = (a + b) / (c * d);
a++;
```

**Wrong:**
```java
a+=b+c;
a=(a+b)/(c*d);
a++;
```

- The statements in a for statement should be separated from one another by spaces.

**Correct:**
```java
for (int i = 0; i < 10; i++) {
    // ...
```

**Wrong:**
```java
for (int i=0;i<10;i++) {
    // ...
```

- A space should always be inserted after a cast.

**Correct:**
```java
methodWithIntAndAdminParams((int) longInteger, (Admin) user);
```

**Wrong:**
```java
methodWithIntAndAdminParams((int)longInteger, (Admin)user);
```

- In a method or class declaration, there should be a space in front of the opening curly brace.

**Correct:**
```java
public void methodWithSpace() {
    // ...
```

**Wrong:**
```java
public void methodWithoutSpace(){
    // ...
```

- Spaces at the end of a line must be removed.

### Blank lines
At least one blank line is inserted between method declarations. The field declarations may be made on two consecutive lines without a line in between.

**Correct:**
```java
private String firstName;
private String lastName;

public void method1() {
    // ...
}

public void method2() {
    // ...
}
```

**Wrong:**<br />
Missing line between methods
```java
public void method3() {
    // ...
}
public void method4() {
    // ...
}
```

## Documentation
The documentation accompanying the program in the form of Javadoc and code comments is important for the maintainability of the code and for its appropriate use. Important features of good documentation are its readability (for Javadoc both in the source text and in the generated HTML). Clarity and unambiguity, as well as being up to date and consistent with the corresponding code parts.

It is also important not only to document the functionality of a single part of the code, but also to describe the relationships with other parts of the code and configurations. Without such documentation, local changes at one point often result in an error in the program because the dependent points have not been adapted.

## Formatting
The text in Javadoc comments is converted to HTML by the Javadoc tool. Characters that are used as control characters in HTML are interpreted differently than the rest of the text. For example, the HTML tags ```<p>, <br>``` etc. are processed according to the HTML rules. If you want to exclude text from the HTML interpretation (e.g. because it contains special characters <,>, & etc.), you have to enclose it in the special *@literal* tag.<br />
An example:
```java
/**
 * ...
 * The triangle inequality is {@literal |x + y| < |x| + |y|}.
```
In this example, not only the special character <has been included in the *@literal* tag, but the entire formula that contains it, in order to improve the readability of the comment.

To include code examples in Javadoc comments, use the *@code* tag. This switches off the HTML interpretation for the tag content (similar to the *@literal* tag) and also ensures that the content is displayed in a special code font<br />
An example:
```java
/**
 * ...
 * @throws IndexOutOfBoundsException
 *             if the index is out of range i.e.
 *             {@code index < 0 || index >= this.size()}
 */
public E get(int index) {
    // ...
```
In order to retain the formatting of the code examples, the HTML ```<pre>``` tag should be used in combination with the *@code* tag.<br />
An example:
```java
/**
 * ... you can use an MBean proxy like this:
 * <pre>{@code
 * ConfigurationMBean conf = JMX.newMBeanProxy(connection, objectName, ConfigurationMBean.class);
 * int cacheSize = conf.getCacheSize();
 * conf.setCacheSize(2000);
 * conf.save();
 * }</pre>
```
It should be noted that the first sentence of a Javadoc comment serves as a summary for the corresponding element and is displayed, for example, in the API overview and by the IDEs, so this sentence should be designed as compact and meaningful as possible. To avoid confusion, two program elements must never have the same first sentence in the Javadoc comment. The first sentence in a Javadoc comment must always end with a period.

You should also note that for the Javadoc tool, the first sentence ends after a period followed by a space, a tab or a line break. If the first sentence itself contains such a point, an illegible summary of the commented element can result. To avoid this, you have to use the @literal tag, with which you prevent the special handling of the text by the Javadoc tool.

**Correct:**
```java
/**
 * Avoid incorrect termination of the first javadoc sentence,
 * {@literal e.g.} using the literal tag.
 */
public class FirstSentenceCorrect {
}
```

***Wrong***<br />
Period in the first sentence
```java
/**
 * Avoid incorrect termination of the first javadoc sentence,
 * e.g. using the literal tag.
 */
public class FirstSentenceWrong {
}
```

The HTML tags in the Javadoc comments should be used sparingly so as not to impair the readability of the comments in the source text. In most cases the tags ```<p>, <br>``` and the list tags ```<ul> <li>``` are sufficient. You have to do without the closing HTML tags and not strive for a valid XHTML, which would impair readability.<br />
An example:
```java
/**
 * Short summary sentence.
 * <p>
 * Followed by first paragraph.
 * <p>
 * Followed by second paragraph containing an unnumbered list:
 * <ul>
 *   <li>First list item
 *   <li>Second list item
 * </ul>
 */
```

### Package comments
A file called *package-info.java* must exist in each package alongside the Java source files. In addition to the package declaration, this file contains a summary of the functionality offered, usage and configuration examples, a short description of the classes it contains and any package annotations.<br />
An example:
```java
/**
 * Provides classes necessary to create and manage application users
 * as specified by the business requirement ABC-123. The most important
 * classes are:
 * <ul>
 * <li>{@link com.foo.user.UserManagementService} - used to register,
 * update, and delete users
 * <li>{@link com.foo.user.UserInfoService} - provides read-only services
 * to view user information
 * </ul>
 * <p>
 * To register a user, use the following:
 * <pre>{@code
 * User user = new User();
 * // init user properties
 * // ...
 * userManagementService.registerUser(user);
 * }</pre>
 * <p>
 * You can get an instance of {@link com.foo.user.UserManagementService}
 * by using the standard means of application server managed dependency
 * injection.
 * <p>
 * Note that this package has been obsoleted by the package xyz which
 * implements the new business requirement XYZ-456. It is planned to
 * remove this package in version 7.8.
 * 
 * @author Max Mustermann (Max.Mustermann@kfw.de)
 * @since 2.2
 */
@XmlSchemaType(
        name = "date",
        type = javax.xml.datatype.XMLGregorianCalendar.class)
@Deprecated
package com.foo.user;

import javax.xml.bind.annotation.XmlSchemaType;
```

In this example, the package documentation contains a brief description of the package and a reference to the underlying technical specification (if the specification can be found at a well known address, this must also be specified). The most important classes are listed as well as information on their use. The *@author* and *@since* tags indicate the original package author and the module version from which the package is available. (The module version is optional and must only be specified if the module is versioned manually, e.g. in the Maven POM.)

You can also see that the package should no longer be used because it has been replaced by a newer one. This information is identified by the *@Deprecated* annotation (which can e.g. be evaluated by IDEs). An informal addition to the Javadoc explains why the package should no longer be used, which replacement should be used, and when the package is no longer available.

### Class comments
All externally exported (public or protected) types (classes, interfaces, enums or annotations) must contain a summary description in the form of a Javadoc comment.

The name of the developer who created the type, as well as the names of all developers who made significant changes to the code, must be mentioned in an *@author* tag with their full names and the E-Mail address. If the module containing the type is versioned, the *@since* tag must also be present, which contains the version number from which the type is available.
Each type parameter T must be documented with the *@param* ```<T>``` tag.<br />
An example:
```java
/**
 * Represents an application user as specified in the business requirement ABC-123.
 * <p>
 * (Some more description ...)
 * <p>
 * This class is not thread-safe; use external synchronization if you want to use 
 * this class concurrently.
 * 
 * @author Max Mustermann (Max.Mustermann@foo.de)
 * @since 1.0
 * 
 * @param <T> the type of the id property
 */
@Entity
@NotThreadSafe
public class User<T extends Serializable> {
    // ...
```
In concurrent applications and thus in every web application, each class must be examined to see whether it could be used by several threads at the same time. In the case of concurrent use, the thread safety of the class must be documented with the *@ThreadSafe* annotation or its absence with the *@NotThreadSafe* annotation.

### Method comments
Every public or protected method must be provided with a method comment in Javadoc format.

A method comment briefly describes the contract between the method code and the calling client. The contract includes the preconditions that are necessary for the correct execution of the method (mostly the method parameters) and the effect (the postconditions) caused by the execution of the method (mostly the return value). The contract does not, however, include how exactly the method achieves the effect described (implementation details).

If the method has a side effect, this must also be described. A side effect is an observable change in the system state that is obviously not necessary to achieve postcondition. An example would be a background thread started by the method.

All parameters must be documented with *@param*, the return value with *@return* and the declared exceptions with *@throws*. The preconditions can be described with *@throws* and *@param*, the requirements for the respective parameter with the *@param* tag and the violation of these requirements with the *@throws* tag.<br />
An example:
```java
/**
 * Computes the factorial of the specified integer. The factorial {@code n!}
 * for positive integer {@code n} is defined as:
 * 
 * <p>
 * <pre>{@code
 * n! = n * (n - 1) * ... * 2 * 1
 * }</pre>
 * 
 * <p>
 * The factorial of 0 is defined as 1.
 * 
 * @param n the integer for which the factorial is computed;
 *          must be non-negative
 * @return the factorial of the specified integer
 * 
 * @throws IllegalArgumentException if the specified integer is negative
 */
public int factorial(int n) {
    // ...
```
The text in the *@param* or *@return* tag must be a noun phrase that describes the corresponding value. The text in the *@throws* tag must begin with the word "if" and contain a description of the conditions under which the corresponding exception is thrown. These descriptive texts do not represent full sentences. They therefore start with a lowercase letter and end without the end of the sentence.

In the case of accessor methods (getters and setters) that do nothing more than return or set fields, the method comment may be omitted in order to avoid trivial comments. If, on the other hand, additional actions are taken, the method must be provided with a corresponding Javadoc comment.

The keyword this should be used for references to the object on which the commented method is called.<br />
An example:
```java
/**
 * ...
 * @throws IndexOutOfBoundsException
 *             if the index is out of range i.e.
 *             {@code index < 0 || index >= this.size()}
 */
public E getElement(int index) {
    // ...
```

### Field comments
All public and protected fields of a class must be provided with a Javadoc comment. A brief description should clarify the purpose of a field. For non constant fields, the permissible values and other restrictions must be documented.

The values of an enumeration and annotation members are to be treated like public static fields of a class. If the Javadoc comment is short, you can put it compactly in one line.<br />
An example:
```java
public class ForEach {

    /**
     * Represents the two faces of a coin.
     * 
     * @author Max Mustermann (Max.Mustermann@kfw.de)
     * @since 2.4
     */
    protected enum CoinFace {
        /** The coin head (also known as obverse). */
        HEAD,
        /** The coin tail (also known as reverse). */
        TAIL
    }
    // ...
```
With annotation members you can optionally specify the default values.<br />
An example:
```java
/**
 * Describes transaction attributes on a method or class.
 * 
 * @author Max Mustermann (Max.Mustermann@kfw.de)
 * @since 0.8
 */
@Target({ ElementType.METHOD, ElementType.TYPE })
@Retention(RetentionPolicy.RUNTIME)
@Inherited
@Documented
public @interface Transactional {

    /**
     * A qualifier value for the specified transaction. Defaults
     * to an empty string.
     */
    String value() default "";

    /**
     * The transaction propagation type. Defaults to
     * {@link Propagation#REQUIRED}.
     */	
    Propagation propagation() default Propagation.REQUIRED;

    /**
     * The timeout for this transaction. Defaults to the default
     * timeout of the underlying transaction system.
     */
    int timeout() default TransactionDefinition.TIMEOUT_DEFAULT;

    /**
     * Indicates whether the transaction is read-only.
     * Defaults to <code>false</code>.
     */
    boolean readOnly() default false;
}
```

### Code comments
In contrast to Javadocs, code comments are not published as a program API and are not displayed by the IDEs as tooltips for exported program elements. They are used to point out the unobvious implementation details to someone who wants to study the code more closely and possibly modify it. The following rules result from this definition:

- The code must be organized in a self-sufficient manner and code comments must be inserted in such a way that another developer understands the code. This means that they are only useful for explaining the (non-trivial) process. Particularly trivial comments (those that duplicate code) should be avoided.

**Wrong:**
```java
i++; // increase i by 1
```

-Rather, a code comment is a helpful note where help could be needed, i.e. in more complicated places where the overall meaning does not immediately result from individual parts. The following example should make this clearer.

**Wrong:**
```java
/**
 * Computes an approximation of the square root of the specified
 * argument.
 * 
 * @param c the argument for which the square root is to be
 *            computed; must not be negative
 * @return the square root approximation
 * @throws IllegalArgumentException if the argument is negative
 */
public static double squareRoot(double c) {
    Preconditions.checkArgument(c >= 0, "Argument must not be negative");

    if (c == 0) {
        return 0;
    }

    double t = c / 2;
    while (Math.abs(t - (c / t)) > (SQUARE_ROOT_TOLERANCE * t)) {
        t = (t + (c / t)) / 2;
    }
    return t;
}
```

Here the code comments have been completely left out. As a result, a programmer who did not write the code will have quite a few problems understanding and analyzing this method. Some questions that cannot be answered at first glance are:
- Why is there special treatment for the number 0?
- What is t?
- Why is t initialized with c/2?
- How is the root actually calculated, i.e. which approximation algorithm is the calculation based on?
- The condition that is checked in the loop apparently has the purpose of aborting the approximation as soon as an error tolerance is reached. Why is the tolerance constant multiplied by t and not simply the tolerance constant?

In the next variant, some code comments were added, but they still leave something to be desired.
**Still wrong:**
```java
public static double squareRoot(double c) {
    Preconditions.checkArgument(c >= 0, "Argument must not be negative");
    
    // check for zero argument
    if (c == 0) {
        return 0;
    }

    // initialize estimate
    double t = c / 2;
    
    // update estimate until error tolerance is reached (Newton's method)
    while (Math.abs(t - (c / t)) > (SQUARE_ROOT_TOLERANCE * t)) {
        t = (t + (c / t)) / 2;
    }
    return t;
}
```

There is a comment on what is being done, but not why. At least it is clear that the approximation is made according to Newton's method and that t is the current estimate.

**Correct:**
```java
public static double squareRoot(double arg) {
    Preconditions.checkArgument(arg >= 0, "Argument must not be negative");

    /*
     * The square root estimation is computed using Newton's method as
     * described here:
     * http://en.wikipedia.org/wiki/Newton's_method
     * Error tolerance relative to current estimate is used to avoid
     * numerical accuracy problems.
     */
    
    // avoid division by zero in the error tolerance check
    if (arg == 0) {
        return 0;
    }

    // choose initial value for the estimation;
    // half initial argument has proven to be a good choice
    double estimate = arg / 2;

    // repeatedly apply Newton's update step until desired relative error
    // tolerance is achieved
    while (Math.abs(estimate - (arg / estimate)) > (SQUARE_ROOT_TOLERANCE * estimate)) {
        estimate = (estimate + (arg / estimate)) / 2;
    }
    return estimate;
}
```

In this variant, an introductory comment was made on the algorithm and the choice of error tolerance with a reference to the detailed algorithm description. The zero check and the choice of the initial value for the estimate are documented. In addition, the variables have been renamed to improve readability and understanding.

Another small improvement is still possible:
```java
    // ...      
    while (!isGoodSquareRootEstimate(arg, estimate)) {
        estimate = (estimate + (arg / estimate)) / 2;
    }
    return estimate;
}

/*
 * Indicates whether the specified estimate is a good approximation
 * as square root value for the specified argument.
 */
private static boolean isGoodSquareRootEstimate(double arg, double estimate) {
    return Math.abs(estimate - (arg / estimate)) <= (SQUARE_ROOT_TOLERANCE * estimate);
}
```

Here the termination condition was reformulated and transferred to an auxiliary method in order to further improve the readability of the squareRoot() method. It is also an example of a self-documenting program element, in this case a speaking method name.

**It is necessary to create or adapt the code comments when writing the code and not afterwards.**
