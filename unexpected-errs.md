# Lineage which has not been found by M, but found by Dalineage.

## Similar result in files like `./mcalc_cognos/view.V_Transf_*_ac.tx` :
```
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Cube_Deployment_Location_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Query_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Level_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Measure_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Dimension_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Level_Order_By_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Column_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Dimension_Drill_Down_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Package_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Cube_ac.txt"
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Model_ac.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/mcalc_cognos/view.V_Transf_Cube_Deployment_Location_ac.txt": {
    "ok": [
      "MCALC_COGNOS > V_C_PLATFORM_INSTANCE_AC > PLATFORM_INSTANCE_CODE -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > PLATFORM_INSTANCE_CODE"
    ],
    "missing": [],
    "unexpected": [
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_IBFS_AC > CUBE_DEPLOYMENTLOCATION -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > CUBE_DEPLOYMENTLOCATION",
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_IBFS_AC > CUBE_NAME -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > CUBE_NAME",
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_IBFS_AC > MODEL_NAME -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > MODEL_NAME",
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_KB_AC > CUBE_DEPLOYMENTLOCATION -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > CUBE_DEPLOYMENTLOCATION",
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_KB_AC > CUBE_NAME -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > CUBE_NAME",
      "MCALC_COGNOS > V_TRANSF_PROD_CUBE_DEPLOYMENT_LOCATION_KB_AC > MODEL_NAME -> MCALC_COGNOS > V_TRANSF_CUBE_DEPLOYMENT_LOCATION_AC > MODEL_NAME"
    ]
  }
```
#### Code
```
replace view mcalc_cognos.V_Transf_Cube_Deployment_Location_ac
as
select
 pic1.Platform_Instance_Code,
 Model_Name,
 Cube_Name,
 Cube_DeploymentLocation
from mcalc_cognos.V_Transf_Prod_Cube_Deployment_Location_KB_ac
 left join mcalc_cognos.V_C_Platform_Instance_ac pic1 on pic1.Platform_Instance_Alias = 'CgKBProd'

union all

select
 pic1.Platform_Instance_Code,
 Model_Name,
 Cube_Name,
 Cube_DeploymentLocation
from mcalc_cognos.V_Transf_Prod_Cube_Deployment_Location_IBFS_ac
 left join mcalc_cognos.V_C_Platform_Instance_ac pic1 on pic1.Platform_Instance_Alias = 'CgIBFSProd'
;
```
### Result
  - this lineage is expected

## Similar result in files like `./moper_*/view.V_Script_Rel.txt`:
```
  "test/kb_teradata/moper_ad/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_aqua/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_audit/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_bdsys/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_cmdb/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_cognos/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_common/view.V_Script_Rel.txt"
  "test/kb_teradata/moper_control/view.V_Script_Rel.txt"
  ...
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/moper_ad/view.V_Script_Rel.txt": {
    "ok": [
      "MOPER_AD > C_SOURCE > COMPANY_CODE -> MOPER_AD > V_SCRIPT_REL > COMPANY_CODE",
      "MOPER_AD > C_SOURCE > SOURCE_DESC -> MOPER_AD > V_SCRIPT_REL > SCRIPT_DESC",
      "MOPER_AD > C_SOURCE > SOURCE_ID -> MOPER_AD > V_SCRIPT_REL > SCRIPT_PATH",
      "MOPER_AD > C_SOURCE > SOURCE_ID -> MOPER_AD > V_SCRIPT_REL > SOURCE_ID",
      "MOPER_AD > C_SOURCE > SOURCE_NAME -> MOPER_AD > V_SCRIPT_REL > SCRIPT_NAME",
      "MOPER_AD > MD_SOURCE_REL > PARENT_SOURCE_ID -> MOPER_AD > V_SCRIPT_REL > PARENT_SOURCE_ID",
      "MOPER_AD > MD_SOURCE_REL > PARENT_SOURCE_ID -> MOPER_AD > V_SCRIPT_REL > SCRIPT_PATH"
    ],
    "missing": [],
    "unexpected": [
      "MOPER_AD > V_SCRIPT_REL > SCRIPT_LEVEL -> MOPER_AD > V_SCRIPT_REL > SCRIPT_LEVEL",
      "MOPER_AD > V_SCRIPT_REL > SCRIPT_PATH -> MOPER_AD > V_SCRIPT_REL > SCRIPT_PATH"
    ]
  }
```
#### Code
```
replace recursive view moper_ad.V_Script_Rel
(
Script_Level,
Source_Id,
Parent_Source_Id,
Company_Code,
Script_Name,
Script_Desc,
Script_Path
)
as
(
select
 1 as Script_Level,
 cs1.Source_Id,
 msr1.Parent_Source_Id,
 cs1.Company_Code,
 cs1.Source_Name as Script_Name,
 cs1.Source_Desc as Script_Desc,
 cast(trim(msr1.Parent_Source_Id) || '_' || trim(cs1.Source_Id) as varchar(512)) as Script_Path
from moper_ad.MD_Source_Rel msr1
 inner join moper_ad.C_Source cs1 on msr1.Source_Id = cs1.Source_Id
where msr1.Parent_Source_Id = 0

union all

select
 vsm1.Script_Level + 1 as Script_Level,
 cs1.Source_Id,
 msr1.Parent_Source_Id,
 cs1.Company_Code,
 cs1.Source_Name as Script_Name,
 cs1.Source_Desc as Script_Desc,
 vsm1.Script_Path || '_' || trim(cs1.Source_Id) as Script_Path
from V_Script_Rel vsm1
 inner join moper_ad.MD_Source_Rel msr1 on vsm1.Source_Id = msr1.Parent_Source_Id
 inner join moper_ad.C_Source cs1 on msr1.Source_Id = cs1.Source_Id
where vsm1.Script_Level < 100
);
```
### Result
  - this lineage is expected

