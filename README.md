# AgileFingers - Script Autoplay (Absolute Mode) 🚀

Um script de automação de alta performance para o site AgileFingers, garantindo sincronia total com o DOM e uma velocidade de digitação agressiva.

---

## 📋 Compatibilidade
O script possui um sistema de segurança que valida a página antes de iniciar. Ele só funcionará nas seguintes URLs:
* `https://agilefingers.com/*/teste/*` (Onde `*` representa qualquer idioma ou tarefa).

**Exemplos válidos:**
* `.../pt/teste/tarefa-texto-1`
* `.../pt/teste/tarefa-palavras-aleatorias`
* `.../en/teste/tarefa-numeros-palavras-aleatorios`

---

## 🛠️ Como Instalar e Usar

1. Acesse uma das páginas de teste do [AgileFingers]([https://agilefingers.com/pt/teste]).
2. Pressione `F12` (ou `Ctrl+Shift+I`) para abrir o Console.
3. Copie o código abaixo na íntegra.
4. Cole no console e pressione `Enter`.

---

## 💻 Script (Copiar Abaixo)

```javascript
(function agileFingersAbsolute() {
    // Validação de URL: Garante que o script só rode nas páginas de teste
    const urlPattern = /^https:\/\/agilefingers\.com\/.*\/teste\/.*/;

    if (!urlPattern.test(window.location.href)) {
        console.log("⏳ Aguardando usuário ir para a página específica...");
        console.warn("Este script só executa em páginas de teste, como:\n" +
                     "- [https://agilefingers.com/pt/teste/tarefa-texto-2](https://agilefingers.com/pt/teste/tarefa-texto-2)\n" +
                     "- [https://agilefingers.com/pt/teste/tarefa-numeros-palavras-aleatorios](https://agilefingers.com/pt/teste/tarefa-numeros-palavras-aleatorios)\n" +
                     "- [https://agilefingers.com/pt/teste/tarefa-texto-1](https://agilefingers.com/pt/teste/tarefa-texto-1)\n" +
                     "- [https://agilefingers.com/pt/teste/tarefa-palavras-aleatorias](https://agilefingers.com/pt/teste/tarefa-palavras-aleatorias)");
        return; 
    }

    console.log("🚀 MODO ABSOLUTE: URL Validada. Forçando sincronia total...");

    const target = document.querySelector('input.inserted-text') || document.querySelector('input');
    
    if (!target) {
        console.error("❌ Alvo não encontrado. Clique no campo de digitação e tente novamente.");
        return;
    }

    function getNextChar() {
        const firstLetterEl = document.querySelector('.first-letter');
        if (!firstLetterEl) return null;
        let char = firstLetterEl.innerText;
        // Tratamento de espaços especiais
        return (char === "" || char === "\u00A0") ? " " : char;
    }

    function type() {
        const char = getNextChar();
        
        if (char) {
            target.focus();

            // Simulação de eventos de teclado para máxima compatibilidade
            const keyOpts = { key: char, keyCode: char.charCodeAt(0), bubbles: true };
            target.dispatchEvent(new KeyboardEvent('keydown', keyOpts));

            // Injeção de texto via comando de sistema
            document.execCommand('insertText', false, char);

            // Disparo de eventos de entrada para o site validar o caractere
            target.dispatchEvent(new InputEvent('input', { data: char, bubbles: true }));
            target.dispatchEvent(new KeyboardEvent('keyup', keyOpts));

            // Delay de 10ms (Velocidade Extrema)
            setTimeout(type, 10);
        } else {
            // Se a próxima letra ainda não carregou, tenta novamente em 1ms
            setTimeout(type, 1);
        }
    }

    type();
})();
