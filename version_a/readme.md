## Design
Use different element names for every level. e.g. have `Location000` and `Location100` and user can only specify one.

## Pros
- Xpaths are consistent no matter the level used. E.g. to get PostalCode it's auc:Building/auc:Location/auc:PostalCode no matter the level

## Cons
- xpaths are not consistent between levels. E.g. to get Postal code it's auc:Building/auc:Location000/auc:PostalCode for level 000, and auc:Building/auc:Location100/auc:PostalCode for level 100.

Note that we could add an attribute at the level of the root element which could indicate the versions used. This wouldn't really solve any problems but it might be convenient for some applications. This could lead to inconsistent states though. E.g.
```xml
<auc:Building
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:auc="http://buildingsync.net/schemas/bedes-auc/2019"
    xsi:schemaLocation="http://buildingsync.net/schemas/bedes-auc/2019 Building.xsd"
    auc:location="100",
    auc:form="000">
...
```
