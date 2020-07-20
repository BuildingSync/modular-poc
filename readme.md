Each directory contains a design approach for making BuildingSync more modular as well as using MLODs.

### General thoughts/notes
- What is the goal of modularization
  - reduce the burden of understanding what data is required
    - Is modularization the best approach for solving this problem? Alternatives: more documentation, more tooling, more examples
    - how would modularization accomplish this
      - The current schema is pretty complex and large. If a user wants to use it, it’s pretty much a requirement that they understand the whole schema and how it works. By scoping the amount of information they have to digest, you make their job easier.
    - technically speaking, how do you limit the scope? Ie **how/why will a user browsing the schema of a more modular design have an easier time making mappings to/from their data model to buildingsync’s**?
      - create MLoDs, and users should be able to focus on the level they care about (assumes they know what MLOD they want)
- What are the costs of modularization? Ie what do we lose (the user writing buildingsync, the applications using buildingsync, etc)
  - Gain some complexity through extensions - less centralized
- what do we gain
  - reusable components
  - clearer requirements - by breaking down the schema into mini-schemas we can have more stringent requirements, hopefully making writing and processing documents easier (less to understand for the user, fewer forks in possible data structures for processing)
- What’s the best way to modularize?
  - User centric (which I think is our primary goal)
    - first, who are our primary users? what is the primary use case in mind?
    - I would guess whatever structure maps best to their mental model of buildings is the approach you should take
  - Use case/application centric
    - what is the use case, and what would make it easiest to automate processing documents
  - standard centric
    - The ASHRAE 211 standard provides a pretty clear model of how building components are categorized - does that mapping make sense to users?
  - schema interoperability centric
    - how do other tools modularize, what are their components? Is there an approach that makes mapping from one to another easier?
    - Brick, IFC, etc


### key features of XML Schema used
#### types
Types should be used a lot more rather than defining element structures inline. The main benefit is that we can extend (or restrict) types but not elements. For example
```
<!--
    Location000Type requires:
        - CensusDivision
    Location100Type requires:
        - CensusDivision
        - PostalCode
-->
<xs:complexType name="Location000Type">
    <xs:sequence>
        <xs:element name="CensusDivision" type="auc:CensusDivisionType"></xs:element>
    </xs:sequence>
</xs:complexType>

<xs:complexType name="Location100Type">
    <xs:complexContent>
        <!-- creating a type that's a superset of L000 -->
        <xs:extension base="auc:Location000Type">
            <xs:sequence>
                <xs:element name="PostalCode" type="auc:PostalCodeType"></xs:element>
            </xs:sequence>
        </xs:extension>
    </xs:complexContent>
</xs:complexType>
```
This allows us to create a 'superset' of the elements defined in the base type which is what the MLOD does. We could also define the top level MLOD then restrict downwards (rather than extend off of the lowest), but I think this method is more verbose.

The downside is that I _think_ the ordering is maintained, so whatever you extend, the base elements must come first.
