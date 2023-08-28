# Shrnutí chyb parseru

Nejčastější chyby parser zjištěny z logu pomocí

```grep -n "PARSING ERROR:" log | sed 's/.* found //g' | sort | uniq -c | sort -n -r```

("`PARSING ERROR:`" jsem si doplnil do výpisu `dump.parsing.errors`)

Výsledek:

```
    331 "# Predpis "
    253 ":on error "
    157 "#requires "
     42 ".\n.SET SES"
     33 "##########"
     30 "RETLIMIT 0"
     13 "position('"
     10 "(cast(Time"
      6 "collect st"
      6 "#---------"
      5 "over (part" <=> FIXED
      5 "cast(0.001" <=> FIXED
      4 "PAR_LOGON_" <=> ???
      4 "(Master_Id"  <=> missing comma in expr
      3 "substr(bap" <=> FIXED
      3 "RETLIMIT\n\n" <=> FIXED
      3 "RETCANCEL " <=> FIXED
      3 "not betwee" <=> FIXED
      3 "cast(curre" <=> FIXED
      ? "select 1 from vt_View_Columns sample 1" <=> ???
      3 "$PSDefault" <=> *.ps1 files
      2 "when not m" <=> ??? merge into query - missing columns in whenInsertClause (only values are there)
      2 "tmp\nwhere " <=> WIP - `chrto/parser/fix_unpivot_clause`
      2 "substring(" <=> FIXED
      2 "substr(gc1" <=> FIXED
      2 "SUBSTR(c1." <=> FIXED
      2 "RETLIMIT *" <=> FIXED
      2 "position(a" <=> FIXED
      2 "over(parti" <=> FIXED
      2 "on substr(" <=> FIXED
      2 "#METASOL-4" <=> ???
      2 "# METASOL-" <=> ???
      2 "<!-- Infor" <=> *.dtd files
      2 "import arg" <=> *.py files
      2 "<ENDL> whe" <=> ???
      2 "create set" <=> FIXED
      2 "AS Tmp\n;\n\n"
      2 "as from mi"
      2 "abs(Object"
      2 "$infile='\\"
      1 "��\u0000#\u0000*\u0000*\u0000*"
      1 "<?xml vers"
      1 "trim(subst"
      1 "translate("
      1 "substr(ore"
      1 "substr(fd2"
      1 "substr(Err"
      1 "spadni del"
      1 "sample 1\n;"
      1 "RETLIMIT 1"
      1 "( position"
      1 ",\n    Wind"
      1 ",\n trim(su"
      1 ",\n  SUBSTR"
      1 ",\n substr("
      1 ",\n   subst"
      1 ",\n   csum("
      1 "<#\nConnect"
      1 ",\n case wh"
      1 ",\n       c"
      1 ",\n abs(eam"
      1 "(max(Text_"
      1 "iterate L1"
      1 ".EXPORT RE"
      1 "'E'\n      "
      1 "(cast(Proc"
      1 "(cast(curr"
      1 ",begin(pd)"
      1 "as Custom_"
      1 48707:PARSING ERROR: Internal parsing error [KB/MosControl/wf_mos_control_calc/SC_mccontrol_Check_Infra_Dev.txt]: java.lang.Exception: Unknown multiple expression list List(ColumnReference(List(ReferenceItem(TeradataIdentifierInsensitive(d1),None), ReferenceItem(TeradataIdentifierInsensitive(Schema_Name),None))), ColumnReference(List(ReferenceItem(TeradataIdentifierInsensitive(d1),None), ReferenceItem(TeradataIdentifierInsensitive(Object_Name),None))))
      1 48623:PARSING ERROR: Internal parsing error [KB/MosControl/wf_mos_control_calc/SC_mccontrol_Check_Infra.txt]: java.lang.Exception: Unknown multiple expression list List(ColumnReference(List(ReferenceItem(TeradataIdentifierInsensitive(d1),None), ReferenceItem(TeradataIdentifierInsensitive(Schema_Name),None))), ColumnReference(List(ReferenceItem(TeradataIdentifierInsensitive(d1),None), ReferenceItem(TeradataIdentifierInsensitive(Object_Name),None))))
      1 "(18)) * 60"
      1 "**********"
      1 "** *******"
      1 "#*********"
```

## Analýza nejčastějších chyb

```
    331 "# Predpis "
```
Soubor má vždy předponu `sed_` a obsahuje kromě komentářů s prefixem `#` řadky s příkazem pro cli utilitku `sed`

---
FIXED
```
    253 ":on error "
```
Příkaz pro `sqlcmd` SQL Serveru. To by náš TSQL parser zvládnul.

---

```
    157 "#requires "
```
V souborech s názvem ve tvaru `odwh-setProcessDate_wf_*.ps1`.

Např. obsah souboru `MolTeradata/odwh-setProcessDate_wf_mol_teradata_load_vcdt_TdProd.ps1`:

```
#requires -version 2.0

#*******************************************************************************
# Name:         odwh-setProcessDate
# Description:  předchozí byznys den
#
#*******************************************************************************

function spd_setProcessDate($iProcessName, $iProcessType) {
    return @"

insert into $TD_SP_O_DWH.Process_Date_Param (
        Process_Name
    ,   Process_Type_Code
    ,   Data_Area_Name
    ,   Param_Date
    ,   Param_Date_Weight
)
select
        '$iProcessName'
    ,   '$iProcessType'
    ,   null
    ,   date
    ,   1
;

"@
}
```

---
FIXED
```
     42 ".\n.SET SES"
```
Začátky těchto souborů jsou ve tvaru:
```
/********1*********2*********3*********4*********5*********6*********7*********8*********9*********/
/*  ...komentář vynechán...                                                                       */
/**************************************************************************************************/
.
.SET SESSION CHARSET 'utf8'
.LOGON '@Logon_trfddl_mol_dta'
```
Asi ta tečka na samostatném řádku vadí parseru...?

---
```
     33 "##########"
```

V souborech s názvy `check_SC_*_Snake_GEN.ps1`. Příklad `MolDMM/wf_mol_dmm_calc/check_SC_mcdmm_Snake_GEN.ps1`:

```
##########################
### Parameter settings ###
##########################

$targetFile="SC_mcdmm_Snake_GEN"
$folderName="MolDMM\wf_mol_dmm_calc"

####################
### Main Program ###
####################

Start-Sleep -Seconds 2

. $Env:PMCTLFILEDIR\Schedulers\checkBTEQ.ps1
Main
```

---
FIXED
```
     30 "RETLIMIT 0"
```

Chybí nám zpracování BTEQ příkazu jako je:
```
.SET RETLIMIT 0;
```
Trivka.

---
FIXED
```
     13 "position('"
```
Chyba na výrazech ve stylu:
```
     position(',' in Database_Column_Data_Type) > 0
```
Asi `in` v parametru nemáme podchyceno.

---

```
     10 "(cast(Time"
```

Příklad nezparsovaného výrazu:
```
 max(cast(cast(Time_Valid as date) as timestamp(0)) + (cast(Time_Valid as time(6) with time zone) - time '00:00:00' hour to second))
```
fixed ^
Asi ve všech případech stejný.

---

```
      6 "collect st"
```

Příkazy ve stylu:
```
collect stat on mcalc_cmdb.Config_Item_Rel;
```
fixed ^
---

```
      6 "#---------"
```
V souborech `MolDTA/wf_mol_dta_load_netvault/Netvault_*.ps1` a `MolDTA/wf_mol_dta_load_netvault/EnvModule/EnvModule.psm1`
To nebude SQL.

---

Takhle stačí, ne?
