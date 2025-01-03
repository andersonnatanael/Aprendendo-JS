# Guia de Variáveis em JavaScript

## Regras de Nomeação de Variáveis

### Regras Obrigatórias
1. **Caracteres Permitidos**
   - Letras (a-z, A-Z)
   - Números (0-9)
   - Underscore (_)
   - Cifrão ($)

2. **Restrições**
   - Deve começar com letra, underscore ou cifrão
   - Não pode começar com número
   - Não pode usar palavras reservadas
   - Case sensitive (`nome` ≠ `Nome`)

```javascript
// Nomes válidos
let nome = "João";
let $preco = 99.99;
let _idade = 25;
let usuario123 = "Maria";
let minhaVariavel = "valor";

// Nomes inválidos
// let 123nome = "João";    // Não pode começar com número
// let meu-nome = "João";   // Não pode usar hífen
// let class = "valor";     // Não pode usar palavra reservada
```

### Convenções (Boas Práticas)

1. **camelCase**
   - Para variáveis e funções
   ```javascript
   let nomeCompleto = "João Silva";
   let idadeUsuario = 25;
   let calcularTotal = () => {};
   ```

2. **PascalCase**
   - Para classes e construtores
   ```javascript
   class UsuarioAdmin {}
   class CarrinhoCompras {}
   ```

3. **SNAKE_CASE_MAIÚSCULO**
   - Para constantes imutáveis
   ```javascript
   const MAX_TENTATIVAS = 3;
   const API_BASE_URL = "https://api.exemplo.com";
   ```

4. **Prefixos e Sufixos Significativos**
   ```javascript
   let isAtivo = true;              // Booleanos com is, has, can
   let shouldUpdate = true;
   let hasPermission = false;
   
   let userList = [];               // Arrays no plural
   let productIds = [];
   
   let onClickHandler = () => {};   // Handlers com on ou handle
   let handleSubmit = () => {};
   ```

## Hoisting (Elevação)

### O que é Hoisting?
- Comportamento do JavaScript de "mover" declarações para o topo do seu escopo
- Afeta declarações de variáveis e funções
- Apenas a declaração é elevada, não a inicialização

### 1. Hoisting com var

```javascript
console.log(nome);     // undefined (não dá erro!)
var nome = "João";     

// O código acima é interpretado como:
var nome;              // Declaração é elevada
console.log(nome);     // undefined
nome = "João";         // Inicialização permanece no lugar
```

### 2. Hoisting com let e const
- Existe tecnicamente, mas de forma diferente
- Cria uma "Temporal Dead Zone" (TDZ)
- Gera erro se tentar acessar antes da declaração

```javascript
// console.log(idade);    // Erro! Cannot access 'idade' before initialization
let idade = 25;

// console.log(API_KEY);  // Erro! Cannot access 'API_KEY' before initialization
const API_KEY = "123456";
```

### 3. Hoisting com Funções
- Funções declaradas com `function` são completamente elevadas
- Função e seu corpo são elevados
- Function expressions não são elevadas

```javascript
// Funciona! Declaração completa é elevada
sayHello();

function sayHello() {
    console.log("Olá!");
}

// NÃO funciona! Function expression não é elevada
// sayGoodbye();  // Erro!

const sayGoodbye = function() {
    console.log("Tchau!");
};
```

### 4. Exemplos Práticos de Hoisting

```javascript
// Exemplo 1: Hoisting em diferentes escopos
var x = 1;

function exemplo() {
    console.log(x); // undefined (não 1!)
    var x = 2;
    console.log(x); // 2
}

// Exemplo 2: Interação entre function e var hoisting
console.log(typeof minhaFuncao); // "function"
function minhaFuncao() {}
var minhaFuncao;

// Exemplo 3: Problema comum com loops
for (var i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000); // Imprime 3, 3, 3
}

// Solução com let
for (let i = 0; i < 3; i++) {
    setTimeout(() => console.log(i), 1000); // Imprime 0, 1, 2
}
```

### Boas Práticas com Hoisting

1. Declare todas as variáveis no topo do seu escopo
2. Use `let` e `const` para evitar problemas de hoisting
3. Declare funções antes de usá-las
4. Evite depender do hoisting para a lógica do código
5. Mantenha o código mais previsível e legível

```javascript
// Boa prática: Declarações no topo
function exemploBomCodigo() {
    const MAX_ITEMS = 100;
    let count = 0;
    let result = [];
    
    // Resto do código...
}
```

