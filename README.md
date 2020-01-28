![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Use SQL To Find Granted SQL Permissions Per SQL User
**Post Date: February 27, 2015**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>Here's some quick SQL logic to get a list of permissions per account and roles that were granted per account.
If you are dropping an account with for example: drop login [int\myaccount], and you run into an error where the account was used to grant permissions to some other database object, then you'll need to go about finding those permissions, but I would not recommend using the GUI to determine where those grants were done.
To find SQL GRANTS which were explicitly given per account.</p>   


## SQL-Logic
```SQL
use master;
set nocount on
 
select
'account name' = servprin.name
,   'type' = servprin.type_desc
,   'permission' = servperm.permission_name
,   'state' = servperm.state_desc
from
sys.server_principals servprin
join sys.server_permissions servperm
on servprin.principal_id = servperm.grantee_principal_id
where
servprin.type in ('s', 'u', 'g')
--and servprin.name like '%chris%'
order by
'account name' asc
```

<p>If you want to change the database owner to something ( for example 'sa' ), you can run the following logic.</p>    


## SQL-Logic
```SQL
use master;
set nocount on
declare @change_database_owner      varchar(max)
set @change_database_owner      = ''
select  @change_database_owner      = @change_database_owner + 
'exec [' + name + ']..sp_changedbowner ''sa'';' + char(10)
from    sys.databases where name not in ('tempdb') order by name asc
exec    (@change_database_owner)
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

    
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

