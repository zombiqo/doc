# Incoming format description #

## Minimal configuration ##
```
{
  "name": "My first batch",
  "dialect": "tsql",
  "sources": [ { "name": "." } ]
}
```

## Basic options ##
- name: String. Mandatory. Name of the uploaded batch"
- dialect: String. Mandatory. SQL dialect"
-- Supported dialects are: TSQL (MS Sql Server), Oracle, Teradata, Impala, BigQuery
- sources: List of source specifications
```
  {
    "name": ...          # dir or file entry inside incoming ZIP
    "defaultSchema": ... # optional
  }
```

## Analyzer options ###
```
{
  "analyzerOptions": { ... }
}
```
- autoCreate: Boolean. Default: false. Automatically create missing tables and views
- multiPass: Boolean. Default: false. Run analyzer in multiple passes to resolve more missing tables and views

- showQueryLineage: Boolean. Default: false. Show detailed query lineage
## Query lineage options ##
```
"queryLineageOptions": { ... }
```
- expandTables: Boolean. Default: false. Expand tables on diagram, otherwise only header is shown
- expandGroups: Boolean. Default: false. Expand groups on diagram, otherwise only header is shown
- groupExpandLevel: Int. Default: 0. Expand groups on diagram to this level. 0 means no limit
- showIndirectRelations: Boolean. Default: false.
  Show relations originated in WHERE/GROUP BY/HAVING/ORDER BY