# Guia de Variáveis em JavaScript
## Escopos de Variáveis

### 1. Escopo Global
- Variáveis declaradas fora de qualquer função ou bloco
- Acessíveis em qualquer parte do código
- Podem causar problemas de colisão de nomes
- Ocupam memória durante toda a execução do programa

```javascript
// Variável global
let globalVariable = "Sou global!";

function testScope() {
    console.log(globalVariable); // Acessível aqui
}

if (true) {
    console.log(globalVariable); // Também acessível aqui
}
```

### 2. Escopo de Função
- Variáveis declaradas dentro de uma função
- Acessíveis apenas dentro da função onde foram declaradas
- Protegidas do acesso externo
- Liberadas da memória quando a função termina

```javascript
function functionScope() {
    let functionVariable = "Sou local à função!";
    console.log(functionVariable); // Acessível
    
    function nestedFunction() {
        console.log(functionVariable); // Também acessível
    }
}

// console.log(functionVariable); // Erro! Não acessível fora da função
```

### 3. Escopo de Bloco
- Variáveis declaradas dentro de blocos (if, for, while, etc.)
- Apenas variáveis declaradas com `let` e `const` respeitam este escopo
- Acessíveis apenas dentro do bloco onde foram declaradas
- Ideal para loops e condicionais

```javascript
if (true) {
    let blockVariable = "Sou local ao bloco!";
    const alsoBlockScoped = "Também sou local ao bloco!";
    console.log(blockVariable); // Acessível
}

// console.log(blockVariable); // Erro! Não acessível fora do bloco
```

## Declaração de Variáveis

### 1. var
- Escopo de função ou global
- Pode ser redeclarada
- Sofre hoisting (elevação)
- Não respeita escopo de bloco
- **Não recomendado** para código moderno

```javascript
function varExample() {
    var x = 1;
    if (true) {
        var x = 2; // Mesma variável!
        console.log(x); // 2
    }
    console.log(x); // 2 - valor foi alterado
}

// Hoisting com var
console.log(hoistedVar); // undefined
var hoistedVar = "Fui elevada!";
```

### 2. let
- Escopo de bloco
- Não pode ser redeclarada no mesmo escopo
- Não sofre hoisting
- Pode ter seu valor alterado
- **Recomendado** para variáveis que mudam

```javascript
function letExample() {
    let x = 1;
    if (true) {
        let x = 2; // Nova variável
        console.log(x); // 2
    }
    console.log(x); // 1 - valor original mantido
}

// Temporal Dead Zone (TDZ)
// console.log(notHoisted); // Erro!
let notHoisted = "Não sou elevada!";
```

### 3. const
- Escopo de bloco
- Não pode ser redeclarada
- Não pode ter seu valor reatribuído
- Não sofre hoisting
- **Recomendado** para valores constantes

```javascript
const PI = 3.14159;
// PI = 3.14; // Erro! Não pode ser reatribuída

// Objetos const podem ter suas propriedades modificadas
const user = {
    name: "João"
};
user.name = "Maria"; // Permitido
// user = {}; // Erro! Não pode reatribuir o objeto

// Arrays const podem ser modificados
const numbers = [1, 2, 3];
numbers.push(4); // Permitido
// numbers = []; // Erro! Não pode reatribuir o array
```

### Boas Práticas
1. Use `const` por padrão
2. Use `let` quando precisar reatribuir valores
3. Evite `var`
4. Minimize o uso de variáveis globais
5. Declare variáveis no escopo mais restrito possível
6. Use nomes descritivos e significativos

### Exemplos de Uso Combinado

```javascript
// Exemplo prático de diferentes escopos
const API_KEY = "12345"; // Constante global

function fetchUserData() {
    let userData = null; // Escopo de função
    
    try {
        const response = { status: 200 }; // Escopo de bloco
        if (response.status === 200) {
            let data = { name: "João" }; // Escopo de bloco
            userData = data;
        }
    } catch (error) {
        const errorMessage = "Falha ao buscar dados"; // Escopo de bloco
        console.error(errorMessage);
    }
    
    return userData;
}

// Exemplo de closure com diferentes escopos
function createCounter() {
    const counterName = "Meu Contador"; // Escopo de função, constante
    let count = 0; // Escopo de função, mutável
    
    return {
        increment() {
            count++;
            console.log(`${counterName}: ${count}`);
        },
        getCount() {
            return count;
        }
    };
}
```