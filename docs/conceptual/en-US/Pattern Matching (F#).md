# Pattern Matching (F#)

Patterns are rules for transforming input data. They are used throughout the F# language to compare data with a logical structure or structures, decompose data into constituent parts, or extract information from data in various ways.


## [!INCLUDE[System_CAPS_remarks](//System/Token/System_CAPS_remarks_md.md)]
Patterns are used in many language constructs, such as the **match** expression. They are used when you are processing arguments for functions in **let** bindings, lambda expressions, and in the exception handlers associated with the **try...with** expression. For more information, see [Match Expressions &#40;F&#35;&#41;](Match+Expressions+28%F%2329%.md), [let Bindings &#40;F&#35;&#41;](let+Bindings+28%F%2329%.md), [Lambda Expressions: The fun Keyword &#40;F&#35;&#41;](Lambda+Expressions%3A+The+fun+Keyword+28%F%2329%.md), and [Exceptions: The try...with Expression &#40;F&#35;&#41;](Exceptions%3A+The+try...with+Expression+28%F%2329%.md).

For example, in the **match** expression, the *pattern* is what follows the pipe symbol.

match *expression* with

| *pattern* [ when* condition* ] -&gt; *result-expression*

...

Each pattern acts as a rule for transforming input in some way. In the **match** expression, each pattern is examined in turn to see if the input data is compatible with the pattern. If a match is found, the result expression is executed. If a match is not found, the next pattern rule is tested. The optional when *condition* part is explained in [Match Expressions &#40;F&#35;&#41;](Match+Expressions+28%F%2329%.md).

Supported patterns are shown in the following table. At run time, the input is tested against each of the following patterns in the order listed in the table, and patterns are applied recursively, from first to last as they appear in your code, and from left to right for the patterns on each line.



|Name|Description|Example|
|----|-----------|-------|
|Constant pattern|Any numeric, character, or string literal, an enumeration constant, or a defined literal identifier|**1.0**, **"test"**, **30**, **Color.Red**|
|Identifier pattern|A case value of a discriminated union, an exception label, or an active pattern case|**Some(x)**<br /><br />**Failure(msg)**|
|Variable pattern|*identifier*|**a**|
|**as** pattern|*pattern* as *identifier*|**(a, b) as tuple1**|
|OR pattern|*pattern1* &#124; *pattern2*|**([h] &#124; [h; _])**|
|AND pattern|*pattern1* &amp; *pattern2*|**(a, b) &amp; (_, "test")**|
|Cons pattern|*identifier* :: *list-identifier*|**h :: t**|
|List pattern|[ *pattern_1*; ... ; *pattern_n* ]|**[ a; b; c ]**|
|Array pattern|[&#124; *pattern_1*; ..; *pattern_n* ]|**[&#124; a; b; c &#124;]**|
|Parenthesized pattern|( *pattern* )|**( a )**|
|Tuple pattern|( *pattern_1*, ... , *pattern_n* )|**( a, b )**|
|Record pattern|{ *identifier1* = *pattern_1*; ... ; *identifier_n* = *pattern_n* }|**{ Name = name; }**|
|Wildcard pattern|_|**_**|
|Pattern together with type annotation|*pattern* : *type*|**a : int**|
|Type test pattern|:? *type* [ as *identifier* ]|**:? System.DateTime as dt**|
|Null pattern|null|**null**|

## Constant Patterns
Constant patterns are numeric, character, and string literals, enumeration constants (with the enumeration type name included). A **match** expression that has only constant patterns can be compared to a case statement in other languages. The input is compared with the literal value and the pattern matches if the values are equal. The type of the literal must be compatible with the type of the input.

The following example demonstrates the use of literal patterns, and also uses a variable pattern and an OR pattern.

[!CODE [FsLangRef2#4801](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4801)]
    Another example of a literal pattern is a pattern based on enumeration constants. You must specify the enumeration type name when you use enumeration constants.

[!CODE [FsLangRef2#4802](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4802)]
    
## Identifier Patterns
If the pattern is a string of characters that forms a valid identifier, the form of the identifier determines how the pattern is matched. If the identifier is longer than a single character and starts with an uppercase character, the compiler tries to make a match to the identifier pattern. The identifier for this pattern could be a value marked with the Literal attribute, a discriminated union case, an exception identifier, or an active pattern case. If no matching identifier is found, the match fails and the next pattern rule, the variable pattern, is compared to the input.

Discriminated union patterns can be simple named cases or they can have a value, or a tuple containing multiple values. If there is a value, you must specify an identifier for the value. In the case of a tuple, you must supply a tuple pattern with an identifier for each element of the tuple or an identifier with a field name for one or more named union fields. See the code examples in this section for examples.

The **option** type is a discriminated union that has two cases, **Some** and **None**. One case (**Some**) has a value, but the other (**None**) is just a named case. Therefore, **Some** needs to have a variable for the value associated with the **Some** case, but **None** must appear by itself. In the following code, the variable **var1** is given the value that is obtained by matching to the **Some** case.

[!CODE [FsLangRef2#4803](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4803)]
    In the following example, the **PersonName** discriminated union contains a mixture of strings and characters that represent possible forms of names. The cases of the discriminated union are **FirstOnly**, **LastOnly**, and **FirstLast**.

[!CODE [FsLangRef2#4804](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4804)]
    For discriminated unions that have named fields, you use the equals sign (=) to extract the value of a named field. For example, consider a discriminated union with a declaration like the following.


```
type Shape =
| Rectangle of height : float * width : float
| Circle of radius : float
```
You can use the named fields in a pattern matching expression as follows.


```
let matchShape shape =
match shape with
| Rectangle(height = h) -> printfn "Rectangle with length %f" h
| Circle(r) -> printfn "Circle with radius %f" r
```
The use of the named field is optional, so in the previous example, both **Circle(r)** and **Circle(radius = r)** have the same effect.

When you specify multiple fields, use the semicolon (;) as a separator.


```
match shape with
| Rectangle(height = h; width = w) -> printfn "Rectangle with height %f and width %f" h w
| _ -> ()
```
Active patterns enable you to define more complex custom pattern matching. For more information about active patterns, see [Active Patterns &#40;F&#35;&#41;](Active+Patterns+28%F%2329%.md).

The case in which the identifier is an exception is used in pattern matching in the context of exception handlers. For information about pattern matching in exception handling, see [Exceptions: The try...with Expression &#40;F&#35;&#41;](Exceptions%3A+The+try...with+Expression+28%F%2329%.md).


## Variable Patterns
The variable pattern assigns the value being matched to a variable name, which is then available for use in the execution expression to the right of the **-&gt;** symbol. A variable pattern alone matches any input, but variable patterns often appear within other patterns, therefore enabling more complex structures such as tuples and arrays to be decomposed into variables.

The following example demonstrates a variable pattern within a tuple pattern.

[!CODE [FsLangRef2#4805](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4805)]
    
## as Pattern
The **as** pattern is a pattern that has an **as** clause appended to it. The **as** clause binds the matched value to a name that can be used in the execution expression of a **match** expression, or, in the case where this pattern is used in a **let** binding, the name is added as a binding to the local scope.

The following example uses an **as** pattern.

[!CODE [FsLangRef2#4806](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4806)]
    
## OR Pattern
The OR pattern is used when input data can match multiple patterns, and you want to execute the same code as a result. The types of both sides of the OR pattern must be compatible.

The following example demonstrates the OR pattern.

[!CODE [FsLangRef2#4807](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4807)]
    
## AND Pattern
The AND pattern requires that the input match two patterns. The types of both sides of the AND pattern must be compatible.

The following example is like **detectZeroTuple** shown in the [Tuple Pattern](http://msdn.microsoft.com/en-us/library/#tuple) section later in this topic, but here both **var1** and **var2** are obtained as values by using the AND pattern.

[!CODE [FsLangRef2#4808](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4808)]
    
## Cons Pattern
The cons pattern is used to decompose a list into the first element, the *head*, and a list that contains the remaining elements, the *tail*.

[!CODE [FsLangRef2#4809](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4809)]
    
## List Pattern
The list pattern enables lists to be decomposed into a number of elements. The list pattern itself can match only lists of a specific number of elements.

[!CODE [FsLangRef2#4810](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4810)]
    
## Array Pattern
The array pattern resembles the list pattern and can be used to decompose arrays of a specific length.

[!CODE [FsLangRef2#4811](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4811)]
    
## Parenthesized Pattern
Parentheses can be grouped around patterns to achieve the desired associativity. In the following example, parentheses are used to control associativity between an AND pattern and a cons pattern.

[!CODE [FsLangRef2#4812](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4812)]
    
## <a name="tuple"> </a>

## Tuple Pattern
The tuple pattern matches input in tuple form and enables the tuple to be decomposed into its constituent elements by using pattern matching variables for each position in the tuple.

The following example demonstrates the tuple pattern and also uses literal patterns, variable patterns, and the wildcard pattern.

[!CODE [FsLangRef2#4813](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4813)]
    
## Record Pattern
The record pattern is used to decompose records to extract the values of fields. The pattern does not have to reference all fields of the record; any omitted fields just do not participate in matching and are not extracted.

[!CODE [FsLangRef2#4814](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4814)]
    
## Wildcard Pattern
The wildcard pattern is represented by the underscore (**_**) character and matches any input, just like the variable pattern, except that the input is discarded instead of assigned to a variable. The wildcard pattern is often used within other patterns as a placeholder for values that are not needed in the expression to the right of the **-&gt;** symbol. The wildcard pattern is also frequently used at the end of a list of patterns to match any unmatched input. The wildcard pattern is demonstrated in many code examples in this topic. See the preceding code for one example.


## Patterns That Have Type Annotations
Patterns can have type annotations. These behave like other type annotations and guide inference like other type annotations. Parentheses are required around type annotations in patterns. The following code shows a pattern that has a type annotation.

[!CODE [FsLangRef2#4815](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4815)]
    
## Type Test Pattern
The type test pattern is used to match the input against a type. If the input type is a match to (or a derived type of) the type specified in the pattern, the match succeeds.

The following example demonstrates the type test pattern.

[!CODE [FsLangRef2#4816](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4816)]
    
## Null Pattern
The null pattern matches the null value that can appear when you are working with types that allow a null value. Null patterns are frequently used when interoperating with [!INCLUDE[dnprdnshort](../Token/dnprdnshort_md.md)] code. For example, the return value of a .NET API might be the input to a **match** expression. You can control program flow based on whether the return value is null, and also on other characteristics of the returned value. You can use the null pattern to prevent null values from propagating to the rest of your program.

The following example uses the null pattern and the variable pattern.

[!CODE [FsLangRef2#4817](../CodeSnippet/VS_Snippets_Fsharp/fslangref2/FSharp/fs/patterns.fs#4817)]
    
## See Also
[Match Expressions &#40;F&#35;&#41;](Match+Expressions+28%F%2329%.md)

[F&#35; Language Reference](F%23+Language+Reference.md)

[Active Patterns &#40;F&#35;&#41;](Active+Patterns+28%F%2329%.md)
