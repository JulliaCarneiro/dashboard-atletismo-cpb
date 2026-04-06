# Dicionário de Dados: Performance nos Circuitos Paralímpicos

##  Descrição do Projeto
Este projeto consistiu na extração, tratamento e modelagem de dados de eventos de atletismo paralímpico. O objetivo principal foi transformar dados brutos de planilhas Excel em um dashboard estratégico para análise de participação de atletas, desempenho por prova e abrangência geográfica.

---

## Etapas de ETL e Tratamento de Dados
Os dados originais apresentavam inconsistências de formato e estrutura. Utilizei o **Power Query** para realizar as seguintes transformações:

* **Padronização de Resultados:** Separação de métricas de tempo (corridas) e métricas numéricas (salto em distância e arremesso de peso) em colunas específicas (`resultado_tempo` e `resultado_numero`).
* **Consolidação (Append):** Junção de múltiplas abas anuais em uma única tabela fato (`F_eventos`).
* **Enriquecimento de Dados:** Criação de colunas calculadas para identificar a **idade do atleta na data da prova** e a contagem de **municípios participantes**.

---

##  Modelagem de Dados (Star Schema)
A estrutura foi desenhada seguindo as melhores práticas de Business Intelligence para garantir performance e escalabilidade, utilizando o modelo **Star Schema**.

> ![image_alt](https://github.com/JulliaCarneiro/dashboard-atletismo-cpb/blob/main/repositorio-projeto/images/star_schema.png?raw=true)

* **Fato (`F_eventos`):** Registros de performance e detalhes das provas.
* **Dimensão (`D_atletas`):** Dados cadastrais, gênero e tipo de deficiência.
* **Dimensão (`D_calendario`):** Tabela criada para permitir análises temporais precisas (Time Intelligence).
* **Tabela de Medidas (`_medidas`):** Centralização de todas as lógicas de negócio em DAX.

---

## Dicionário de Dados

### Tabela Fato: `F_eventos`
| Coluna | Tipo | Descrição | Regra de Negócio |
| :--- | :--- | :--- | :--- |
| `evento` | String | Nome do circuito/competição. | Ex: "Circuito Itu". |
| `prova` | String | Modalidade disputada. | Ex: "100m", "Salto em distância". |
| `categoria` | String | Categoria do atleta. | Ex: "Adulto", "Sub14". |
| `resultado_tempo` | Time | Tempo final para provas de pista. | Formato HH:MM:SS. |
| `resultado_numero`| Float | Marca atingida em metros. | Para saltos e arremessos. |
| `data_prova` | Date | Data da realização do evento. | Relacionamento com `D_calendario`. |
| `Idade_na_prova` | Int | Idade do atleta no dia do evento. | Calculada: `data_prova` - `data_nascimento`. |
| `municipios` | String | Cidade sede do circuito. | Identificação geográfica do evento. |

### Tabela Dimensão: `D_atletas`
| Coluna | Tipo | Descrição |
| :--- | :--- | :--- |
| `id_atleta` | Int | Identificador único (Chave Primária). |
| `nome` | String | Nome completo do atleta. |
| `genero` | String | Gênero (Masculino/Feminino). |
| `tipo_deficiencia`| String | Classificação funcional da deficiência. |

---

## Medida DAX
Foram desenvolvidas medidas para extrair insights automáticos e facilitar a tomada de decisão:

* **Ranking Dinâmico:** Classificação de atletas dentro de cada prova e categoria utilizando a função `RANKX`.
* **Classificação de Pódio:** Lógica condicional para destacar automaticamente os medalhistas.
* **Métricas de Participação:** Contagem distinta de atletas e análise de diversidade demográfica.




