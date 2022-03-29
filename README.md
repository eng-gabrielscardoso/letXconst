# Let X Const

**Autor:** Gabriel Santos Cardoso - 202111140007

A  proposta de atividade foi analisar o seguinte código e explicar os diferentes escopos entre Let e Const da linguagem Javascript:

```jsx
//* ****** EcmaScript 2015 (ES6): variables scope (https://goo.gl/NbsVvg)
let movie = 'Lord of the Rings';

function starWarsFan() {
  const movie = 'Star Wars';
  return movie;
}

function marvelFan() {
  movie = 'The Avengers';
  return movie;
}

console.log(movie); 
console.log(starWarsFan()); 
console.log(marvelFan()); 
console.log(movie); 

function blizzardFan() {
  const isFan = true;
  let phrase = 'Warcraft'; 
  console.log('Before if: ' + phrase);
  if (isFan) {
    let phrase = 'initial text'; 
    phrase = 'For the Horde!'; 
    console.log('Inside if: ' + phrase);
  }
  phrase = 'For the Alliance!'; 
  console.log('After if: ' + phrase);
}

blizzardFan();
```

A partir do código acima, tem-se a seguinte saída no console:

```
Lord of the Rings
Star Wars
The Avengers
The Avengers
Before if: Warcraft
Inside if: For the Horde!
After if: For the Alliance!
```

---

## Análise

### Palavras utilizadas

De acordo com a documentação MDN, as palavras reservadas da linguagem Javascript:

**var:** declara uma variável, opcionalmente é possível atribuir à ela um valor em sua inicialização.

**let:** Declara uma variável local no escopo do bloco atual, opcionalmente iniciando-a com um valor.

**const:** Declara constantes, que possuem escopo de bloco, semelhantes às variáveis declaradas usando o palavra-chave *let*. O valor de uma constante não pode ser alterado por uma atribuição, e ela não pode ser redeclarada.

A partir disso, entende-se que:

1. Ao declarar uma variável com a palavra reservada *var,* ela terá escopo global ou escopo de bloco
2. Ao declarar uma variável com a palavra reservada *const e let*, ela terá escopo de bloco
3. Variáveis declaradas com *var* podem ser atualizadas **e** redeclaradas
4. Variáveis declaradas com *let* podem ser atualizadas **mas não podem ser** redeclaradas

### Entendimento

```jsx
let movie = 'Lord of the Rings';
```

A linha acima apenas declara a variável utilizando a palavra reservada *let,* fora de qualquer tipo de bloco de código, o que confere a essa variável um escopo local no código inteiro sendo, portanto, um escopo global.

```jsx
function starWarsFan() {
  const movie = 'Star Wars';
  return movie;
}
```

O bloco acima se trata de uma declaração de constante dentro de uma função, o que confere à essa constante um escopo local na função *starWarsFan().* Assim, mesmo se tratando de uma constante, a sua declaração não sobreescreve a declaração da variável *movie* com a palavra *let* que possui um escopo maior.

```jsx
function marvelFan() {
  movie = 'The Avengers';
  return movie;
}
```

Nesse caso, a variável *movie* está sendo atualizada e passada como retorno da função *marvelFan().*

```jsx
console.log(movie); // Lord of the Rings -> console.log(movie);
console.log(starWarsFan()); // Star Wars -> console.log(starWarsFan());
console.log(marvelFan()); // The Avengers -> console.log(marvelFan());
console.log(movie); // The Avengers -> console.log(movie);
```

A primeira saída retorna o valor da variável *movie* declarada com *let*, sem qualquer tipo de comportamento inesperado.

A segunda linha tem como saída o retorno da função *starWarsFan()*, que no caso é a constante *movie*, declarada localmente na função, sem qualquer tipo de comportamento inesperado .

A terceira linha, no entanto, fez a chamada de uma função que possui como retorno uma variável inicializada sem ser declarada. Sua saída é condizente com o comportamento esperado. 

Ao observar a quarta linha, é de se esperar que a saída seja novamente *Lord of the rings*, porém, tem-se como saída o valor retornado pela função *marvelFan().*

Esse fato se deve a um comportamento do Javascript denominado *Hoisting.* De acordo com a MDN, conceitualmente, por exemplo, uma definição estrita de elevação sugere  que as declarações de variáveis e funções sejam fisicamente movidas para o  topo do seu código, mas isso não é realmente o que acontece. Em vez  disso, as declarações de variável e função são colocadas na memória  durante a fase de *compilação* , mas permanecem exatamente onde estavam no momento da escrita do código.

Dessa forma, o que ocorre é que, ao ser inicializada, a variável *movie* dentro do escopo da função é içada para um escopo global, daí seu retorno na chamada seguinte da sua invocação pela função que a delimita.

Outra explicação seria a de que, ao ser atualizada, o valor que estava na variável *movie* no escopo da função ao ser invocada no escopo global acabou escapando do seu escopo e fazendo referência ao escopo global.

```jsx
function blizzardFan() {
  const isFan = true;
  let phrase = 'Warcraft'; 
  console.log('Before if: ' + phrase);
  if (isFan) {
    let phrase = 'initial text'; 
    phrase = 'For the Horde!'; 
    console.log('Inside if: ' + phrase);
  }
  phrase = 'For the Alliance!'; 
  console.log('After if: ' + phrase);
}

blizzardFan();
```

Na função acima, a declaração das variáveis com *let* e *const* não sofrem *hoisting* por padrão e mantém um comportamento esperado. No entanto, a atualização do valor da variável *phrase*  sobreescreve o valor da declaração *let*, assumindo os valores apresentados nas saídas do console.

---

## Referências

MOZILLA. Fundação Mozilla (org.). **Const**.  c2022.  Documentação retirada da MDN Web Docs.  Disponível em: <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/const>. Acesso em: 28 mar. 2022.

MOZILLA. Fundação Mozilla (org.). **Hoisting**.  c2022.  Documentação retirada da MDN Web Docs.  Disponível em: <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/hoisting>. Acesso em: 28 mar. 2022.

MOZILLA. Fundação Mozilla (org.). **Let**.  c2022.  Documentação retirada da MDN Web Docs.  Disponível em: <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/let>. Acesso em: 28 mar. 2022.

MOZILLA. Fundação Mozilla (org.). **Var**.  c2022.  Documentação retirada da MDN Web Docs.  Disponível em: <https://developer.mozilla.org/pt-BR/docs/Web/JavaScript/Reference/Statements/var>. Acesso em: 28 mar. 2022.

---

## Softwares utilizados

[Node.js](https://nodejs.org/en/)

[Visual Studio Code - Code Editing. Redefined](https://code.visualstudio.com/)