## Similar result in files like `./*/view.*_MD_Index.txt.txt` :
```
  "test/kb_teradata/mcalc_control/view.V_MOP_MD_Index.txt"
  "test/kb_teradata/moper_ad/view.V_MD_Index.txt"
  "test/kb_teradata/moper_aqua/view.V_MD_Index.txt"
  "test/kb_teradata/moper_audit/view.V_MD_Index.txt"
  "test/kb_teradata/moper_bdsys/view.V_MD_Index.txt"
  "test/kb_teradata/moper_cmdb/view.V_MD_Index.txt"
  "test/kb_teradata/moper_cognos/view.V_MD_Index.txt"
  "test/kb_teradata/moper_common/view.V_MD_Index.txt"
  "test/kb_teradata/moper_control/view.V_MD_Index.txt"
  ...
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
```
#### Code
```
```
### Result
  - requires detailed analysis






## Similar result in files like `./mcalc_jira/view.V_Issue_Detail.txt`:
```
  "test/kb_teradata/mcalc_jira/view.V_Issue_Detail.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/mcalc_jira/view.V_Issue_Detail.txt": {
    "ok": [
      "MCALC_JIRA > V_JIRA_C_ISSUE_PRIORITY_AC > PRIORITY_DESC -> MCALC_JIRA > V_ISSUE_DETAIL > PRIORITY_DESC",
      "MCALC_JIRA > V_JIRA_C_ISSUE_PRIORITY_AC > PRIORITY_NAME -> MCALC_JIRA > V_ISSUE_DETAIL > PRIORITY_NAME",
      "MCALC_JIRA > V_JIRA_C_ISSUE_STATUS_AC > STATUS_DESC -> MCALC_JIRA > V_ISSUE_DETAIL > STATUS_DESC",
      "MCALC_JIRA > V_JIRA_C_ISSUE_STATUS_AC > STATUS_NAME -> MCALC_JIRA > V_ISSUE_DETAIL > STATUS_NAME",
      "MCALC_JIRA > V_JIRA_C_ISSUE_TYPE_AC > ISSUE_TYPE_DESC -> MCALC_JIRA > V_ISSUE_DETAIL > ISSUE_TYPE_DESC",
      "MCALC_JIRA > V_JIRA_C_ISSUE_TYPE_AC > ISSUE_TYPE_NAME -> MCALC_JIRA > V_ISSUE_DETAIL > ISSUE_TYPE_NAME",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > ASSIGNEE_ID -> MCALC_JIRA > V_ISSUE_DETAIL > ASSIGNEE_ID",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > CREATED_TIME -> MCALC_JIRA > V_ISSUE_DETAIL > CREATED_TIME",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > DUE_DATE_TIME -> MCALC_JIRA > V_ISSUE_DETAIL > DUE_DATE_TIME",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > ISSUE_DESC -> MCALC_JIRA > V_ISSUE_DETAIL > ISSUE_DESC",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > PROJECT_KEY -> MCALC_JIRA > V_ISSUE_DETAIL > PROJECT_KEY",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > REPORTER_ID -> MCALC_JIRA > V_ISSUE_DETAIL > REPORTER_ID",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > RESOLVED_TIME -> MCALC_JIRA > V_ISSUE_DETAIL > RESOLVED_TIME",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > SUMMARY_TITLE -> MCALC_JIRA > V_ISSUE_DETAIL > SUMMARY_TITLE",
      "MCALC_JIRA > V_JIRA_ISSUE_AC > TICKET_ID -> MCALC_JIRA > V_ISSUE_DETAIL > TICKET_ID"
    ],
    "missing": [
      "MCALC_JIRA > V_JIRA_CUSTOM_FIELD_VALUE_AC > PARENT_KEY -> MCALC_JIRA > V_ISSUE_DETAIL > PARENT_KEY",
      "MCALC_JIRA > V_JIRA_CUSTOM_FIELD_VALUE_AC > STRING_VALUE -> MCALC_JIRA > V_ISSUE_DETAIL > JIRA_SOLVER_GROUP"
    ],
    "unexpected": [
      "MCALC_JIRA > CUSTOM_FIELD_VALUE > PARENT_KEY -> MCALC_JIRA > V_ISSUE_DETAIL > PARENT_KEY",
      "MCALC_JIRA > CUSTOM_FIELD_VALUE > STRING_VALUE -> MCALC_JIRA > V_ISSUE_DETAIL > JIRA_SOLVER_GROUP"
    ]
  }
```
#### Code
```
replace view mcalc_jira.V_Issue_Detail as
select i1.Ticket_Id
,i1.Project_Key
,i1.Reporter_Id
,i1.Assignee_Id
,it1.issue_type_name
,it1.issue_type_desc
,i1.Summary_Title
,i1.Issue_Desc
,ip1.priority_name
,ip1.priority_desc
,is1.status_name
,is1.status_desc
,i1.Created_Time
,i1.Due_Date_Time
,i1.Resolved_Time
,cfv1.Parent_Key
,cfv1.String_Value as JIRA_Solver_Group
from
mcalc_jira.V_JIRA_Issue_ac i1
left join mcalc_jira.Custom_Field_Value cfv1
	on cfv1.issue_id = i1.issue_id
left join mcalc_jira.V_JIRA_C_custom_field_ac cf1
	on cf1.custom_field_id = cfv1.custom_field_id
left join mcalc_jira.V_JIRA_Custom_Field_Option_ac cfo1
	on cfo1.custom_field_id = cf1.custom_field_id
left join mcalc_jira.V_JIRA_Project_ac p1
	on p1.project_id = i1.project_id
left join mcalc_jira.V_JIRA_C_Issue_Type_ac it1
	on it1.issue_type_id = i1.type_id
left join mcalc_jira.V_JIRA_C_Issue_Status_ac is1
	on is1.status_id = i1.status_id
left join mcalc_jira.V_JIRA_C_Issue_Priority_ac ip1
	on ip1.priority_id = i1.priority_id
where cf1.custom_field_id in (33021) /*Řešitelská skupina*/;
```
### Result
  - Lineage is expected

