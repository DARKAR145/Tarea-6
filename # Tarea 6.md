# Tarea 6

## Hallazgos encontrados

Obtener la moda en sql resulte ser un desafio ya que existen diferentes maneras, me gustaria discutir en clase acerca de un metodo mas 
eficiente pero en el cual se requiere SQL Server, por el momento lo solucione con un metodo simple pero efectivo. En cuanto a los
cuantiles, yo me fui mas por obtener los percentiles de la tabla. La informacion obtenida se puede observar en estos links:

https://www.mysqltutorial.org/mysql-window-functions/mysql-percent_rank-function/
https://www.mytecbits.com/microsoft/sql-server/how-to-calculate-statistical-mode
https://estradawebgroup.com/Post/-Como-obtener-el-promedio-en-SQL-Server-con-la-funcion-AVG---/20369
https://stackoverflow.com/questions/19770026/calculate-percentile-value-using-mysql



#Codigo escrito para SQL

use torneo_videojuegof;

select * from scoresf;

select ID_Juegos,
avg(total_kills) as Promedio_Kills, 
avg(Total_Deaths) as Promedio_Deaths,
avg(Assists) as Promedio_Assists,
avg(NonTraded_Kills) as Promedio_NonTraded,
avg(Highest_Streak) as Promedio_Streak,
avg(Daño) as Promedio_Daño,
avg(Primera_Sangre) as Promedio_PrimeraSangre 
from scoresf group by ID_Juegos order by ID_Juegos;

select ID_Partido,
avg(total_kills) as Promedio_Kills, 
avg(Total_Deaths) as Promedio_Deaths,
avg(Assists) as Promedio_Assists,
avg(NonTraded_Kills) as Promedio_NonTraded,
avg(Highest_Streak) as Promedio_Streak,
avg(Daño) as Promedio_Daño,
avg(Primera_Sangre) as Promedio_PrimeraSangre 
from scoresf group by ID_Partido order by ID_Partido;

select ID_Juegos,
max(total_kills) as Maximo_Kills, 
max(Total_Deaths) as Maximo_Deaths,
max(Assists) as Maximo_Assists,
max(NonTraded_Kills) as Maximo_NonTraded,
max(Highest_Streak) as Maximo_Streak,
max(Daño) as Maximo_Daño,
max(Primera_Sangre) as Maximo_PrimeraSangre 
from scoresf group by ID_Juegos order by ID_Juegos;

select ID_Partido,
max(total_kills) as Maximo_Kills, 
max(Total_Deaths) as Maximo_Deaths,
max(Assists) as Maximo_Assists,
max(NonTraded_Kills) as Maximo_NonTraded,
max(Highest_Streak) as Maximo_Streak,
max(Daño) as Maximo_Daño,
max(Primera_Sangre) as Maximo_PrimeraSangre 
from scoresf group by ID_Partido order by ID_Partido;

select Total_Kills, count(Total_Kills) as Moda from scoresf group by Total_Kills order by Moda desc limit 1;
select Total_kills, ID_Jugador, round(percent_rank()over( order by Id_Jugador),2)percentile_rank from scoresf;

```En proceso```
select Total_Kills, count(*) as Cuenta from scoresf group by Total_Kills;
select Total_kills, Cuenta, dense_rank() over(order by cuenta DESC) as rnk from (select Total_Kills, count(*) as Cuenta from scoresf group by Total_Kills);

select ID_Juegos, Total_Kills, count(Total_kills) as Frecuencia from scoresf group by ID_Juegos, Total_Kills order by ID_Juegos, 3 desc;

select
	ID_Juegos,
    Total_Kills,
    count(Total_kills) as Frecuencia,
    RANK() over(Partition by ID_Juegos order by count(Total_Kills) desc) as Rango
    from scoresf
    group by ID_Juegos, Total_Kills order by ID_Juegos; 
    
    

with CTE_Stats as (select
	ID_Juegos,
    Total_Kills,
    count(Total_kills) as Frecuencia,
    RANK() over(Partition by ID_Juegos order by count(Total_Kills) desc) as Rango
    from scoresf
    group by ID_Juegos, Total_Kills)
    
select ID_Juegos, total_Kills, Frecuencia, Rango, from CTE_Stats where Rango=1    


select total_kills from (select Total_Kills, count(Total_Kills) as Cuenta_Kills from scoresf group by Total_Kills order by Total_Kills) where Cuenta_kills=max(Cuenta_kills);
