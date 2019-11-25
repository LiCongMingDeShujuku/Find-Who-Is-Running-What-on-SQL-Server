![CLEVER DATA GIT REPO](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/0-clever-data-github.png "李聪明的数据库")

# 查询谁在SQL Server上运行什么程序
#### Find Who Is Running What on SQL Server
**发布-日期: 2014年5月7日 (评论)**

## Contents

- [中文](#中文)
- [English](#English)
- [SQL Logic](#Logic)
- [Build Info](#Build-Info)
- [Author](#Author)
- [License](#License) 


## 中文
这是一个快速的脚本，可以让你立即了解谁在运行什么程序。 它会返回一些可能对你有用的东西。 其中包括以下内容：
1.	会话ID
2.	用户名
3.	来自什么设备
4.	来自什么应用
5.	开始时间
6.	正在运行的命令
7.	CPU上的时间
8.	所用内存
9.	GB读取的数据量
10.	GB写入的数据量
11.	逻辑读取
12.	声明的状态
13.	完成百分比
14.	进程所花时间
15.	所剩小时
16.	所剩分钟



## English
Here’s a quick script to give you an immediate view of who is running what. It will return a number of things that you might find helpful. Those include the following:
1.	Session ID
2. User Name
3. From Device
4. From Application
5. Time Started
6. Statement that is running
7. Time on CPU
8. Memory Usage
9. Amount of Data that’s read by GB
10. Amount of Data that’s Written by GB
11. Logical Reads
12. Status of statement.
13. Percentage Done
14. Time Spent on process
15. Hours Remaining
16. Minutes Remaining


---
## Logic
```SQL
use master;
set nocount on
select
sdes.session_id
, 'user name' = sdes.login_name
, 'from device' = sdes.host_name
, 'from applcation' = sdes.program_name
, 'time started' = convert(char, start_time, 9) , 'statement' = command
, 'time on cpu' = sder.cpu_time
, 'database'    = sder.db_name(database_id)
, 'memory usage' = sdes.memory_usage
, 'gb data read' = (sder.reads * 8 / 1024 / 1024) -- if under 1gb (0) zero will be listed.
, 'gb data written' = (sder.writes * 8 / 1024 / 1024)-- if under 1gb (0) zero will be listed. 
, 'raw logical reads' = sder.logical_reads
, 'status' = sder.status
, 'percent done' = convert(numeric(6,2), sder.percent_complete) --may not be accurate for complex operatons.
, 'time spent' = convert(numeric(10,2), sder.total_elapsed_time /1000.0/60.0) --may not be accurate for complex operatons.
, 'hours remaining' = convert(numeric(10,2), sder.estimated_completion_time /1000.0/60.0/60.0) --may not be accurate for complex operatons.
, 'minutes remaining' = convert(numeric(10,2), sder.estimated_completion_time /1000.0/60.0) --may not be accurate for complex operatons. 
, sdest.text
from
sys.dm_exec_requests sder cross apply sys.dm_exec_sql_text(sder.sql_handle) sdest join sys.dm_exec_sessions sdes on sder.session_id = sdes.session_id where
sder.session_id > 50
order by
sder.logical_reads desc


```
以下是结果的截图。
Here’s a quick screenshot of the results you’ll see.

![#](images/find-who-is-running-what-with-sql-a.png?raw=true "#")


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

- **李聪明的数据库 Lee's Clever Data**
- **Mike的数据库宝典 Mikes Database Collection**
- **李聪明的数据库** "Lee Songming"

[![Gist](https://img.shields.io/badge/Gist-李聪明的数据库-<COLOR>.svg)](https://gist.github.com/congmingshuju)
[![Twitter](https://img.shields.io/badge/Twitter-mike的数据库宝典-<COLOR>.svg)](https://twitter.com/mikesdatawork?lang=en)
[![Wordpress](https://img.shields.io/badge/Wordpress-mike的数据库宝典-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

---
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Lee Songming](https://raw.githubusercontent.com/LiCongMingDeShujuku/git-resources/master/1-clever-data-github.png "李聪明的数据库")

