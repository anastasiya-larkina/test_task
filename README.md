# DINS Test Task

## Analysis

### Perform analysis of MySQL DB extract

### Document DB structure (UML, English language)
Tee diagram is in repository test_task.
### Use SQL for data aggregation and reporting

## Reports

## Total expenses

The result of that SQL query represents total expenses. The other users have not expenses. The data grouped by UID.  
### Code

```sql
select
    call_logs.`UID`,
    count(call_logs.`UID`),
    count(call_logs.`UID`)*(select `Money` from rates where `ID`=3)
from
    call_logs left join numbers on 
    call_logs.`To` = numbers.`Phone_Number`
where
    call_logs.`Call_dir` = 'out' and numbers.`Phone_Number` is null
group by
    call_logs.`UID`
order by
    call_logs.`UID`;
```

### Result

|UID|count(call_logs.`UID`)|count(call_logs.`UID`)*(select `Money` from rates where `ID`=3)|
|---|----------------------|---------------------------------------------------------------|
|4763|1|0.04|
|6677|1|0.04|
|9757|1|0.04|
|10163|1|0.04|
|12885|1|0.04|
|16963|2|0.08|
|28866|1|0.04|
|33365|1|0.04|
|34872|1|0.04|
|38210|1|0.04|
|39388|1|0.04|
|46376|1|0.04|
|50170|1|0.04|
|50480|1|0.04|
|54820|1|0.04|
|55198|1|0.04|
|56919|1|0.04|
|60889|1|0.04|
|63638|1|0.04|
|70829|1|0.04|
|77367|1|0.04|
|83718|2|0.08|
|84883|1|0.04|
|89257|1|0.04|
|92876|1|0.04|
|93250|1|0.04|
|94076|1|0.04|
|96083|1|0.04|
|96310|1|0.04|
|98148|1|0.04|
|98821|1|0.04|

---

## Top 10: Most active users
The result of that SQL query demonstrates top-10 users by call duration. I used "outer join" to select rows from joined tables and "timediff" to define talk time. 
### Code

```sql
select
    accounts.`Name`,
    call_logs.`UID`,
    timediff(call_logs.`Timestamp_end`,
    call_logs.`Timestamp_start`) as diff
from
    call_logs
right join accounts on
    call_logs.`UID` = accounts.`UID`
group by
    call_logs.`UID`
order by
    diff desc
limit 10;
```

### Result

|Name|UID|diff|
|----|---|----|
|Roman|21110|00:03:40|
|Bob|48841|00:03:31|
|Pavel|71788|00:03:31|
|Tom|14414|00:03:30|
|Alex|22719|00:03:29|
|Tom|99499|00:03:21|
|Elena|30415|00:03:19|
|Pavel|34913|00:03:16|
|Vlad|45535|00:03:12|
|Denis|27065|00:03:11|

---

## Top 10: Users with highest charges, and daily distribution for each of them

### Code

```sql
select
    call_logs.`UID`,
    count(call_logs.`UID`),
    count(call_logs.`UID`)*(
    select
        `Money`
    from
        rates
    where
        `ID` = 3) as charge
from
    call_logs
left join numbers on
    call_logs.`To` = numbers.`Phone_Number`
where
    call_logs.`Call_dir` = 'out'
    and numbers.`Phone_Number` is null
group by
    day(call_logs.`Timestamp_start`),
    call_logs.`UID`
order by
    charge desc
limit 10;
```

### Result

|UID|count(call_logs.`UID`)|charge|
|---|----------------------|------|
|16963|2|0.08|
|83718|2|0.08|
|93250|1|0.04|
|96310|1|0.04|
|98821|1|0.04|
|89257|1|0.04|
|50480|1|0.04|
|63638|1|0.04|
|55198|1|0.04|
|94076|1|0.04|
