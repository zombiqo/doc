## Indirect lineage ##

- our input description format can be extended, everything can be configurable :
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
- what is an "indirect relation"?
```
INSERT INTO OUTTBL
SELECT A + C AS X FROM ( SELECT A, B, C FROM INTBL WHERE D > 0 )

INTBL.D --> INDIRECT --> A (inner select) --> A (outer select) --> X --> OUTTBL.X
INTBL.D --> INDIRECT --> B (inner select)
INTBL.D --> INDIRECT --> C (inner select) --> C (outer select) --> X --> OUTTBL.X
```


## Lineage clasification ##
- each relation consist of detailed part:
```
ISERT INTO OUTTBL
SELECT MAX(C) AS X FROM (SELECT A, B, C FROM INTBL)

INTBL.C --DIRECT--> INNER_SELECT.C --DIRECT--> OUTER_SELECT.C --AGGREGATE--> X --DIRECT--> OUTTBL.X
```
- composed relation is `INTBL.C --> OUTTBL.X` is classify with `Direct`, `Indirect`, `Aggregate` classes.