## Similar result in files like `./mcalc_techdw/view.V_Decomm_Overall_Value*_ac.txt`:
```
  "test/kb_teradata/mcalc_techdw/view.V_Decomm_Overall_Value_morepos_ac.txt"
  "test/kb_teradata/mcalc_techdw/view.V_Decomm_Overall_Value_ac.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/mcalc_techdw/view.V_Decomm_Overall_Value_morepos_ac.txt": {
    "ok": [
      "MCALC_TECHDW > V_MD_DATE > DATE_VALID -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > DATE_VALID"
    ],
    "missing": [],
    "unexpected": [
      "MIN_TECHDW > H_DECOMM_OVERALL_VALUE_MOREPOS > COMMENT_STRING -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > COMMENT_STRING",
      "MIN_TECHDW > H_DECOMM_OVERALL_VALUE_MOREPOS > HIGH_THOLD -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > HIGH_THOLD",
      "MIN_TECHDW > H_DECOMM_OVERALL_VALUE_MOREPOS > LOW_THOLD -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > LOW_THOLD",
      "MIN_TECHDW > H_DECOMM_OVERALL_VALUE_MOREPOS > METRIC_VALUE -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > METRIC_VALUE",
      "MIN_TECHDW > H_DECOMM_OVERALL_VALUE_MOREPOS > VALUE_ID -> MCALC_TECHDW > V_DECOMM_OVERALL_VALUE_MOREPOS_AC > VALUE_ID"
    ]
  }
```
#### Code
```
replace view mcalc_techdw.V_Decomm_Overall_Value_morepos_ac as
   /* puvodni zaznamy z Hist nedotcene korekcemi */
   select
     data1.Value_ID,
     tim1.Date_Valid,
     data1.Metric_Value,
     data1.Low_Thold,
     data1.High_Thold,
     data1.Comment_String
   from min_techdw.H_Decomm_Overall_Value_morepos data1
   cross join mcalc_techdw.V_MD_Date tim1
    where tim1.Date_Valid between data1.Start_Date and data1.End_Date
   ;
```
### Result
  - lineage is expected from data1

