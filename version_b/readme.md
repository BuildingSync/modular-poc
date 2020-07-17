## Design
The overall structure is defined and validated, but it is up to the user to provide `xsi:type` to specify the level the element should validate against.

## Pros
- Xpaths are consistent no matter the level used. E.g. to get PostalCode it's auc:Building/auc:Location/auc:PostalCode no matter the level

## Cons
- users must specify the `xsi:type` attribute on each element with levels.
- if users don't specify a level, then it will always result in the validation passing (the source element is of any type, so anything is valid)
