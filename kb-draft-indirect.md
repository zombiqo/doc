# Indirect lineage #

- our input description format can be extended this way:
```
"analayzerOptions": {
  "showIndirectLineage": true,
  "indirectLineageOptions": {
    "wherClause" : true,
    "groupByClause" : true,
    "havingClause" : false,
    "semidirectLineage" : false,
    "semidirectCaseCondition" : true
      ....
  }
}
```