## Similar result in files like `./min_*/view.V_Columns.txt`:
```
  "test/kb_teradata/min_ad/view.V_Columns.txt"
  "test/kb_teradata/min_aqua/view.V_Columns.txt"
  "test/kb_teradata/min_audit/view.V_Columns.txt"
  "test/kb_teradata/min_bdsys/view.V_Columns.txt"
  "test/kb_teradata/min_cmdb/view.V_Columns.txt"
  "test/kb_teradata/min_cognos/view.V_Columns.txt"
  "test/kb_teradata/min_common/view.V_Columns.txt"
  "test/kb_teradata/min_control/view.V_Columns.txt"
  "test/kb_teradata/min_dacan/view.V_Columns.txt"
  "test/kb_teradata/min_dali/view.V_Columns.txt"
  "test/kb_teradata/min_dapro/view.V_Columns.txt"
  "test/kb_teradata/min_dbms/view.V_Columns.txt"
  "test/kb_teradata/min_dmm/view.V_Columns.txt"
  "test/kb_teradata/min_doc/view.V_Columns.txt"
  "test/kb_teradata/min_dta/view.V_Columns.txt"
  "test/kb_teradata/min_dynamit/view.V_Columns.txt"
  "test/kb_teradata/min_ea/view.V_Columns.txt"
  "test/kb_teradata/min_ifpc/view.V_Columns.txt"
  "test/kb_teradata/min_iftdm/view.V_Columns.txt"
  "test/kb_teradata/min_jira/view.V_Columns.txt"
  "test/kb_teradata/min_manta/view.V_Columns.txt"
  "test/kb_teradata/min_mssql/view.V_Columns.txt"
  "test/kb_teradata/min_mtools/view.V_Columns.txt"
  "test/kb_teradata/min_oracle/view.V_Columns.txt"
  "test/kb_teradata/min_pms/view.V_Columns.txt"
  "test/kb_teradata/min_repositories/view.V_Columns.txt"
  "test/kb_teradata/min_techdw/view.V_Columns.txt"
  "test/kb_teradata/min_teradata/view.V_Columns.txt"
  "test/kb_teradata/min_uam/view.V_Columns.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
 "test/kb_teradata/min_ad/view.V_Columns.txt": {
    "ok": [
      "R_TERADATA > C_TABLE_COLUMN_TYPE > TABLE_COLUMN_TYPE -> MIN_AD > V_COLUMNS > COLUMN_DATA_TYPE",
      "R_TERADATA > V_COLUMNS > CHAR_TYPE -> MIN_AD > V_COLUMNS > CHARACTER_SET",
      "R_TERADATA > V_COLUMNS > CHAR_TYPE -> MIN_AD > V_COLUMNS > CHAR_TYPE",
      "R_TERADATA > V_COLUMNS > COLUMN_FORMAT -> MIN_AD > V_COLUMNS > COLUMN_FORMAT",
      "R_TERADATA > V_COLUMNS > COLUMN_ID -> MIN_AD > V_COLUMNS > COLUMN_ID",
      "R_TERADATA > V_COLUMNS > COLUMN_LENGTH -> MIN_AD > V_COLUMNS > COLUMN_LENGTH",
      "R_TERADATA > V_COLUMNS > COLUMN_LENGTH -> MIN_AD > V_COLUMNS > MAX_LENGTH",
      "R_TERADATA > V_COLUMNS > COLUMN_NAME -> MIN_AD > V_COLUMNS > COLUMN_NAME",
      "R_TERADATA > V_COLUMNS > COLUMN_TYPE -> MIN_AD > V_COLUMNS > COLUMN_TYPE",
      "R_TERADATA > V_COLUMNS > COMMENT_STRING -> MIN_AD > V_COLUMNS > COMMENT_STRING",
      "R_TERADATA > V_COLUMNS > DATABASE_NAME -> MIN_AD > V_COLUMNS > SCHEMA_NAME",
      "R_TERADATA > V_COLUMNS > DECIMAL_FRACTIONAL_DIGITS -> MIN_AD > V_COLUMNS > COLUMN_PRECISION",
      "R_TERADATA > V_COLUMNS > DECIMAL_FRACTIONAL_DIGITS -> MIN_AD > V_COLUMNS > DECIMAL_FRACTIONAL_DIGITS",
      "R_TERADATA > V_COLUMNS > DECIMAL_TOTAL_DIGITS -> MIN_AD > V_COLUMNS > DECIMAL_TOTAL_DIGITS",
      "R_TERADATA > V_COLUMNS > DEFAULT_VALUE -> MIN_AD > V_COLUMNS > DEFAULT_VALUE",
      "R_TERADATA > V_COLUMNS > NULLABLE_FLAG -> MIN_AD > V_COLUMNS > NULLABLE_FLAG",
      "R_TERADATA > V_COLUMNS > TABLE_NAME -> MIN_AD > V_COLUMNS > TABLE_NAME",
      "R_TERADATA > V_COLUMNS > UPPERCASE_FLAG -> MIN_AD > V_COLUMNS > UPPERCASE_FLAG"
    ],
    "missing": [],
    "unexpected": [
      "R_TERADATA > C_TABLE_COLUMN_TYPE > TABLE_COLUMN_TYPE -> MIN_AD > V_COLUMNS > COLUMN_PRECISION",
      "R_TERADATA > C_TABLE_COLUMN_TYPE > TABLE_COLUMN_TYPE -> MIN_AD > V_COLUMNS > MAX_LENGTH",
      "R_TERADATA > V_COLUMNS > CHARACTER_SET -> MIN_AD > V_COLUMNS > MAX_LENGTH"
    ]
  }
```
#### Code
```
replace view V_Columns
as
select
 col1.Database_Name as Schema_Name,
 col1.Table_Name,
 col1.Column_Name,
 col1.Column_Format,
 col1.Column_Type,
 col1.Column_Length,
 col1.Default_Value,
 col1.Nullable_Flag,
 col1.Comment_String,
 col1.Decimal_Total_Digits,
 col1.Decimal_Fractional_Digits,
 col1.Column_Id,
 col1.UpperCase_Flag,
 col1.Char_Type,
 ft1.Table_Column_Type as Column_Data_Type,
 case col1.Char_Type
  when 1 then 'Latin'
  when 2 then 'Unicode'
  else null
 end as Character_Set,
 case
  when trim(Column_Data_Type) in ('char', 'varchar')
   and Character_Set = 'Unicode' then col1.Column_length / 2
  else col1.Column_length
 end as Max_Length,
 case
  when Column_Data_Type in ('decimal') then col1.Decimal_Fractional_Digits
  when Column_Data_Type in ('char', 'varchar') then 0
  when Column_Data_Type in ('date', 'integer', 'smallint', 'bigint') then 0
  when Column_Data_Type in ('timestamp') then 0
 end as Column_Precision
from r_teradata.V_Columns col1
 left outer join r_teradata.C_Table_Column_Type ft1 on col1.Column_Type = ft1.Table_Column_Type_Code
;

```
### Result
  patria tam, ale je nutne preskumat vazby [???]:

