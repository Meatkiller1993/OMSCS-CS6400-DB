# EXAM2 NOTES

## Entity Types

### Multi-Valued Property Types
![Image](https://assets.omscs.io/notes/20221010225301.png)
- ET-F contains attributes A, which is a foreign key on A in ET, and F, which holds a single value of F for the ET identified by A.

- In the relational model, instead of creating an attribute that takes multiple values, we create multiple tuples in a new relation that all reference the original relation.

### 1-1 Relationships
![Image](https://assets.omscs.io/notes/20221010230348.png)

- In ET1, we can insert the primary key of ET2, B, as a foreign key in ET1 to connect the two relations. 
- Alternatively, we can insert the primary key of ET1, A as a foreign key in ET2 to form the connection.

### 1-many relationships
- There is a unique value for A for each instance of ET1, and there must associate multiple instances of ET2 with that key. 
- The relational model does not allow multi-valued attributes, so these references cannot live in ET1. We need the ET2 relation to hold the foreign key. 
- Note that this solution only works because the ET2 instance can only associate with at most one ET1 instance.

### Many to many relationships
![Image](https://assets.omscs.io/notes/20221010235830.png)
- We could not reference ET2 from ET1 since that would require a multi-valued attribute. But now we cannot reference ET1 from ET2 for the same reason. 
- It needs to generate a particular relation for R, which stores references to the primary keys A of ET1 and B of ET2.
- For a tuple in ET1, there may be multiple tuples in R referencing its key, A, each with a unique value of B identifying a tuple in ET2, and, vice versa. 

### Weak Entity Types and Identifying Relationships
![Image](https://assets.omscs.io/notes/20221011001925.png)
Insert a reference to ET1 in ET2. Specifically, we add an A attribute in ET2 as a foreign key on the A attribute in ET1. ET2 may have duplicate values for both A and B separately, but each pair will be unique. In other words, the combination of A and B constitutes the key in ET2.

### Inheritance: Mandatory Disjoint
![image](https://assets.omscs.io/notes/20221011004826.png)

- In this relationship, there can be no instances of ET that are neither instances of ET1 nor ET2 (the mandatory constraint), and no instances of ET can be instances of both ET1 and ET2 (the disjoint requirement).

- All ET instances will sit as tuples in either the ET1 or ET2 relations. ET1 has primary key A and attributes B and C, and ET2 has primary key A and attributes B and D.

### Inheritance: Mandatory, Overlap Allowed
![image](https://assets.omscs.io/notes/20221011010840.png)
- We might represent this system with a single relation, ET, which has a primary key A, attributes B, C, and D, and an attribute named Type. Type holds information indicating whether this tuple is an instance of ET1, ET2, or both.  OR：

- Let's instead create three relations: ET, with primary key A and attribute B; ET1, with foreign key A on ET and attribute C; and ET2 with foreign key A on ET and attribute D. When we create a new instance of ET, we add a tuple to the ET relation and then to either ET1, ET2, or both.

### Inheritance: Non-Mandatory, Overlap Allowed
The solution here is almost identical to that from the previous section. Let's create three relations: ET, with primary key A and attribute B; ET1, with foreign key A on ET and attribute C; and ET2 with foreign key A on ET and attribute D. When creating a new instance of ET, we add a tuple to ET and then either ET1, ET2, both, or neither.

### Inheritance: Non-Mandatory, Disjoint
Finally, let's consider the non-mandatory, disjoint relationship type. Again we generate the relations ET, ET1, and ET2, as we saw previously. Since the relationship is not mandatory, we can have tuples in ET not referenced by ET1 or ET2. When references are present, however, the referencing tuple will exist exclusively in ET1 or ET2. 

### Union Types
![image](https://assets.omscs.io/notes/20221011013919.png)
We can insert an artificial identifier in the relation ET, ET-ID, which consists of either a value of C or D. Remember, every tuple in ET must be either an instance ET1 or ET2, so we can uniquely identify each tuple by the primary key of one of the union types.

## Relational Algebra and Calculus
Relational Algebra Operations
- The set union operator, R∪S
- The set intersection operator, R∩S
- The set difference operator, R∖S
- The Cartesian product operator, R×S

*-* We use the projection operator, π, to eliminate attributes, or columns, from a relation. 

*-* We use the selection operator, σ, which eliminates rows.

- The third group of operators is the joins, which are constructor operations. We have the natural join operator, R∗S. 
- We also have outer joins, of which there are three types: right, left, and full. In this lesson, we look at left outer joins, ⋈L.
- ​Finally, we have theta joins, R⋈θ S.

### Selection
The general syntax to select all the tuples from a relation,R, that satisfy an expression is 
σexpression(R). For example, if we want to retrieve all tuples in the RegularUser relation, we would say:
σ(RegularUser)
### Projection
For example, suppose we want to retrieve just the email, birth year, and sex for all regular users in Atlanta. We can express this query with the following syntax:

π Email, BirthYear, Sex (σ HomeTown=’Atlanta’)(RegularUser)

### Intersection
The second set-based operator is the intersection operator: ∩

π CurrentCity (RegularUser)  ∩ π HomeTown(RegularUser)

### Set Difference
![image](https://assets.omscs.io/notes/20221012163435.png)
The third set-based operator is the difference operator, ∖ . Suppose we want to find the elements of the set of current cities that are not also elements of the set of hometowns. Here's how we would formulate this query:

π CurrentCity (RegularUser) ∖ π HomeTown (RegularUser)

### Natural Join
![image](https://assets.omscs.io/notes/20221012214045.png)

RegularUser ∗ Major60sEvents

Notice that both RegularUser and Major60sEvents have a similarly named attribute, Year. The natural join operation compares all combinations of tuples from both relations and checks if the value of Year is the same.

Features of a natural join:

- It matches values of attributes with the same name and only those.
- It keeps only one copy of the join attribute in the result.
- We call a natural join an inner join because only the matching tuples appear in the result, with unmatched tuples eliminated.

### Theta Join
![image](https://assets.omscs.io/notes/20221012214918.png)

RegularUser⋈ 
BirthYear < EventYear Major60sEvents
Feature: 
- A natural join can express only equality conditions, while a theta join can also express inequality conditions using the <, ≤, >, ≥, and ≠ operators.
- Two relations joined by a natural join must share an attribute name, while the theta join does not have this restriction.
- In the natural join, the join attribute appears once in the result. Both attributes in a theta join appear in the relation (BirthYear and EventYear above).

### Left Outer Join
![image](https://assets.omscs.io/notes/20221012221326.png)

The outer join operator, specifically the left outer join, whose operator is  ⋈L.

 Suppose we want to perform the natural join we saw above and retain unmatched tuples from `RegularUser`

 RegularUser ⋈L  Major60sEvents

 For every user whose birth year matches the event year, we include a tuple for every match. Two users' birth years don't match any event year in the events table. We said we want to retain their information, and we can include it, but we must fill in the absent event information with NULL values. That NULL corresponds to the "outer" part of the result.

 ### Cartesian Product: X
 ![image](https://assets.omscs.io/notes/20221012222005.png)

A Cartesian product, A×B, results in a relation with N∗M tuples, where N is the number of tuples in A, and M is the number of tuples in B. For example, we can combine every tuple in RegularUser with every tuple in UserInterests:

RegularUser×UserInterests

### Devide By
![image](https://assets.omscs.io/notes/20221013155331.png)

Now let's look at the divide by algebra operator: 
÷. Suppose we want to find the email of all users with at least all the interests of user one.eg:

When we divide one relation by another, the divisor has attributes A and B, the dividend has attribute B, and the result has attribute A. We divide Email, Interest by Interest to produce 
Email here. The divide by operation always follows this structure.

### Rename
![image](https://assets.omscs.io/notes/20221012184357.png)

The last algebraic operator we examine is the rename operator, ρ. Renaming attributes and relations is useful when we want to perform various types of joins. Here is the basic syntax:


ρ New Relation Name[New Name Old Name, New Name Old Name, ...] (Old Relation Name)

## Relational calculus

In summary, atoms are one of the following:

- A range expression: t∈R
- A comparison of two attributes: r.A θ s.B
- A comparison of one attribute and a constant: r.A θ c.
- ∃(t∈R)(P(t)): there is a , then true
- ∀(t ∈R)(P(t)): all must satisfy, then true

### Calculus - Selection: Composite Expression & Projection.
![image](https://assets.omscs.io/notes/20221013182305.png)

Let's find all regular users who have the same current city and hometown or have a hometown of Atlanta:

### Calculus - Projection
![image](https://assets.omscs.io/notes/20221013183056.png)
Suppose we want to find the email, birth year, and sex for all regular users whose hometown is Atlanta. Here is that query:

{r.Email, r.BirthYear, r.Sex | r ∈ RegularUser and
 (r.CurrentCity=r.HomeTown or r.HomeTown=’Atlanta’)}

### Calculus - Union:
![image](https://assets.omscs.io/notes/20221013183636.png)

eg: {s.City | ∃(r∈ RegularUser)(s.City=r.CurrentCity) or ∃(t∈ RegularUser)(s.City=t.HomeTown)}

### Calculus - Intersection:
![image](https://assets.omscs.io/notes/20221014145645.png)
eg: {s.City | ∃(r∈ RegularUser)(s.City=r.CurrentCity) and
∃(t∈ RegularUser)(s.City=t.HomeTown)}

### Calculus - Set Difference:
![image](https://assets.omscs.io/notes/20221014150523.png)
eg: {s.City | ∃(r∈ RegularUser)(s.City=r.CurrentCity) and not
∃(t∈ RegularUser)(s.City=t.HomeTown)}

### Calculus - Natural Join:
![image](https://assets.omscs.io/notes/20221015105839.png)
eg: {t.Email, t.Year, t.Sex, t.Event | ∃(r∈RegularUser)∃(s∈Major60sEvents)
(r.Year = s.Year and t.Email = r.Email and t.Year = r.Year and t.Sex = r.Sex
 and t.Event = s.Event)}


### Calculus - Cartesian Product:
![image](https://assets.omscs.io/notes/20221014150954.png)

{r, s | r∈RegularUser and s∈UserInterests}

### Calculus - Divide By:
![image](https://assets.omscs.io/notes/20221016134441.png)

{r.Email | r ∈UserInterests and ∀(s∈UserInterests)((s.Email  !=’user1@gt.edu’) or 
∃(t∈UserInterests)(r.Email = t.Email and t.Interest = s.Interest))}
