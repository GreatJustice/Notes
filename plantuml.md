# Plant UML

## Contents

- [UML basics](#uml-basics)
- [File layout](#file-layout)
- [Comments](#comments)
- [Class diagrams](#class-diagrams)
- [Sequence diagrams](#sequence-diagrams)

## UML basics

- Association: a link between two elements
- Aggregation: a parent-child association, where the child *CAN* exist independent of the parent
- Composition: a parent-child association, where the child *CANNOT* exist indepdendent of the parent

Associations with arrows show "knowledge of". A subclass will point to its base class, because the subclass knows about the base class (but typically the reverse is not true).

Composition and Aggregation are shown with diamonds which appear on the side of the "larger" object.

### Multiplicity
Multiplicity indicates how many of one element are associated with how many of another element
```
x    : x
*    : Many
x..y : x to y
x..* : x to many
```
Multiplicity is directional, and can appear on both sides of an association.
The side it appears on reflects the number of that side's objects which are associated with one of the other side's objects. Note that "a to b" formulations indicate counts of the object on that side- not both sides. 
```
<Element1> [<Multiplicity of Element1's per Element2>] -- [<Multiplicity of Element2's per Element1>] <Element2>

Player "2..*" --- "1" Game
```
A player can only play one game, and a game requires two or more players.\
Note the "2..*" does NOT mean 2 players per many games.

[Top](#plant-uml)

## File layout
```
@startuml
Alice -> Bob
@enduml
```
[Top](#plant-uml)

## Comments

```
'A single line comment
```

```
/'Partial line comment'/ Alice -> Bob
```

```
/'
multiline
comment
'/
```

[Top](#plant-uml)

## Class diagrams

### Elements
Element types:
- abstract
- abstract class
- annotation
- class
- entity
- exception
- interface
- metaclass
- protocol
- stereotype
- struct
- circle (or ())
- diamond (or <>)
```
[<type>] <name> [as <alias>]

class BaseClass as C1
```
### Relations
Relations are composed of three parts:
```
[<left shape>]<connection>[<right shape>]
```
#### Connections
Use dashes for solid lines and dots for dotted lines. The more characters, the longer the connection
```
--
---
..
...
```

#### Shapes:
Left and right shapes are symmetric
```
Extension:    <|--   (a triangle)
Composition:  *--    (a filled diamond)
Aggregation:  o--    (a diamond)

Just a line:  --
Pointy arrow: <--
Box:          #--
x:            x--
}:            }--
+:            +--
```

Common class diagram relations
```
Extension:   <|--
Composition: *--
Aggregation: o--
```

You can label relations on each side (such as for cardinality), and on the connection. The connection label can have an arrow
```
<lhs> ["<lhs-label>"] <connection> ["<rhs-label>"] <rhs> [: [<] <connection label> [>]]

Class01 "1" *-- "many" Class02 : contains >
```

### Fields and Methods
```
+ Foo : size()
- Foo : Bar[] data
```
Elements are auto-detected to be fields or methods. To override, precede with `{field}` or `{method}`

Visibility
```
Public:          + (rendered as green circle)
Protected:       # (rendered as yellow diamond)
Private:         - (rendered as red square)
Package Private: ~ (rendered as blue triangle)
```

Modifiers
```
{static}   (rendered as underlined)
{abstract} (rendered as italic)
```

You can specify fields and methods inline
```
class Foo {
    + int size()
    - Bar[] data
}
```

### Notes
```
class Foo {
    + bar()
}
note left: This is a note on the last defined class

note left of Foo : This is a note on Foo
note left of Foo::bar : This is a note a Foo::bar()

note "This is a named note" as N1
Foo .. N1

```
Note directions:
- left
- right
- top
- bottom

### Packages

You can group elements into a "package"
```
package MyPackage {
    class Foo;
    class Bar;
}
```

### Generics
Generic/Template parameters?
```
class Foo<Element> {
    int size()
}
Foo *- Element
```

### Stereotypes
Stereotypes are a way of extending the vocabulary of UML.
For example, there is no element in UML to represent C++ enums.
To model enums, we can use a stereotyped class.
```
class Color << Enum >>
```
Despite the similar appearance, stereotypes are NOT template/generic parameters.

[Top](#plant-uml)

## Sequence diagrams

Participant types:
- participant
- actor
- boundary
- control
- entity
- database
- collections
- queue

Declare participants
```
[<type>] <name> [as <alias>] [order <number>]

Alice -> Bob : Authentication Request
Bob -> Alice : Authentication Response
```
[Top](#plant-uml)
