# Vigésimo quinto dia

<p style="text-align: center; opacity: .67">(sem preview)</p>

---

## O que aprendi?

- Opções dos event listeners
  - [`capture`](https://developer.mozilla.org/pt-BR/docs/Web/API/EventTarget/addEventListener#:~:text=en%2DUS)%C2%A0JavaScript.-,useCapture%C2%A0,-Optional)
  - `once` 
    - Não encontrei na MDN, mas o que ele faz é rodar o listener apenas uma vez. Por exemplo, se tem um event listener de click com essa opção e, ao clicar, ele loga "Clicado!", não importa quantas vezes você clicar (desde que clique pelo menos uma vez), ele fará o log apenas uma vez.
- [`e.stopPropagation()`](https://developer.mozilla.org/pt-BR/docs/Web/API/Event/stopPropagation)