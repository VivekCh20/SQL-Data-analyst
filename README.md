# SQL-Data-analyst
Compute Total Points for Each Team in World Cup Group Stage
---------------------------------------------------------------------------------------------------------
select team_id,team_name,COALESCE(SUM(rankings.total_goals), 0) AS num_points from (
select t1.team_id,t1.team_name,m1.host_team,m1.host_goals,m1.guest_goals,
case when m1.host_goals>m1.guest_goals then 3
     when m1.host_goals=m1.guest_goals then 1 else 0
     end as total_goals
from teams t1 left join matches m1
on t1.team_id=m1.host_team
union all
select t2.team_id,t2.team_name,m2.guest_team,m2.host_goals,m2.guest_goals,
case when m2.guest_goals>m2.host_goals then 3
     when m2.host_goals=m2.guest_goals then 1 else 0
     end as total_goals
from teams t2 left join matches m2
on t2.team_id=m2.guest_team) as rankings 
group by team_id,team_name
order by num_points desc;
