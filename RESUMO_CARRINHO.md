# Resumo Técnico: Implementação Atual do Carrinho e Loja (LiraMed)

Este documento descreve o estado atual da lógica do catálogo de produtos e do carrinho de compras na aplicação LiraMed. Utilize este resumo como base para transferir ou evoluir a lógica em outro código ou projeto.

## 1. Estrutura da Loja (`loja.html`)

A página da loja possui uma interface estática composta pelos seguintes elementos principais:

*   **Filtros de Categoria:** Botões de filtro (`.filter-btn` dentro de `.store-filters`) para "Todos", "Descartáveis", "Equipamentos" e "EPIs". **Nota:** Atualmente, os botões são apenas visuais e não possuem lógica JavaScript atrelada para ocultar/mostrar os produtos.
*   **Grid de Produtos:** Uma listagem (`.products-grid`) contendo 6 produtos fixos construídos no HTML. 
*   **Card do Produto (`.product-card`):** Cada produto possui imagem, selo opcional (`.product-badge`), categoria, título, preço em texto fixo e um botão de adicionar ao carrinho (`<button class="btn-add-cart">`).
*   **Ícone do Carrinho Flutuante:** Fica fixo na tela (`.cart-float`), contendo um contador (`.cart-count`) que inicia escondido (`display: none`).

## 2. Lógica do Carrinho (`js/auth.js`)

A lógica atual do carrinho está embutida no final do arquivo `js/auth.js`. Trata-se de uma **implementação puramente visual (mock)**, sem persistência de dados. 

### O que o código atual FAZ:
1.  **Contador de Cliques:** Mantém uma variável global simples `let cartCount = 0;`.
2.  **Evento de Clique:** Ao clicar em qualquer botão `.btn-add-cart`:
    *   Incrementa a variável `cartCount`.
    *   Atualiza o número dentro do `.cart-count` e o torna visível (`display: 'flex'`).
3.  **Feedback Visual (Micro-interação):** 
    *   Substitui o texto original do botão clicado por `<i class="fas fa-check"></i> Adicionado`.
    *   Muda a cor de fundo do botão para verde (`var(--secondary-green)`).
    *   Usa um `setTimeout` de 2 segundos (2000ms) para restaurar o botão ao seu estado, texto e cor originais.

### O que o código atual NÃO FAZ (Limitações):
*   **Não armazena os produtos:** Não existe um array, objeto ou banco de dados guardando *quais* produtos foram adicionados.
*   **Não usa LocalStorage:** Ao recarregar a página, o carrinho é zerado e a variável volta para 0.
*   **Sem cálculos de preço:** Não há lógica para somar valores ou multiplicar por quantidade.
*   **Sem menu do carrinho:** O usuário não consegue clicar no ícone do carrinho para abrir uma barra lateral ou modal para ver os itens escolhidos ou removê-los.

## 3. Snippet do Código Original (`js/auth.js`)

```javascript
// Lógica simples de carrinho (apenas visual)
let cartCount = 0;
const addCartBtns = document.querySelectorAll('.btn-add-cart');
const cartCountDisplay = document.querySelector('.cart-count');

addCartBtns.forEach(btn => {
    btn.addEventListener('click', function () {
        cartCount++;
        if (cartCountDisplay) {
            cartCountDisplay.textContent = cartCount;
            cartCountDisplay.style.display = 'flex';
        }

        // Feedback visual no botão
        const originalText = this.innerHTML;
        this.innerHTML = '<i class="fas fa-check"></i> Adicionado';
        this.style.backgroundColor = 'var(--secondary-green)';

        setTimeout(() => {
            this.innerHTML = originalText;
            this.style.backgroundColor = '';
        }, 2000);
    });
});
```
