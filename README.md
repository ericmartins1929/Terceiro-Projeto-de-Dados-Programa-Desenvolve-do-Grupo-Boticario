# Terceiro-Projeto-de-Dados-Programa-Desenvolve-do-Grupo-Boticario

Foi realizado uma análise sobre os  dados da base  de crimes em São Paulo do ano de 2019 e 2020 . O arquivo encontra-se  em anexo em formato CSV . Para uma análise mais detalhada sobre os crimes   ocorridos em São Paulo foi feito um upload da base de crimes no Big Query a fim de fazer tratamentos nas informações e gerar uma consulta . Posteriormente a consulta foi utilizada no Looker Studio , com o propósito de gerar uma visualização dos dados .

### Consulta feita no BigQuery 

A consulta feita no Bigquery encontra-se em anexo . 

No BigQuery eu subi cada uma das tabelas  que foram criadas . Priorizei o uso da tabela Crimes_SP, além disso subi uma consulta personalizada a fim de filtrar os Furtos na região com valores maiores que 1500 . Segue a consulta personalizada  que foi utilizada na análise :

SELECT *,
  SPLIT(Delegacia, ' - ')[SAFE_OFFSET(0)] AS Codigo_dp,
  SPLIT(Delegacia, ' - ')[SAFE_OFFSET(1)] AS Nome_dp,
  Estupro + Estrupo_vulneravel AS Total_Estupros
FROM
  `projeto-3-desenvolve.ProjetoDesenvolve3.Crimes_SP`
WHERE
   Furtos_na_regiao > 1500;

## Looker Studio

Para fazer análise do relatório no Looker  procurei verificar quais eram as delgacias que tinha maior incidência de crimes registrados , como : Estupro , Roubos , Lesões corporais , Latrocínios,Furtos. Por meio do Looker juntei e fiz agrupamento de funções como:

Total de Roubos = sum(Roubos)+sum(Roubo_banco) +sum(Roubo_de_carga)+sum(Roubo_de_veiculos)
Total_furtos = sum(Furtos_na_regiao)+sum(Furtos_de_veiculos)
Total_lesao_corporal =sum(Lesao_corporal_culposa)+sum(Lesao_corporal_culposa_acidente_transito)+sum(Lesao_corporal_seguida_morte)+sum(Lesao_corporal_dolosa)

E por fim criei uma filtragem a fim de classificar o nível de perigo registrados por Delegacia , aonde :

Nível de perigo =  CASE
 WHEN Furtos_na_regiao>=3000  THEN "Regiao muito perigosa"
 ELSE 'Regiao perigosa'
END

Tinha notado que na base de crimes de SP a medida que a quantidade de furtos é maior de 3000 , consequentemente a quantidade de crimes é maior em outras modalidades também , como : Estupros , Roubos , Lesões corporais .

A visualização dos dados gerados no Looker Studio , encontra-se no link a seguir : https://lookerstudio.google.com/reporting/acc6b271-3107-4a53-b7ba-090dfc5adb84

Também encaminho o pdf em anexo do dashboard .

