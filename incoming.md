# Incoming format description #

## Minimal configuration ##
```
{
  "name": "My first batch",
  "dialect": "tsql",
  "sources": [ { "name": "." } ]
}
```

## Mandatory options ##
- name: String. Name of the uploaded batch"
- dialect: SQL dialect"
### Supported dialects ###
- TSQL (MS Sql Server), Oracle, Teradata, Impala, BigQuery

## Analyzer options ###
- autoCreate: Boolean. Default: false. Automatically create missing tables and views
- multiPass: Boolean. Default: false. Run analyzer in multiple passes to resolve more missing tables and views