## Similar result in files like `./min_*/view.V_C_Squad_mocommon.txt`
```
  "test/kb_teradata/min_audit/view.V_C_Squad_mocommon.txt"
  "test/kb_teradata/min_ea/view.V_C_Squad_mocommon.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/min_audit/view.V_C_Squad_mocommon.txt": {
    "ok": [
      "MOUT_COMMON > V_C_SQUAD > AGILE_UNIT_CODE -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > SQUAD_CODE",
      "MOUT_COMMON > V_C_SQUAD > JIRA_SOLVER_GROUP -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > JIRA_SOLVER_GROUP",
      "MOUT_COMMON > V_C_SQUAD > SUPERIOR_AGILE_UNIT_CODE -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > DEPARTMENT_CODE",
      "MOUT_COMMON > V_C_SQUAD > UNIT_COACH_ID -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > COACH_ID",
      "MOUT_COMMON > V_C_SQUAD > UNIT_DESC -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > SQUAD_DESC",
      "MOUT_COMMON > V_C_SQUAD > UNIT_LEADER_ID -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > PRODUCT_OWNER_ID",
      "MOUT_COMMON > V_C_SQUAD > UNIT_NAME -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > SQUAD_NAME"
    ],
    "missing": [
      "MOUT_COMMON > V_C_SQUAD > UNIT_EMAIL -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > SQUAD_EMAIL"
    ],
    "unexpected": [
      "MOUT_COMMON > V_C_SQUAD > AGILE_UNIT_EMAIL -> MIN_AUDIT > V_C_SQUAD_MOCOMMON > SQUAD_EMAIL"
    ]
```
#### Code
```
replace view min_audit.V_C_Squad_mocommon as
   select
   Agile_Unit_Code as Squad_Code,
   Superior_Agile_Unit_Code as Department_Code,
   Unit_Name as Squad_Name,
   Agile_Unit_Email as Squad_Email,
   Unit_Leader_Id as Product_Owner_Id,
   Unit_Coach_Id as Coach_Id,
   src1.Jira_Solver_Group as Jira_Solver_Group,
   Unit_Desc as Squad_Desc
   from mout_common.V_C_Squad src1;
```
### Result
  - [OK]

