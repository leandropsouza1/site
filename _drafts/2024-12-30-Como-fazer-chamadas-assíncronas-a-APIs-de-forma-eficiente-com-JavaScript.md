---
layout: post
author: Leandro Pessoa
tag: ConteudoTecnico Programação JavaScript
excerpt: Neste post, vamos mostrar como podemos fazer a chamada a uma API várias vezes de forma assincrona
tag: ConteudoTecnico Programacao JavaScript
---

## Como fazer chamadas assíncronas a APIs de forma eficiente com JavaScript?

**Olá**, desenvolvedores!

Estou precisando da ajuda de vocês para encontrar a melhor forma de realizar múltiplas chamadas a uma API de maneira eficiente e performática utilizando JavaScript.

## O problema

Preciso lidar com duas etapas principais:

- Fazer uma chamada para uma API que retorna uma lista de raças de gatos.
- Com essa lista em mãos, chamar outra API utilizando o ID de cada raça para obter os detalhes específicos (como origem, nível de energia, se são amigáveis, etc.).

## A dúvida

Para isso, criei o código abaixo, que realiza as chamadas de forma assíncrona, iterando sobre os IDs (vindos no primeiro passo).

Quero saber: **existe uma abordagem mais performática ou uma técnica mais eficiente para resolver esse problema usando JavaScript?**

**Importante:** Fiz um código simples e adicionei alguns comentários para facilitar o entendimento. O foco da minha dúvida está no método `getCatDetails(cats)`.

```js
// Função para buscar uma lista de raças de gatos
async function getCats() {
  const URL =
    "https://api.thecatapi.com/v1/images/search?has_breeds=1&order=rand&limit=10";

  const headers = new Headers({
    "Content-Type": "application/json",
  });

  const requestOptions = {
    method: "GET",
    headers: headers,
    redirect: "follow",
  };

  try {
    const response = await fetch(URL, requestOptions);
    return await response.json();
  } catch (err) {
    console.error(`Aconteceu um erro não esperado. Detalhes: ${err}`);
  }
}

// Função para buscar os detalhes de cada raça de gato
async function getCatDetails(cats) {
  const headers = new Headers({
    "Content-Type": "application/json",
  });

  const requestOptions = {
    method: "GET",
    headers: headers,
    redirect: "follow",
  };

  // Monta a lista de URLs para obter os detalhes de cada gato
  const urls = cats.map(
    (cat) => `https://api.thecatapi.com/v1/images/${cat.id}`
  );

  // Faz as chamadas assíncronas usando Promise.all
  try {
    const catDetails = await Promise.all(
      urls.map(async (url) => {
        const response = await fetch(url, requestOptions);
        return await response.json();
      })
    );
    return catDetails;
  } catch (err) {
    console.error(`Aconteceu um erro não esperado. Detalhes: ${err}`);
  }
}

// Função principal para executar o processo
(async () => {
  // Obtém uma lista aleatória de 10 raças de gatos
  const cats = await getCats();

  if (cats.length === 0) {
    console.error("Não foi possível obter a lista de gatos.");
    return;
  }

  // Busca os detalhes de cada raça de gato
  const catDetails = await getCatDetails(cats);

  console.log(catDetails);
})();
```
