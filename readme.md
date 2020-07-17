Each directory contains a design approach for making BuildingSync more modular as well as using MLODs.

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
