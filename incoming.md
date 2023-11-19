# Incoming format description #

## Minimal configuration ##
```
{
  "name": "My first batch",
  "dialect": "tsql",
  "sources": [ { "name": "." } ]
}
```

## Basic ooptions ##
- name: String. Name of the uploaded batch"
- dialect: SQL dialect"
### Supported dialects ###
- TSQL (MS Sql Server), Oracle, Teradata, Impala, BigQuery
