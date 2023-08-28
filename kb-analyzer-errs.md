# Shrnutí chyb při analýze

```
$ grep "ANALYZER ERROR:" log | wc -l
194
```

("ANALYZER ERROR:" jsem si doplnil do výpisu dump.analazer.errors)

### 150 `ANALYZER ERROR: java.util.NoSuchElementException: key not found: ProjectionKey(`

Update: Asi FIXED commitem [ca66b19](https://github.com/zombiqo/dalineage/commit/ca66b19a52cd42517db39a38069d049be82962ef).

Např.:
```
ANALYZER [AUTO_CREATE] 8839/19160 #1/6 KB/moper_cmdb/view.V_MD_Object_Column.txt: FREE 1928 MAX 7168 TOTAL 3072
ERROR: java.util.NoSuchElementException: key not found: ProjectionKey(MOPER_CMDB.CONFIG_ITEM_REL[8833;311|17-311|43][CIR1])
```
Nechápu. V tom souboru je primitivní REPLACE VIEW AS SELECT. Žádný CONFIG_ITEM_REL tam není. Navíc ta chyba ukazuje na jiný source ID.

Ke zreprodukovaní stačí vzít samotný adresář `moper_cmdb`. Souvisí to s `Environment.lastAlias`.

### 18 `ANALYZER ERROR: scala.MatchError: TableFunctionCall(`

Např. na `StrTok_Split_To_Table` v `moper_uam/view.Arch_V_Super_Roles.txt`:
```sql
CREATE VIEW moper_uam.Arch_V_Super_Roles AS
SELECT r.System_Name, r.Role_Name AS Min_Role_Name, sup.Role_Name||'_supp_auto' AS Sup_Role_Name
FROM moper_uam.C_App_Role r
INNER JOIN(
        SELECT dt.token AS Role_Name
        FROM TABLE (StrTok_Split_To_Table (1, 'role_ibfs_ben_grp|role_ibfs_ben|role_ibfs_bfa|role_ibfs_civ|role_ibfs_cmr|role_ibfs_cog|role_ibfs_dza|role_ibfs_gha|role_ibfs_gin|role_ibfs_grp|role_ibfs_mar_bnk|role_ibfs_mar_grp|role_ibfs_mar_sgl|role_ibfs_mar_tos|role_ibfs_mc|role_ibfs_mdg|role_ibfs_mrt|role_ibfs_sen|role_ibfs_shr|role_ibfs_tcd|role_ibfs_tgo|role_mcdq', '|') RETURNS (outkey INTEGER, tokennum INTEGER, token VARCHAR(500) CHARACTER SET Unicode)) AS dt
        WHERE dt.token <> '') sup ON r.Role_Name LIKE sup.Role_Name||'%' AND NOT(r.Role_Name = 'role_ibfs_ben_grp_stage' AND sup.Role_Name = 'role_ibfs_ben') AND NOT r.Role_Name LIKE '%supp_auto'
WHERE r.System_Name = 'TdProd'
UNION
...
```

### 14 `ANALYZER ERROR: java.lang.AssertionError: assertion failed: START_TS`

Log první z těchto chyb:
```
ANALYZER [AUTO_CREATE] 5655/19160 #1/6 KB/min_dmm/view.V_PDM_Reference_Column.txt: FREE 2492 MAX 7168 TOTAL 3072
ERROR: java.lang.AssertionError: assertion failed: START_TS[5655;4|50-4|50]
primary List(ProjectionKey(JOIN-ON[5655;1|699-3|76]))
secondary List()
sources: 2 batch: db
List(
  LineageSource(
    datatype = DatatypeUnknown(str = "EVAL-CR"),
    tablename = BigQueryIdentifierSensitive(name = "JOIN-ON[5655;1|699-3|76]"),
    sourceName = TeradataIdentifierInsensitive(name = "Start_TS"),
    targetName = TeradataIdentifierInsensitive(name = "Start_TS"),
    tableAlias = Some(value = TableAlias(name = TeradataIdentifierInsensitive(name = "dv1"))),
    detail = NoDetail,
    joinAlias = Some(value = TableAlias(name = TeradataIdentifierInsensitive(name = "dv1")))
  ),
  LineageSource(
    datatype = DatatypeUnknown(str = "EVAL-CR"),
    tablename = BigQueryIdentifierSensitive(name = "JOIN-ON[5655;1|699-3|76]"),
    sourceName = TeradataIdentifierInsensitive(name = "Start_TS"),
    targetName = TeradataIdentifierInsensitive(name = "Start_TS"),
    tableAlias = Some(value = TableAlias(name = TeradataIdentifierInsensitive(name = "dv2"))),
    detail = NoDetail,
    joinAlias = Some(value = TableAlias(name = TeradataIdentifierInsensitive(name = "dv2")))
  )
)
```

Vypadá na "MFI" na `Start_TS` ve `WHERE`. Myslím, že nepozná, že je to alias selectovaného sloupce `dv1.Start_Version_TS as Start_TS`.

`min_dmm/view.V_PDM_Reference_Column.txt`:
```sql
replace view min_dmm.V_PDM_Reference_Column as
  select pdm1.Unique_Key,  pdm1.Branch_Id,  dv1.Start_Version_TS as Start_TS, coalesce(dv2.End_Version_TS,timestamp '2999-12-31 23:59:59') as End_TS,  pdm1.Start_Version, pdm1.End_Version,  pdm1.Model_Unique_Key,  pdm1.Model_Code,  pdm1.Package_Unique_Key,  pdm1.Package_Code,  pdm1.Reference_Unique_Key,  pdm1.Reference_Code,  pdm1.Reference_Join_Nbr,  pdm1.Parent_Object_Unique_Key,  pdm1.Parent_Object,  pdm1.Parent_Column_Unique_Key,  pdm1.Parent_Column,  pdm1.Child_Object_Unique_Key,  pdm1.Child_Object,  pdm1.Child_Column_Unique_Key,  pdm1.Child_Column,  pdm1.Creator_Name,  pdm1.Creation_TS,  pdm1.Modifier,  pdm1.Modification_TS,  pdm1.Overall_Modification_TS,  pdm1.Parent_Unique_Key from min_dmm.PDM_Reference_Column pdm1
  left join min_dmm.DMM_Version dv1 on pdm1.Start_Version = dv1.Version_Number
  left join min_dmm.DMM_Version dv2 on pdm1.End_Version = dv2.Version_Number
  where (pdm1.Unique_Key, pdm1.Branch_Id, Start_TS, pdm1.Start_Version) in
  (select Unique_Key, Branch_Id, Start_TS, Version_Pivot from
  (select pdm2.Unique_Key, pdm2.Branch_Id, dv3.Start_Version_TS as Start_TS, max(pdm2.Start_Version) as Version_Pivot
  from min_dmm.PDM_Reference_Column pdm2 left join min_dmm.DMM_Version dv3 on pdm2.Start_Version = dv3.Version_Number
  group by 1, 2, 3) dt1
  );
```

UPDATE: Ta chyba zmizí v dalším průběhu, protože mezitím dostane definici `min_dmm.DMM_Version`, takže už se tam ten sloupec nepokouší autocreatnout. Každopádně ten `Start_TS` na 5. řádku zůstane unresolved a to je chyba.

### 4 `ANALYZER ERROR: scala.MatchError: DeclareCursorVariable(`
### 3 `ANALYZER ERROR: scala.MatchError: Translate(`
FIXED ^^^
### 3 `ANALYZER ERROR: scala.MatchError: Right((ColumnReference(`
FIXED ^^^
### 2 `ANALYZER ERROR: scala.MatchError: (Date,String)`
