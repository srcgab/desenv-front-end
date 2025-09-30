# Projeto 01: Lista de Tarefas (To-Do List)

Projeto -> uma aplicação de lista de tarefas. O HTML e CSS já estão prontos, usaremos JavaScript para implementar funcionalidades de adicionar, remover e marcar tarefas como concluídas.

> ⚠️ O objetivo é aprender. Evite copiar e colar! **Digite cada trecho de código**, tente entender o que ele faz e veja o resultado no seu navegador.

## 🏁 Resultado Final

Ao final da implementação com JavaScript, seu projeto deverá se parecer com isto:

![Resultado final da lista de tarefas](https://i.imgur.com/y3A6p6W.gif)

## Funcionalidades a Implementar com JS

1.  **Adicionar Tarefa:** O usuário poderá digitar uma tarefa no campo de input e, ao clicar no botão `+` ou apertar "Enter", a nova tarefa deve aparecer na lista.
2.  **Marcar como Concluída:** Ao clicar no checkbox de uma tarefa, ela deve ser movida para a lista de "Tarefas Completas". Se o checkbox for desmarcado, ela deve voltar para a lista principal.
3.  **Remover Tarefa:** Cada tarefa terá um botão `x` que, ao ser clicado, a removerá permanentemente da lista.
4.  **Limpar Tarefas Concluídas:** Um botão permitirá remover todas as tarefas da lista de concluídas de uma só vez.
5.  **Feedback Visual:** Uma imagem deve ser exibida quando não houver nenhuma tarefa na lista. A seção de "Tarefas Completas" só deve aparecer quando houver pelo menos uma tarefa concluída.

## Lembrete: Como Adicionar JS ao HTML

Para que seu código JavaScript possa interagir com a página, é preciso conectá-lo ao `index.html`. No seu arquivo HTML, a seguinte linha deve existir no final do `<body>`:

```html
    <script src="script.js"></script>
</body>
</html>
```

---

## Conceitos essênciais

Antes de codificar, vamos revisar os principais conceitos de JS que usaremos:

* **Manipulação do DOM (`document`):** O DOM é a representação do seu HTML que o JavaScript consegue entender e manipular.
    * `document.getElementById('id-do-elemento')`: Usado para selecionar um único elemento da página pelo seu `id`. É a forma mais rápida de seleção.
    * `document.createElement('tag')`: Usado para criar um novo elemento HTML (como `<li>`, `<input>`, `<button>`) via JavaScript.
    * `elemento.appendChild(filho)`: Usado para adicionar um elemento `filho` dentro de um `elemento` pai.
    * `elemento.remove()`: Remove o elemento da página.
    * `elemento.classList.add('nome-da-classe')`: Adiciona uma classe CSS a um elemento.
    * `elemento.textContent = 'novo texto'`: Altera o conteúdo de texto de um elemento.
    * `elemento.value`: Pega ou define o valor de um campo de input.

* **Eventos (`addEventListener`):** É como dizemos ao JavaScript para "ouvir" por interações do usuário (cliques, digitação, etc.) e executar uma função quando elas acontecerem.
    * `elemento.addEventListener('tipoDoEvento', nomeDaFuncao)`: Associa uma função a ser executada quando um evento (ex: `'click'`, `'change'`) ocorre em um elemento.

* **Funções:** Blocos de código reutilizáveis que executam uma tarefa específica.

* **Arrow Functions (`() => {}`):** Uma forma mais concisa de escrever funções em JavaScript. Usaremos bastante com os `EventListeners`.

## Passo a Passo do Código JS

Crie arquivo `script.js` para começar.

### Passo 1: Selecionar os Elementos da Página

Primeiro, precisamos "capturar" os elementos do HTML com os quais vamos interagir e guardá-los em variáveis. Isso torna nosso código mais legível e eficiente.

```javascript
// script.js

// Seleção dos elementos do DOM
const addTaskBtn = document.getElementById("addTaskBtn");
const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");
const noTasksImage = document.getElementById("noTasksImage");
const completedTasks = document.getElementById("completedTasks");
const completedTasksList = document.getElementById("completedTasksList");
const clearCompletedBtn = document.getElementById("clearCompletedBtn");

// Variável para garantir IDs únicos para as tarefas
let nextTaskId = 0;
```

> **Teste e Debug:** Abra o `index.html` no seu navegador e abra o Console do Desenvolvedor (F12). Digite `console.log(taskInput);` e, se o elemento `<input id="taskInput" ...>` aparecer no console, significa que sua seleção funcionou.

### Passo 2: Criar a Função que Adiciona uma Nova Tarefa

Agora, vamos criar a lógica principal. Precisamos de uma função que:
1.  Pegue o texto do input.
2.  Verifique se o texto não está vazio.
3.  Crie um novo item de lista (`<li>`) com um checkbox, o texto e um botão de deletar.
4.  Adicione esse novo item à lista de tarefas na tela.

Para manter o código organizado, vamos criar uma função auxiliar `createTaskElement` que será responsável apenas por criar o elemento da tarefa.

```javascript
// ... (código do Passo 1) ...

// Função para criar o elemento HTML de uma tarefa
function createTaskElement(taskText) {
    const taskId = nextTaskId++;
    const taskItem = document.createElement("li");
    taskItem.classList.add("listItem");

    // Cria o checkbox
    const checkbox = document.createElement("input");
    checkbox.type = "checkbox";
    checkbox.id = `task-${taskId}`;

    // Cria a label (texto da tarefa)
    const label = document.createElement("label");
    label.htmlFor = `task-${taskId}`;
    label.textContent = taskText;

    // Cria o botão de deletar
    const deleteBtn = document.createElement("button");
    deleteBtn.classList.add("deleteBtn");
    deleteBtn.textContent = "x";

    // Cria a informação de data
    const dateInfo = document.createElement("p");
    dateInfo.classList.add("dateInfo");
    dateInfo.textContent = `Criado em: ${new Date().toLocaleDateString("pt-BR")}`;

    // Adiciona os elementos criados ao item da lista
    taskItem.appendChild(checkbox);
    taskItem.appendChild(label);
    taskItem.appendChild(deleteBtn);
    taskItem.appendChild(dateInfo);

    return taskItem;
}

/* Função para adicionar uma nova tarefa à lista.
.trim() remove espaços em branco do início e fim 
taskInput.value = "" limpa o campo de input
taskInput.focus() coloca o foco de volta no input
*/
function addTask() {
    const taskText = taskInput.value.trim(); 
    if (taskText !== "") {
        const newTask = createTaskElement(taskText);
        taskList.appendChild(newTask);
        taskInput.value = ""
        taskInput.focus();
    }
}

// Adiciona o "ouvinte de evento" no botão de adicionar
addTaskBtn.addEventListener("click", addTask);
```

> **Teste e Debug:** Salve o `script.js` e atualize seu navegador. Tente digitar algo no input e clicar no botão `+`. A nova tarefa deve aparecer na lista.

### Passo 3: Centralizar a Lógica de Atualização da Interface

Ao adicionar uma tarefa, a imagem "Sem Tarefas" deveria sumir. E quando movermos tarefas para a lista de concluídas, a seção correspondente deve aparecer/sumir. Para não repetir código, vamos criar uma função `updateUI()` que cuida de toda essa lógica visual.

Adicione esta função no início do seu script, logo após as variáveis.

```javascript
// ... (código das variáveis) ...

// Função centralizada para atualizar a interface
function updateUI() {
    const hasTasks = taskList.children.length > 0;
    const hasCompletedTasks = completedTasksList.children.length > 0;

    // Mostra ou esconde a imagem de "sem tarefas"
    noTasksImage.style.display = (hasTasks || hasCompletedTasks) ? "none" : "block";

    // Mostra ou esconde a seção de "tarefas completas"
    completedTasks.style.display = hasCompletedTasks ? "block" : "none";
}
```

Agora, precisamos chamar `updateUI()` nos lugares certos.
1.  No final da função `addTask()`.
2.  E vamos adicioná-la no final do script, para garantir que a página comece com o estado visual correto.

```javascript
// ... (dentro da função addTask, no final)
        taskInput.focus();
        updateUI(); // Adicione esta linha!
    }
}

// ... (no final do arquivo)
addTaskBtn.addEventListener("click", addTask);
updateUI(); // Adicione esta linha!
```

> **Teste e Debug:** Atualize a página. A imagem deve estar visível. Adicione uma tarefa. A imagem deve sumir.

### Passo 4: Implementar as Ações (Completar e Deletar)

Agora, vamos adicionar a interatividade aos botões que criamos dentro da `createTaskElement`. Precisamos adicionar `EventListeners` a eles.

Volte para a função `createTaskElement` e adicione os listeners.

```javascript
// ... (dentro da função createTaskElement) ...

function createTaskElement(taskText) {
    // ... (criação do taskItem, checkbox, label, etc.) ...

    // Adiciona o evento de "change" ao checkbox
    checkbox.addEventListener("change", () => {
        if (checkbox.checked) {
            taskItem.classList.add("completed");
            completedTasksList.appendChild(taskItem);
        } else {
            taskItem.classList.remove("completed");
            taskList.appendChild(taskItem);
        }
        updateUI(); // Atualiza a UI sempre que uma tarefa muda de lista
    });

    // Adiciona o evento de "click" ao botão de deletar
    deleteBtn.addEventListener("click", () => {
        taskItem.remove();
        updateUI(); // Atualiza a UI após deletar uma tarefa
    });

    // ... (criação do dateInfo e appends) ...
    
    return taskItem;
}
```

> **Teste e Debug:** Salve e atualize. Adicione algumas tarefas. Agora tente:
> * Clicar no checkbox. A tarefa deve se mover para a seção "Completas" e a seção deve aparecer.
> * Desmarcar o checkbox. A tarefa deve voltar para a lista principal.
> * Clicar no `x`. A tarefa deve ser removida.

### Passo 5: Implementar a Limpeza de Tarefas e Adição com "Enter"

Para finalizar, vamos fazer o botão "Limpar Todas as Tarefas Completas" funcionar e adicionar um atalho para que o usuário possa adicionar tarefas pressionando a tecla "Enter".

Adicione o seguinte código no final do seu arquivo `script.js`.

```javascript
// ... (código anterior) ...

// Função para limpar todas as tarefas completas
function clearCompletedTasks() {
    completedTasksList.innerHTML = '';
    updateUI();
}

// Adiciona os listeners de eventos
addTaskBtn.addEventListener("click", addTask);
clearCompletedBtn.addEventListener("click", clearCompletedTasks);

// Listener para permitir adicionar tarefa com a tecla "Enter"
taskInput.addEventListener("keypress", (event) => {
    if (event.key === "Enter") {
        addTask();
    }
});

// Garante que a UI esteja no estado correto ao carregar a página
updateUI();
```

> **Teste e Debug:** Atualize a página, marque algumas tarefas como completas e clique no botão para limpá-las. Tente também adicionar uma nova tarefa digitando no campo e pressionando "Enter".

---

O próximo desafio será reconstruir este mesmo projeto usando **Angular**.

