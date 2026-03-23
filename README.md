# Algoritmo-de-Recomendacao-de-Musicas
## Descrição:
Este projeto implementa um algoritmo de recomendação de músicas baseado em grafos, utilizando conceitos de relacionamento entre usuários, artistas e gêneros.

A modelagem foi inspirada em sistemas reais de streaming, onde recomendações são geradas a partir de múltiplos sinais como:

comportamento do usuário (escuta e curtidas)
afinidade com artistas
similaridade de gêneros
conexões sociais (seguindo artistas)

![Figura1. Modelo](Modelo-do-Projeto-Música.png)
Figura1.Modelo

![Figura1. Modelo](Projeto-Música-Exemplo.png)
Figura2.Exemplo

## Tipos de nós:
Person (Usuário)
Person (Artista)
Musica
Gênero

## Relacionamentos:
(:Usuário)-[:ESCUTA]->(:Musica)

(:Usuário)-[:CURTE]->(:Musica)

(:Usuário)-[:SEGUE]->(:Artista)

(:Artista)-[:TOCAM]->(:Musica)

(:Musica)-[:DO_GENERO]->(:Genre)

## Exemplo de Query
```cypher
MATCH (u:Person {name: "Lucas", tipo: "Usuário"})

// Músicas já consumidas
OPTIONAL MATCH (u)-[:ESCUTA|CURTE]->(m1:Musica)

// Artistas dessas músicas
OPTIONAL MATCH (m1)-[:TOCAM]->(a1:Person {tipo: "Artista"})

// Gêneros dessas músicas
OPTIONAL MATCH (m1)-[:DO_GENERO]->(g:Genre)

// Artistas que o usuário segue
OPTIONAL MATCH (u)-[:SEGUE]->(a2:Person {tipo: "Artista"})

// Candidatas à recomendação
MATCH (rec:Musica)

// Ligações possíveis
OPTIONAL MATCH (rec)-[:TOCAM]->(aRec:Person {tipo: "Artista"})
OPTIONAL MATCH (rec)-[:DO_GENERO]->(gRec:Genre)

// Regras de pontuação
WITH u, rec,
     COUNT(DISTINCT CASE WHEN gRec = g THEN 1 END) AS genreScore,
     COUNT(DISTINCT CASE WHEN aRec = a1 THEN 1 END) AS artistScore,
     COUNT(DISTINCT CASE WHEN aRec = a2 THEN 1 END) AS followScore

// Remove músicas já ouvidas
WHERE NOT (u)-[:ESCUTA|CURTE]->(rec)

// Score final ponderado
WITH rec,
     (genreScore * 1) +
     (artistScore * 2) +
     (followScore * 3) AS score



RETURN rec.titulo AS musica, score
ORDER BY score DESC
LIMIT 5
```

## Ferramentas

Arrows.app para modelagem visual

Neo4j (conceitual)

Cypher para definição das estruturas

## Arquivos

📊 Modelo-do-Projeto-Música.png → diagrama do grafo

📊 Projeto-Música-Exemplo.png → diagrama do grafo

📄Grapho-de-Música-em-Cipher.txt → exemplo de criação dos nós e relacionamentos

📄 Query-Cipher-de-recomendação.txt → algoritmo para busca

#Setup

#Data Import

#Feedback