## Similar result in files like `./mout_*/*.txt`:
```
  "test/kb_teradata/mout_ad/view.V_CG_IBFS_Group.txt"
  "test/kb_teradata/mout_audit/view.V_DM_Report_Usage.txt"
  "test/kb_teradata/mout_cmdb/view.V_Config_Item_Tree_Gestor.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/mout_ad/view.V_CG_IBFS_Group.txt": {
    "ok": [
      "MTRF_AD > H_EA_GROUP > AD_DISTINGUISHED_NAME -> MOUT_AD > V_CG_IBFS_GROUP > AD_DISTINGUISHED_NAME",
      "MTRF_AD > H_EA_GROUP > AD_GROUP_DESC -> MOUT_AD > V_CG_IBFS_GROUP > AD_GROUP_DESC",
      "MTRF_AD > H_EA_GROUP > AD_GROUP_ID -> MOUT_AD > V_CG_IBFS_GROUP > AD_GROUP_ID",
      "MTRF_AD > H_EA_GROUP > AD_GROUP_NAME -> MOUT_AD > V_CG_IBFS_GROUP > AD_GROUP_NAME",
      "MTRF_AD > H_EA_GROUP > AD_PARENT_DISTINGUISHED_NAME -> MOUT_AD > V_CG_IBFS_GROUP > AD_PARENT_DISTINGUISHED_NAME"
    ],
    "missing": [],
    "unexpected": [
      "MTRF_AD > H_EA_GROUP > AD_DISTINGUISHED_NAME -> MOUT_AD > V_CG_IBFS_GROUP > AD_PARENT_DISTINGUISHED_NAME"
    ]
  }
```
#### Code
```
replace view mout_ad.V_CG_IBFS_Group as
select
 veag1.AD_Group_Id,
 veag1.AD_Group_Name,
 veag1.AD_Distinguished_Name,
 veag1.AD_Group_Desc,
 case
  when veag1.AD_Parent_Distinguished_Name = dt1.AD_Distinguished_Name then veag1.AD_Parent_Distinguished_Name
  -- pokud neexistuje parent, nahrad parent NULLem
  else null
 end as AD_Parent_Distinguished_Name
from mtrf_ad.H_EA_Group veag1
 left join
 -- join jeste jednou to same kvuli existenci AD_Parent_Distinguished_Name ve vyberu
 (
  select
   veag3.AD_Distinguished_Name
  from mtrf_ad.H_EA_Group veag3
  where veag3.AD_Group_Type = 'group'
   and veag3.AD_Distinguished_Name like '%OU=CognosGroup,OU=BI-SME,OU=Groups,OU=SC1,DC=one,DC=ea,DC=socgen%'
   and veag3.AD_Group_Name in
    --vyhodi se zaznamy, ktere jsou vickrat, vsechny
    (select
      veag4.AD_Group_Name
     from mout_ad.V_EA_Group veag4
     where veag4.AD_Group_Type = 'group'
      and veag4.AD_Distinguished_Name like '%OU=CognosGroup,OU=BI-SME,OU=Groups,OU=SC1,DC=one,DC=ea,DC=socgen%'
     group by 1
     having count(veag4.AD_Group_Name) = 1
    )
	and veag3.End_TS= timestamp '2999-12-31 23:59:59'
 ) dt1 on veag1.AD_Parent_Distinguished_Name = dt1.AD_Distinguished_Name
-- konec vnitrniho joinu
where veag1.AD_Group_Type = 'group'
 and veag1.AD_Distinguished_Name like '%OU=CognosGroup,OU=BI-SME,OU=Groups,OU=SC1,DC=one,DC=ea,DC=socgen%'
 and veag1.AD_Group_Name in
  --vyhodi se zaznamy, ktere jsou vickrat, vsechny
  (select
    veag2.AD_Group_Name
   from mtrf_ad.H_EA_Group veag2
   where veag2.AD_Group_Type = 'group'
    and veag2.AD_Distinguished_Name like '%OU=CognosGroup,OU=BI-SME,OU=Groups,OU=SC1,DC=one,DC=ea,DC=socgen%'
	and veag2.End_TS= timestamp '2999-12-31 23:59:59'
   group by 1
   having count(veag2.AD_Group_Name) = 1
  )
  and veag1.End_TS= timestamp '2999-12-31 23:59:59';
```
### Result
  -[OK]
  - is in when condition

