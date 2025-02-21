# myissues
sz
Adicione este código ao seu arquivo .aspx (ou em um arquivo separado .js):

javascript
Copiar
Editar
document.addEventListener("DOMContentLoaded", function () {
    let tabela = document.getElementById("tabelaCavaleiros"); // ID da sua tabela
    let linhas = tabela.getElementsByTagName("tr");
    let cavaleiros = {}; // Objeto para armazenar os cavaleiros e suas posições

    for (let i = 1; i < linhas.length; i++) { // Começa de 1 para ignorar o cabeçalho
        let celulaNome = linhas[i].getElementsByTagName("td")[0]; // Ajuste o índice conforme necessário
        let nome = celulaNome.textContent.trim();

        if (cavaleiros[nome]) {
            // Verifica se a repetição está dentro das últimas 7 posições
            let posicoes = cavaleiros[nome];
            for (let pos of posicoes) {
                if (Math.abs(i - pos) < 7) {
                    // Pinta a linha atual e a anterior de vermelho
                    linhas[i].style.backgroundColor = "#FF6666";
                    linhas[pos].style.backgroundColor = "#FF6666";
                }
            }
            cavaleiros[nome].push(i);
        } else {
            cavaleiros[nome] = [i];
        }
    }
});
Explicação:
Obtém a referência da tabela (tabelaCavaleiros).
Percorre todas as linhas da tabela.
Salva o nome do cavaleiro e a posição da linha.
Se o nome já apareceu antes, verifica se a diferença de posição é menor que 7.
Se for, pinta a linha de vermelho (#FF6666).
Exemplo de Uso no ASPX
No seu arquivo Web Forms (.aspx), a tabela deve ter um ID para que o JavaScript funcione corretamente:

html
Copiar
Editar
<table id="tabelaCavaleiros" border="1">
    <tr>
        <th>Nome do Cavaleiro</th>
    </tr>
    <tr>
        <td>João Silva</td>
    </tr>
    <tr>
        <td>Maria Souza</td>
    </tr>
    <tr>
        <td>João Silva</td> <!-- Deve ser pintado de vermelho se a condição for atendida -->
    </tr>
</table>

<script src="script.js"></script> <!-- Inclua o script no final do body -->
Caso os dados sejam carregados dinamicamente, garanta que esse script seja chamado após a renderização completa da página.
