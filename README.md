# Projeto-dio
Neo4j
# Sistema de Recomendação de Filmes e Séries com Neo4j 🎬

Este repositório contém a modelagem de um banco de dados orientado a grafos para um sistema de recomendações, explorando as conexões entre usuários, conteúdos (filmes e séries), gêneros, atores e diretores.

## 📌 Estrutura do Grafo (Modelagem)

A modelagem foi pensada para permitir consultas rápidas de filtragem colaborativa e recomendações personalizadas:

- **Nós:** `User`, `Movie`, `Series`, `Actor`, `Director`, `Genre`.
- **Relacionamentos:**
  - `(User)-[:WATCHED {rating}]->(Movie|Series)`
  - `(Actor)-[:ACTED_IN]->(Movie|Series)`
  - `(Director)-[:DIRECTED]->(Movie|Series)`
  - `(Movie|Series)-[:IN_GENRE]->(Genre)`

## 🚀 Exemplos de Consultas Cypher

### 1. Recomendação por Gênero
Encontra novos conteúdos baseados nos gêneros que o usuário mais gosta (notas >= 4):
```cypher
MATCH (u:User {name: "NomeUsuario"})-[r:WATCHED]->()-[:IN_GENRE]->(g:Genre)
WHERE r.rating >= 4
MATCH (rec)-[:IN_GENRE]->(g)
WHERE NOT (u)-[:WATCHED]->(rec)
RETURN DISTINCT rec.title AS Recomendacao, g.name AS Genero
LIMIT 10;
