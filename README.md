# Terceiro-Projeto-de-Dados-Programa-Desenvolve-do-Grupo-Boticario

Foi realizado uma análise sobre os  dados da base  de crimes em São Paulo do ano de 2019 e 2020 . O arquivo encontra-se  em anexo em formato CSV . Para uma análise mais detalhada sobre os crimes   ocorridos em São Paulo foi feito um upload da base de crimes no Big Query a fim de fazer tratamentos nas informações e gerar uma consulta . Posteriormente a consulta foi utilizada no Looker Studio , com o propósito de gerar uma visualização dos dados .

### Consulta feita no BigQuery 

ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve` 
DROP COLUMN   id_unico 

ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
ADD COLUMN id_unico STRING;
UPDATE `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
SET id_unico = GENERATE_UUID()
WHERE id_unico IS NULL;
CREATE TABLE `ProjetoDesenvolve3.Crimes_com_roubo` AS (

  SELECT 
  `Roubo de Carga` ,`Roubo de veiculos`, `Latrocinios`,`id_unico`,`Roubo a Banco ` ,
  CAST(REPLACE(CAST(`Furtos na região` AS STRING), '.', '') AS FLOAT64)AS Furtos_na_regiao,
  CAST(REPLACE(CAST(`Furto de veiculo` AS STRING), '.', '') AS FLOAT64)AS Furtos_de_veiculos,
  CAST(REPLACE(CAST(`Roubos` AS STRING), '.', '') AS FLOAT64)AS Roubos
  FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`

);
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Crimes_com_roubo`  RENAME COLUMN  `Roubo de Carga` TO Roubo_de_carga ;
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Crimes_com_roubo`  RENAME COLUMN  `Roubo de veiculos` TO Roubo_de_veiculos ;
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Crimes_com_roubo`  RENAME COLUMN `Roubo a Banco ` TO Roubo_banco ;
CREATE TABLE `ProjetoDesenvolve3.Informacao_geral` AS (

  SELECT Ano,`Delegacia`,`id_unico`
  FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
);

CREATE TABLE `ProjetoDesenvolve3.Homicidio` AS (
  SELECT  `Homicídio doloso por acidente de trânsito`,`Homicídio Culposo por acidente de Trânsito`,`Homicídio Culposo`,`Tentativa de Homicídio`,`id_unico`
FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
);

ALTER TABLE  `projeto-3-desenvolve.ProjetoDesenvolve3.Homicidio` RENAME COLUMN `Homicídio doloso por acidente de trânsito` TO Homicidio_doloso_acidente_transito; 
ALTER TABLE  `projeto-3-desenvolve.ProjetoDesenvolve3.Homicidio` RENAME COLUMN `Homicídio Culposo` TO Homicidio_culposo ;
ALTER TABLE  `projeto-3-desenvolve.ProjetoDesenvolve3.Homicidio` RENAME COLUMN `Homicídio Culposo por acidente de Trânsito` TO Homicidio_culposo_acidente_transito ; 
ALTER TABLE  `projeto-3-desenvolve.ProjetoDesenvolve3.Homicidio` RENAME COLUMN `Tentativa de Homicídio` TO Tentativa_homicidio;

CREATE TABLE `ProjetoDesenvolve3.Lesao` AS(
  SELECT `Lesão Corporal seguida de morte`,`Lesão Corporal Dolosa`,`Lesão Corporal Culposa por acidente de trânsito`,`Lesão Corporal Culposa` ,`id_unico`
  FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
);

ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Lesao`  RENAME COLUMN `Lesão Corporal seguida de morte` TO Lesao_corporal_seguida_morte ;
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Lesao`  RENAME COLUMN  `Lesão Corporal Dolosa` TO Lesao_corporal_dolosa ;
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Lesao`  RENAME COLUMN  `Lesão Corporal Culposa por acidente de trânsito` TO Lesao_corporal_culposa_acidente_transito ;
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Lesao`  RENAME COLUMN  `Lesão Corporal Culposa` TO Lesao_corporal_culposa;


CREATE TABLE `ProjetoDesenvolve3.Estrupo` AS(
  SELECT `Estupro`,`Estupro de vulnerável`,`id_unico`
  FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Projeto 3 -Desenvolve`
);
ALTER TABLE `projeto-3-desenvolve.ProjetoDesenvolve3.Estrupo` RENAME COLUMN `Estupro de vulnerável` TO Estrupo_vulneravel ;


CREATE TABLE  `ProjetoDesenvolve3.Crimes_SP` AS(
SELECT
c.id_unico,
c.Roubos,
c.Furtos_na_regiao,
c.Roubo_banco,
i.Delegacia,
c.Latrocinios,
c.Roubo_de_carga,
c.Roubo_de_veiculos,
c.Furtos_de_veiculos,
e.Estrupo_vulneravel,
e.Estupro,
h.Homicidio_culposo,
h.Homicidio_culposo_acidente_transito,
h.Homicidio_doloso_acidente_transito,
h.Tentativa_homicidio,
i.Ano,
l.Lesao_corporal_culposa,
l.Lesao_corporal_culposa_acidente_transito,
l.Lesao_corporal_dolosa,
l.Lesao_corporal_seguida_morte,
FROM `projeto-3-desenvolve.ProjetoDesenvolve3.Crimes_com_roubo` AS c
 LEFT JOIN  `projeto-3-desenvolve.ProjetoDesenvolve3.Estrupo` AS e ON c.id_unico = e.id_unico
LEFT JOIN `projeto-3-desenvolve.ProjetoDesenvolve3.Homicidio` AS h ON c.id_unico = h.id_unico 
LEFT JOIN `projeto-3-desenvolve.ProjetoDesenvolve3.Informacao_geral` AS i ON c.id_unico = i.id_unico
LEFT JOIN `projeto-3-desenvolve.ProjetoDesenvolve3.Lesao` AS l ON c.id_unico = l.id_unico

);

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

