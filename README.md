# Data Science Independent Project #3 – Education & Census Data

## Descrição
O seu conselho é necessário para uma equipa de decisores políticos que procuram tomar decisões mais informadas sobre a educação. Ajude-os a investigar como fatores externos podem influenciar o desempenho em exames estaduais de avaliação de alunos do ensino médio público.

Utilizei o SQLite e o DB Browser for SQLite para manupilação e consulta do banco de dados.

E agora irei responder as perguntas:

<br>

**- Quantas escolas públicas de ensino médio existem em cada CEP? em cada estado?**

```sql
select cd.zip_code, cd.state_code, count(*) as Total
from census_data cd 
left join public_hs_data hs
on hs.zip_code = cd.zip_code
group by cd.zip_code
order by total desc
limit 5;
```

R: Segue os 5 CEPS com mais escolas publicas. 

<br>
<br>

**- Qual é a renda média mínima, máxima e média da nação? para cada estado?**

Para a nação:

```sql
SELECT min(median_household_income) as min, max(median_household_income) as max, round(avg(median_household_income), 2) as avg
from census_data
where median_household_income != 'NULL';

```

Para cada estado:

```sql
SELECT state_code, min(median_household_income) as min, max(median_household_income) as max, round(avg(median_household_income), 2) as avg
from census_data
where median_household_income != 'NULL'
group by state_code;
```

<br>
<br>

**- As características da área do CEP, como a renda familiar média, influenciam o desempenho dos alunos no ensino médio?**

Segue as informações baseadas na coluna pct_proficient_math.

```sql
SELECT income_range, round(AVG(pct_proficient_math), 2) AS avg_exam_score
FROM (
  SELECT cd.zip_code, cd.median_household_income,
    CASE
      WHEN median_household_income < 50000 THEN '<$50k'
      WHEN median_household_income BETWEEN 50000 AND 100000 THEN '$50k-$100k'
      WHEN median_household_income > 100000 THEN '>$100k'
      ELSE 'Unknown'
    END AS income_range,
    hs.pct_proficient_math
  FROM census_data cd
  JOIN public_hs_data hs
  ON cd.zip_code = hs.zip_code
) t
GROUP BY income_range;
```


## Considerações finais
Houve uma pergunta que não consegui responder. Isso me mostrou que eu preciso estudar mais e praticar onde eu estou com mais dificuldades.

Segue o link do projeto (em inglês) e do curso que estou fazendo, caso queiram dar uma olhada:

https://discuss.codecademy.com/t/data-science-independent-project-3-education-census-data/419947

https://www.codecademy.com/career-journey/data-scientist-aly