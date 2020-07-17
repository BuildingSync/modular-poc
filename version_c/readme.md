## Design
Define functions (location, form etc) for each level, then enumerate every possible combination of levels into a separate "base" schema. These base schemas are what's actually imported and validated. In other words, for each function-MLOD pair, create an xsd type for it. Then for every combination of these types (between functions), create a document which integrates them into a single schema.

I think the general naming strategy might be `BuildingSync_<Function>-<MLOD>[_<Function>-<MLOD>]*.xsd` where Function components are alphabetized.

Also we may want to consider adding versions to functional blocks if they might change independently

## Pros
- Xpaths are consistent no matter the level used. E.g. to get PostalCode it's auc:Building/auc:Location/auc:PostalCode no matter the level
- users only have to look at and care about one single schema

## Cons
- We have to create a lot of schemas to enumerate all possibilities (but we can automate this...)
  - generally, I think the total number of schemas is `product([n+1 for n in num_levels_of_each_function]) - 1` (+1 in product b/c we may choose not to include that function, -1 b/c we remove the empty set ie when no functions are chosen).