## Similar result in files like `./min_common/view.V_H_Employee_mchr.txt`:
```
  "test/kb_teradata/min_common/view.V_H_Employee_mchr.txt"
```
### Example
#### In "ok" is lineage, which has been found by M and Dalineage as well. In "unexpected" is lineage which has not been found by M, but found by Dalineage.
```
  "test/kb_teradata/min_common/view.V_H_Employee_mchr.txt": {
    "ok": [
      "MCHR_M_KB > V_H_EMPLOYEE > ALIAS_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > ALIAS_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > BIRTH_NBR -> MIN_COMMON > V_H_EMPLOYEE_MCHR > BIRTH_NBR",
      "MCHR_M_KB > V_H_EMPLOYEE > COMPANY_CODE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > COMPANY_CODE",
      "MCHR_M_KB > V_H_EMPLOYEE > COUNTRY_CODE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > COUNTRY_CODE",
      "MCHR_M_KB > V_H_EMPLOYEE > DATE_OF_BIRTH -> MIN_COMMON > V_H_EMPLOYEE_MCHR > DATE_OF_BIRTH",
      "MCHR_M_KB > V_H_EMPLOYEE > DATE_OF_PENSION_ENTITLEMENT -> MIN_COMMON > V_H_EMPLOYEE_MCHR > DATE_OF_PENSION_ENTITLEMENT",
      "MCHR_M_KB > V_H_EMPLOYEE > DOMAIN_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > DOMAIN_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > EMAIL_ADDRESS -> MIN_COMMON > V_H_EMPLOYEE_MCHR > EMAIL_ADDRESS",
      "MCHR_M_KB > V_H_EMPLOYEE > EMPLOYEE_EGJE_ID -> MIN_COMMON > V_H_EMPLOYEE_MCHR > EMPLOYEE_EGJE_ID",
      "MCHR_M_KB > V_H_EMPLOYEE > EMPLOYEE_ID -> MIN_COMMON > V_H_EMPLOYEE_MCHR > EMPLOYEE_ID",
      "MCHR_M_KB > V_H_EMPLOYEE > END_DATE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > END_DATE",
      "MCHR_M_KB > V_H_EMPLOYEE > FAMILY_STATUS -> MIN_COMMON > V_H_EMPLOYEE_MCHR > FAMILY_STATUS",
      "MCHR_M_KB > V_H_EMPLOYEE > FIRST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > FIRST_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > FIRST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > FIRST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_3",
      "MCHR_M_KB > V_H_EMPLOYEE > GENDER_CODE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > GENDER_CODE",
      "MCHR_M_KB > V_H_EMPLOYEE > GESOP_AGREEMENT -> MIN_COMMON > V_H_EMPLOYEE_MCHR > GESOP_AGREEMENT",
      "MCHR_M_KB > V_H_EMPLOYEE > GGI_CODE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > GGI_CODE",
      "MCHR_M_KB > V_H_EMPLOYEE > GROUP_ID -> MIN_COMMON > V_H_EMPLOYEE_MCHR > GROUP_ID",
      "MCHR_M_KB > V_H_EMPLOYEE > HI_FROM -> MIN_COMMON > V_H_EMPLOYEE_MCHR > HI_FROM",
      "MCHR_M_KB > V_H_EMPLOYEE > HI_HOLDER_NUMBER -> MIN_COMMON > V_H_EMPLOYEE_MCHR > HI_HOLDER_NUMBER",
      "MCHR_M_KB > V_H_EMPLOYEE > HI_TO -> MIN_COMMON > V_H_EMPLOYEE_MCHR > HI_TO",
      "MCHR_M_KB > V_H_EMPLOYEE > HOME_OFFICE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > HOME_OFFICE",
      "MCHR_M_KB > V_H_EMPLOYEE > LAST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > LAST_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > LAST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > LAST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_3",
      "MCHR_M_KB > V_H_EMPLOYEE > LEGAL_CAPACITY -> MIN_COMMON > V_H_EMPLOYEE_MCHR > LEGAL_CAPACITY",
      "MCHR_M_KB > V_H_EMPLOYEE > LOGIN_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > LOGIN_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > MAIDEN_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > MAIDEN_NAME",
      "MCHR_M_KB > V_H_EMPLOYEE > MED_INS_COMPANY_NUMBER -> MIN_COMMON > V_H_EMPLOYEE_MCHR > MED_INS_COMPANY_NUMBER",
      "MCHR_M_KB > V_H_EMPLOYEE > NOT_SEND_OKO -> MIN_COMMON > V_H_EMPLOYEE_MCHR > NOT_SEND_OKO",
      "MCHR_M_KB > V_H_EMPLOYEE > PLACE_OF_BIRTH -> MIN_COMMON > V_H_EMPLOYEE_MCHR > PLACE_OF_BIRTH",
      "MCHR_M_KB > V_H_EMPLOYEE > SOCIAL_STATUS -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SOCIAL_STATUS",
      "MCHR_M_KB > V_H_EMPLOYEE > START_DATE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > START_DATE",
      "MCHR_M_KB > V_H_EMPLOYEE > TITLE_AFTER -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_3",
      "MCHR_M_KB > V_H_EMPLOYEE > TITLE_AFTER -> MIN_COMMON > V_H_EMPLOYEE_MCHR > TITLE_AFTER",
      "MCHR_M_KB > V_H_EMPLOYEE > TITLE_BEFORE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_3",
      "MCHR_M_KB > V_H_EMPLOYEE > TITLE_BEFORE -> MIN_COMMON > V_H_EMPLOYEE_MCHR > TITLE_BEFORE",
      "MCHR_M_KB > V_H_EMPLOYEE > VIRTUAL_USER_FLG -> MIN_COMMON > V_H_EMPLOYEE_MCHR > VIRTUAL_USER_FLG"
    ],
    "missing": [
      "MCHR_M_KB > V_H_EMPLOYEE > SHORT_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_2"
    ],
    "unexpected": [
      "MCHR_M_KB > V_H_EMPLOYEE > FIRST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_2",
      "MCHR_M_KB > V_H_EMPLOYEE > LAST_NAME -> MIN_COMMON > V_H_EMPLOYEE_MCHR > SHORT_NAME_2"
    ]
  }
```
#### Code
```
replace view min_common.V_H_Employee_mchr as
   select
   src1.Employee_Id as Employee_Id,
   src1.Company_Code as Company_Code,
   src1.Start_Date as Start_Date,
   src1.End_Date as End_Date,
   src1.Employee_EGJE_Id as Employee_EGJE_Id,
   src1.Last_Name || ' ' || src1.First_Name as Short_Name,
   src1.Short_Name as Short_Name_2,
   src1.Last_Name || ' ' || src1.First_Name || case when src1.Title_Before <> '' then ', ' else '' end || src1.Title_Before || case when src1.Title_After <> '' then ', ' else '' end || src1.Title_After as Short_Name_3,
   src1.Last_Name as Last_Name,
   src1.First_Name as First_Name,
   src1.Gender_Code as Gender_Code,
   src1.Birth_Nbr as Birth_Nbr,
   src1.Title_Before as Title_Before,
   src1.Title_After as Title_After,
   src1.Date_Of_Birth as Date_Of_Birth,
   src1.Domain_Name as Domain_Name,
   src1.Login_Name as Login_Name,
   src1.GGI_Code as GGI_Code,
   src1.Family_Status as Family_Status,
   src1.Alias_Name as Alias_Name,
   src1.Maiden_Name as Maiden_Name,
   src1.Place_Of_Birth as Place_Of_Birth,
   src1.Country_Code as Country_Code,
   src1.Social_Status as Social_Status,
   src1.Legal_Capacity as Legal_Capacity,
   src1.Med_Ins_Company_Number as Med_Ins_Company_Number,
   src1.HI_Holder_Number as HI_Holder_Number,
   src1.HI_From as HI_From,
   src1.HI_To as HI_To,
   src1.Date_Of_Pension_Entitlement as Date_Of_Pension_Entitlement,
   src1.Not_Send_Oko as Not_Send_Oko,
   src1.Gesop_Agreement as Gesop_Agreement,
   src1.Home_Office as Home_Office,
   src1.Group_Id as Group_Id,
   src1.Email_Address as Email_Address,
   src1.Virtual_User_Flg as Virtual_User_Flg
   from mchr_m_kb.V_H_Employee src1
   ;
```
### Result
  - wrong lineage here. SHORT_NAME comme from src table not from alias [!!!]
