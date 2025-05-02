<p align="left"><img src="http://img.shields.io/static/v1?label=STATUS&message=CONCLUIDO&color=GREEN&style=for-the-badge"/></p>

# 🕵️‍♂️ Who don´t follow you back in Github


- ## **📌 Resumo do projeto** 

Who don´t follow you back in Github é um codigo que permite ver quem não segue você de volta no Github. Não requer downloads ou instalações.


- ## **🎯 Objetivo**

Criação de uma lista com usuários que não te segue nessa rede.


- ## **👥 Equipe**

Projeto de Thaise Oliveira. Com base no repositório do [herbertcarnaubadesouza](https://github.com/herbertcarnaubadesouza/who-dont-follows-u) de descobrir não seguidores no Instagram 


- ## **👩‍💻Como usar:** 
1. Copie o codigo abaixo
```java
"use strict";

const GITHUB_API_URL = "https://api.github.com/users/";

let nonFollowersList = [];
let isActiveProcess = false;

function sleep(ms) {
    return new Promise((resolve) => setTimeout(resolve, ms));
}

// Fetch followers with pagination
async function getAllFollowers(username) {
    let followers = [];
    let page = 1;

    while (true) {
        const response = await fetch(`${GITHUB_API_URL}${username}/followers?per_page=100&page=${page}`);
        if (!response.ok) {
            throw new Error('Error fetching followers: ' + response.statusText);
        }
        const data = await response.json();
        if (data.length === 0) break; // Se nenhum dado, sai do loop
        followers = followers.concat(data);
        page++;
    }

    return followers;
}

// Fetch following with pagination
async function getAllFollowing(username) {
    let following = [];
    let page = 1;

    while (true) {
        const response = await fetch(`${GITHUB_API_URL}${username}/following?per_page=100&page=${page}`);
        if (!response.ok) {
            throw new Error('Error fetching following: ' + response.statusText);
        }
        const data = await response.json();
        if (data.length === 0) break; // Se nenhum dado, sai do loop
        following = following.concat(data);
        page++;
    }

    return following;
}

async function findNonFollowers(username) {
    if (isActiveProcess) return;
    
    isActiveProcess = true;
    showLoading(true);
    document.querySelector('.error-message').innerText = '';

    try {
        const followers = await getAllFollowers(username);
        const following = await getAllFollowing(username);

        const followerSet = new Set(followers.map(user => user.login));
        nonFollowersList = following.map(user => user.login).filter(user => !followerSet.has(user));

        renderResults(nonFollowersList);
    } catch (error) {
        console.error(error);
        showError(error.message);
    } finally {
        isActiveProcess = false;
        showLoading(false);
    }
}

function renderResults(nonFollowers) {
    const resultsContainer = document.querySelector(".results-container");
    resultsContainer.innerHTML = ""; // Limpa resultados anteriores

    if (nonFollowers.length === 0) {
        resultsContainer.innerHTML = "<div>Ninguém não segue você de volta.</div>";
    } else {
        nonFollowers.forEach(user => {
            resultsContainer.innerHTML += `<div>${user}</div>`;
        });
    }

    document.querySelector(".nonfollower-count").textContent = `Não seguidores: ${nonFollowers.length}`;
}

function showLoading(isLoading) {
    const loader = document.querySelector(".loader");
    loader.style.display = isLoading ? "block" : "none";
}

function showError(message) {
    document.querySelector('.error-message').innerText = message;
}

function renderOverlay(username) {
    document.body.innerHTML = `
        <main>
            <div>
                <header>
                    <h1>GitHub Non-Followers</h1>
                    <button id="check-followers-btn">Verificar Não Seguidores</button>
                </header>
                <div class="loader" style="display:none;">Carregando...</div>
                <div class="error-message" style="color:red"></div>
                <div class="results-container"></div>
                <footer>
                    <div class="nonfollower-count">Não seguidores: 0</div>
                </footer>
            </div>
        </main>
    `;

    document.getElementById('check-followers-btn').addEventListener('click', () => {
        findNonFollowers(username);
    });
}

function init() {
    const username = prompt("Digite seu nome de usuário do GitHub:");
    if (username) {
        renderOverlay(username);
    } else {
        alert("O nome de usuário é obrigatório!");
    }
}

init();

```
2. Faça login em sua conta e abra o console do desenvolvedor ou (Ctrl+Shift+J(Windows) || ⌘+⌥+I (Mac os)) e cole o código. Ele pedirá seu nome de usuário:
  
    ![login](img-read-me/Capturar.PNG)
  
3. Após colocar seu nome de usuário você encontrará a seguinte interface:
  
   ![login](img-read-me/Capturar2.PNG)
  
4. Clique no botão 'Verificar não Seguidores', os usuários que não seguem de volta serão listados. 
ps: Se desejar deixar de seguir algum usuário será necessário copiar o username, inserir no final da URL https://github.com/ e clicar em unfollow.

## **🙋‍♀️ Responsabilidade de uso é inteiramente do usuário**: Esse código não é afiliado, associado, autorizado, endossado ou de qualquer forma oficialmente conectado ao Github.
  
