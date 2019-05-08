# Sets

A set is a collection of items.

- **univ** is the universal set: it contains all items present in the current model
- **none** is the empty set
- **Int** is the set of integers 

To introduce a new set, a *signature* is used, e.g.
sig Things {}

## Multiplicity

Multiplicity can be used by using one of the following keywords:
- **lone**: zero or one
- **one**: one member
- **some**: one or more

In order to specify a different size, it is possible to use a *fact*:
sig Fruit {}
fact FiveFruit { # Fruit = 5 }

## Subsets

A set can be created as a subset of another set:
- sig Apple, Banana extends Fruit {}
- sig Fresh, Expensive in Fruit {}

**extends** introduces subsets which are mutually disjoint (do not have members in common)
**in** introduces sets which may overlap


## Enumerations

*Enumerations* are introduced using the keyword **enum**, e.g.
enum Stuff {A, B, C}


## Abstract signatures

An abstract signature introduces a set that contains nothing apart from the members of sets that extend that signature.

abstract sig Things {}

## Operations on sets

- + union
- & intersection
- - difference
- # number or cardinality

## Logical expressions

- **in** subset
- **in** membership
- **=** equality
- **some** non-emptiness
- **no** emptiness

## Predicates

A predicate is a parametrized boolean expression, e.g:
pref full \[a:Aircraft] { ... }


## Facts

A fact is a boolean expression which imposes some additional constraint on our specification:

fact manyPeopleExist {#Person > 2}

## Assertions

An assertion is a boolean expression to express some property that should follow a specification.

assert capacityNonNegative {all a:Aircraft | a.capacity >= 0}

# Relations

## Atoms

Atoms are Alloy's primitive entities. An atom is:

* indivisible
* immutable
* un-interpreted: it doesn't have any built-in properties

## Relations

A relation is a structure that relates atoms. A relation table consists of a set of tuples, where each tuple is a sequence of atoms. All tuples must have the same length.

## Domain and Range

The domain of a relation is the set of atoms in the first column of the table
The range of a relation is the set of atoms in its last column

## Identity relation

**iden** is the identity relation (each element relates to itself)