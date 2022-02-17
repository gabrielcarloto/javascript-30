# Décimo dia

![](https://ik.imagekit.io/698xlahbaqz/sele%C3%A7%C3%A3o-de-v%C3%A1rias-checkboxes_8r71Ayj2I.gif?ik-sdk-version=javascript-1.4.3&updatedAt=1645127380670)

---

## O que aprendi?

Nesse projeto, acho difícil ter algum link do MDN para colocar aqui, então vou explicar, em minhas palavras, a lógica (ao menos o que entendi).

A ideia é ter vários *checkboxes*, como uma lista de tarefas. Você pode clicar em um normalmente ou clicar em algum, segurar `shift`, clicar em outro e, assim, tanto os que você clicou quanto os que estão entre eles serão selecionados.

A primeira coisa a se fazer, claro, é selecionar os *checkboxes*. Então adicionamos *event listeners* de cliques para cada *checkbox*. Quando o usuário clica em alguma caixa, é executada uma função, nomeada `handleCheck`. Até agora, temos isso:

```js
const checkboxes = document.querySelectorAll('.inbox input[type="checkbox"]');

checkboxes.forEach(checkbox => checkbox.addEventListener('click', handleCheck));
```

A função `handleCheck` recebe como parâmetro os logs do evento para poder verificar se a tecla `shift` foi pressionada e se o *checkbox* foi selecionado. É importante notar que nessa função foi declarada a variável `inBetween`, que inicialmente é falsa. Continuando, caso as condições sejam verdadeiras, então para cada *checkbox* será verificado se é o último selecionado ou que foi selecionado antes desse. Caso uma das condições seja verdadeira, o valor de `inBetween` será invertido (como antes era `false`, passará para `true`). Então, os *checkboxes* onde `inBetween` for verdadeiro serão selecionados.

Tentarei deixar mais fácil de entender. Veja a lista a seguir:

- [ ] Vazio
- [ ] Vazio
- [X] Selecionado
- [ ] Vazio
- [ ] Vazio
- [ ] Vazio
- [ ] Vazio

Imagine que, após selecionar o terceiro item, eu apertei `shift` e selecionei o penúltimo item. `this` corresponde ao que selecionei agora e `lastChecked` o que selecionei anteriormente.

- [ ] Vazio
- [ ] Vazio
- [X] Selecionado `lastChecked`
- [ ] Vazio
- [ ] Vazio
- [X] Selecionado `this`
- [ ] Vazio

Então, como eu disse antes, para cada *checkbox* será verificado se corresponde a `this` ou a `lastChecked`. Inicialmente, o valor de `inBetween` é `false`. Imagine alguém verificando cada caixa e dizendo o valor de `inBetween`. Quando essa pessoa chegar no `lastChecked`, o valor de `inBetween` será invertido, virando `true`, até ela chegar no `this`, onde será invertido novamente, para `false`.

- [ ] Vazio `inBetween = false`
- [ ] Vazio `inBetween = false`
- [X] Selecionado `lastChecked` | `inBetween = true`
- [ ] Vazio `inBetween = true`
- [ ] Vazio `inBetween = true`
- [X] Selecionado `this` | `inBetween = false`
- [ ] Vazio `inBetween = false`

Por fim, onde `inBetween` for verdadeiro, a pessoa deixará o *checkbox* selecionado.

- [ ] Vazio `inBetween = false`
- [ ] Vazio `inBetween = false`
- [X] Selecionado `lastChecked` | `inBetween = true`
- [X] Selecionado `inBetween = true`
- [X] Selecionado `inBetween = true`
- [X] Selecionado `this` | `inBetween = false`
- [ ] Vazio `inBetween = false`

Saindo do português e indo para o js, a função é essa:

```js
let lastChecked;

function handleCheck(e) {
  let inBetween = false;
  if (e.shiftKey && this.checked) {
    checkboxes.forEach(checkbox => {
      if (checkbox === this || checkbox === lastChecked) {
        inBetween = !inBetween;
      }

      if (inBetween) {
        checkbox.checked = true;
      }
    })
  }

  lastChecked = this;
}
```