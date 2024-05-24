```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

String s = "2020-03-18";
Pattern regex = Pattern.compile("(?<year>\\d{4})-(?<month>\\d{2})-(?<day>\\d{2})");
Matcher m = regex.matcher(s);
if (m.matches()) {
    String year = m.group("year");
    String month = m.group("month");
    String day = m.group("day");
    // Matcher.group(name) returns the part of s that matched (?<name>...)
}
```


```parserlib
statement ::= 
  '{' statement* '}'
| 'if' '(' expression ')' statement ('else' statement)?
| 'for' '(' forinit? ';' expression? ';' forupdate? ')' statement
| 'while' '(' expression ')' statement
| 'do' statement 'while' '(' expression ')' ';'
| 'try' '{' statement* '}' ( catches | catches? 'finally' '{' statement* '}' )
| 'switch' '(' expression ')' '{' switchgroups '}'
| 'synchronized' '(' expression ')' '{' statement* '}'
| 'return' expression? ';'
| 'throw' expression ';' 
| 'break' identifier? ';'
| 'continue' identifier? ';'
| expression ';' 
| identifier ':' statement
| ';'
```