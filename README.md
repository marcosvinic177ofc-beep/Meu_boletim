# Meu_boletim
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Meu Boletim</title>

<style>
* {
    box-sizing: border-box;
}

body {
    margin: 0;
    font-family: Arial, sans-serif;
    background: #f1f5f9;
    color: #172033;
}

header {
    background: #2563eb;
    color: white;
    text-align: center;
    padding: 25px 15px;
}

header h1 {
    margin: 0;
    font-size: 28px;
}

header p {
    margin: 8px 0 0;
}

.container {
    width: 100%;
    max-width: 700px;
    margin: auto;
    padding: 18px;
}

.card {
    background: white;
    border-radius: 16px;
    padding: 18px;
    margin-bottom: 16px;
    box-shadow: 0 4px 15px rgba(0,0,0,.08);
}

.config {
    display: flex;
    gap: 10px;
}

input {
    width: 100%;
    padding: 12px;
    border: 1px solid #cbd5e1;
    border-radius: 9px;
    font-size: 16px;
    outline: none;
}

input:focus {
    border-color: #2563eb;
}

button {
    border: 0;
    border-radius: 10px;
    padding: 12px 15px;
    font-size: 15px;
    font-weight: bold;
    cursor: pointer;
}

.add {
    width: 100%;
    background: #2563eb;
    color: white;
    margin-top: 12px;
}

.save {
    width: 100%;
    background: #16a34a;
    color: white;
}

.deleteAll {
    width: 100%;
    background: #dc2626;
    color: white;
    margin-top: 10px;
}

.materia {
    background: white;
    border-radius: 16px;
    padding: 18px;
    margin-bottom: 16px;
    box-shadow: 0 4px 15px rgba(0,0,0,.08);
}

.materia-topo {
    display: flex;
    align-items: center;
    gap: 7px;
    margin-bottom: 15px;
}

.materia-nome {
    flex: 1;
    font-size: 20px;
    font-weight: bold;
}

.small {
    padding: 8px 10px;
}

.edit {
    background: #e2e8f0;
}

.remove {
    background: #fee2e2;
    color: #b91c1c;
}

.notas {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
}

.campo label {
    display: block;
    font-weight: bold;
    font-size: 14px;
    margin-bottom: 5px;
}

.resultado {
    margin-top: 15px;
    padding: 13px;
    border-radius: 10px;
    font-weight: bold;
    line-height: 1.5;
}

.aprovado {
    background: #dcfce7;
    color: #166534;
}

.pendente {
    background: #fff7ed;
    color: #9a3412;
}

.vazio {
    text-align: center;
    color: #64748b;
    padding: 20px;
}

.resumo {
    text-align: center;
}

.resumo strong {
    font-size: 28px;
}

@media (max-width: 500px) {
    .notas {
        grid-template-columns: 1fr;
    }
}
</style>
</head>

<body>

<header>
    <h1>📚 Meu Boletim</h1>
    <p>Veja quanto falta para passar</p>
</header>

<div class="container">

    <div class="card">

        <h2>⚙️ Configurações</h2>

        <label>
            Pontos necessários para passar:
        </label>

        <input
            id="meta"
            type="number"
            min="1"
            step="0.1"
            value="24"
        >

        <button
            class="add"
            onclick="adicionarMateria()"
        >
            ➕ Adicionar matéria
        </button>

    </div>

    <div class="card resumo">

        <h2>📊 Resumo</h2>

        <strong id="resumo">
            0 matérias
        </strong>

    </div>

    <div id="listaMaterias">

        <div class="vazio">
            Nenhuma matéria adicionada.
        </div>

    </div>

    <div class="card">

        <button
            class="save"
            onclick="gerarImagem()"
        >
            📸 Salvar boletim como imagem
        </button>

        <button
            class="deleteAll"
            onclick="apagarTudo()"
        >
            🗑️ Apagar boletim
        </button>

    </div>

</div>


<script>

/* =========================
   DADOS
========================= */

let materias = [];

let meta = 24;


/* =========================
   CARREGAR DADOS
========================= */

function carregarDados() {

    try {

        const dados =
            localStorage.getItem("meuBoletimMaterias");

        if (dados) {
            materias = JSON.parse(dados);
        }

        const metaSalva =
            localStorage.getItem("meuBoletimMeta");

        if (metaSalva) {
            meta = Number(metaSalva);
        }

    } catch (erro) {

        console.log("Erro ao carregar:", erro);

        materias = [];
        meta = 24;
    }

    document.getElementById("meta").value = meta;

    mostrarMaterias();
}


/* =========================
   SALVAR DADOS
========================= */

function salvarDados() {

    meta =
        Number(document.getElementById("meta").value) || 24;

    localStorage.setItem(
        "meuBoletimMaterias",
        JSON.stringify(materias)
    );

    localStorage.setItem(
        "meuBoletimMeta",
        meta
    );
}


/* =========================
   ADICIONAR MATÉRIA
========================= */

function adicionarMateria() {

    const nome =
        prompt("Digite o nome da matéria:");

    if (!nome) {
        return;
    }

    const nomeLimpo = nome.trim();

    if (nomeLimpo === "") {
        return;
    }

    materias.push({

        nome: nomeLimpo,

        notas: ["", "", "", ""]

    });

    salvarDados();

    mostrarMaterias();
}


/* =========================
   EDITAR MATÉRIA
========================= */

function editarMateria(index) {

    const novoNome = prompt(
        "Digite o novo nome da matéria:",
        materias[index].nome
    );

    if (!novoNome) {
        return;
    }

    const nomeLimpo = novoNome.trim();

    if (nomeLimpo === "") {
        return;
    }

    materias[index].nome = nomeLimpo;

    salvarDados();

    mostrarMaterias();
}


/* =========================
   EXCLUIR MATÉRIA
========================= */

function excluirMateria(index) {

    const confirmou = confirm(
        "Deseja excluir a matéria " +
        materias[index].nome +
        "?"
    );

    if (!confirmou) {
        return;
    }

    materias.splice(index, 1);

    salvarDados();

    mostrarMaterias();
}


/* =========================
   ALTERAR NOTA
========================= */

function alterarNota(materiaIndex, notaIndex, valor) {

    materias[materiaIndex].notas[notaIndex] =
        valor;

    salvarDados();

    calcular();
}


/* =========================
   MOSTRAR MATÉRIAS
========================= */

function mostrarMaterias() {

    const lista =
        document.getElementById("listaMaterias");

    lista.innerHTML = "";

    if (materias.length === 0) {

        lista.innerHTML = `
            <div class="vazio">
                Nenhuma matéria adicionada.
            </div>
        `;

        atualizarResumo();

        return;
    }

    materias.forEach(function(materia, index) {

        const div =
            document.createElement("div");

        div.className = "materia";

        div.innerHTML = `

            <div class="materia-topo">

                <div class="materia-nome">
                    ${escapar(materia.nome)}
                </div>

                <button
                    class="small edit"
                    onclick="editarMateria(${index})"
                >
                    ✏️
                </button>

                <button
                    class="small remove"
                    onclick="excluirMateria(${index})"
                >
                    🗑️
                </button>

            </div>

            <div class="notas">

                ${campoNota(index, 0, "1º Bimestre")}

                ${campoNota(index, 1, "2º Bimestre")}

                ${campoNota(index, 2, "3º Bimestre")}

                ${campoNota(index, 3, "4º Bimestre")}

            </div>

            <div
                id="resultado-${index}"
                class="resultado pendente"
            >
                Preencha suas notas.
            </div>

        `;

        lista.appendChild(div);

    });

    calcular();
}


/* =========================
   CAMPO DE NOTA
========================= */

function campoNota(
    materiaIndex,
    notaIndex,
    titulo
) {

    const valor =
        materias[materiaIndex].notas[notaIndex];

    return `

        <div class="campo">

            <label>${titulo}</label>

            <input
                type="number"
                min="0"
                max="10"
                step="0.1"
                value="${valor}"
                oninput="
                    alterarNota(
                        ${materiaIndex},
                        ${notaIndex},
                        this.value
                    )
                "
            >

        </div>

    `;
}


/* =========================
   CALCULAR
========================= */

function calcular() {

    meta =
        Number(document.getElementById("meta").value) || 24;

    let aprovadas = 0;
    let pendentes = 0;

    materias.forEach(function(materia, index) {

        let total = 0;
        let temNota = false;

        materia.notas.forEach(function(nota) {

            if (nota !== "") {

                const numero =
                    Number(nota);

                if (!isNaN(numero)) {

                    total += numero;

                    temNota = true;

                }

            }

        });

        const resultado =
            document.getElementById(
                "resultado-" + index
            );

        if (!resultado) {
            return;
        }

        if (!temNota) {

            resultado.className =
                "resultado pendente";

            resultado.innerHTML =
                "📝 Preencha suas notas.";

            pendentes++;

            return;
        }

        if (total >= meta) {

            resultado.className =
                "resultado aprovado";

            resultado.innerHTML =
                "✅ APROVADO!<br>" +
                "Total: " +
                total.toFixed(1) +
                " pontos.";

            aprovadas++;

        } else {

            const falta =
                meta - total;

            resultado.className =
                "resultado pendente";

            resultado.innerHTML =
                "🔴 Total: " +
                total.toFixed(1) +
                " pontos.<br>" +
                "Faltam " +
                falta.toFixed(1) +
                " pontos para chegar aos " +
                meta +
                ".";

            pendentes++;

        }

    });

    document.getElementById("resumo").innerHTML =
        "🟢 " +
        aprovadas +
        " aprovadas<br>" +
        "🔴 " +
        pendentes +
        " pendentes";

    salvarDados();
}


/* =========================
   RESUMO
========================= */

function atualizarResumo() {

    document.getElementById("resumo").innerHTML =
        "0 matérias";

}


/* =========================
   ESCAPAR TEXTO
========================= */

function escapar(texto) {

    return String(texto)
        .replace(/&/g, "&amp;")
        .replace(/</g, "&lt;")
        .replace(/>/g, "&gt;")
        .replace(/"/g, "&quot;")
        .replace(/'/g, "&#039;");

}


/* =========================
   APAGAR TUDO
========================= */

function apagarTudo() {

    const confirmou =
        confirm(
            "Tem certeza que deseja apagar todas as matérias e notas?"
        );

    if (!confirmou) {
        return;
    }

    materias = [];

    meta = 24;

    localStorage.removeItem(
        "meuBoletimMaterias"
    );

    localStorage.removeItem(
        "meuBoletimMeta"
    );

    document.getElementById("meta").value = 24;

    mostrarMaterias();
}


/* =========================
   GERAR IMAGEM
========================= */

function gerarImagem() {

    if (materias.length === 0) {

        alert(
            "Adicione pelo menos uma matéria primeiro."
        );

        return;
    }

    const largura = 900;

    let altura =
        180 + materias.length * 250;

    const canvas =
        document.createElement("canvas");

    canvas.width = largura;
    canvas.height = altura;

    const ctx =
        canvas.getContext("2d");

    /* FUNDO */

    ctx.fillStyle = "#f1f5f9";

    ctx.fillRect(
        0,
        0,
        largura,
        altura
    );


    /* CABEÇALHO */

    ctx.fillStyle = "#2563eb";

    ctx.fillRect(
        0,
        0,
        largura,
        120
    );

    ctx.fillStyle = "white";

    ctx.font =
        "bold 42px Arial";

    ctx.fillText(
        "MEU BOLETIM",
        40,
        65
    );

    ctx.font =
        "22px Arial";

    ctx.fillText(
        "Meta: " + meta + " pontos",
        40,
        98
    );


    /* MATÉRIAS */

    let y = 150;

    materias.forEach(function(materia) {

        let total = 0;

        materia.notas.forEach(function(nota) {

            if (nota !== "") {

                total += Number(nota);

            }

        });


        /* CARTÃO */

        ctx.fillStyle = "white";

        ctx.fillRect(
            30,
            y,
            840,
            215
        );


        /* NOME */

        ctx.fillStyle =
            "#172033";

        ctx.font =
            "bold 30px Arial";

        ctx.fillText(
            materia.nome,
            55,
            y + 42
        );


        /* NOTAS */

        ctx.font =
            "22px Arial";

        ctx.fillText(
            "1º Bimestre: " +
            (materia.notas[0] || "-"),
            55,
            y + 82
        );

        ctx.fillText(
            "2º Bimestre: " +
            (materia.notas[1] || "-"),
            330,
            y + 82
        );

        ctx.fillText(
            "3º Bimestre: " +
            (materia.notas[2] || "-"),
            55,
            y + 120
        );

        ctx.fillText(
            "4º Bimestre: " +
            (materia.notas[3] || "-"),
            330,
            y + 120
        );


        /* TOTAL */

        ctx.font =
            "bold 23px Arial";

        ctx.fillStyle =
            "#172033";

        ctx.fillText(
            "Total: " +
            total.toFixed(1) +
            " pontos",
            55,
            y + 170
        );


        /* STATUS */

        if (total >= meta) {

            ctx.fillStyle =
                "#15803d";

            ctx.fillText(
                "APROVADO ✅",
                580,
                y + 170
            );

        } else {

            ctx.fillStyle =
                "#c2410c";

            ctx.fillText(
                "Falta: " +
                (meta - total).toFixed(1),
                580,
                y + 170
            );

        }

        y += 240;

    });


    /* GERAR PNG */

    canvas.toBlob(function(blob) {

        if (!blob) {

            alert(
                "Não foi possível criar a imagem."
            );

            return;
        }

        const url =
            URL.createObjectURL(blob);


        /*
           ABRE UMA PÁGINA COM A IMAGEM.
           ISSO FUNCIONA MELHOR EM CELULARES
           E NO PREVIEW DO SPCK.
        */

        const pagina =
            window.open("", "_blank");

        if (!pagina) {

            alert(
                "O Spck bloqueou a nova página. " +
                "Abra o projeto no navegador externo."
            );

            URL.revokeObjectURL(url);

            return;
        }


        pagina.document.write(`

            <!DOCTYPE html>

            <html>

            <head>

                <meta
                    name="viewport"
                    content="width=device-width, initial-scale=1.0"
                >

                <title>Meu Boletim</title>

                <style>

                    body {
                        margin: 0;
                        padding: 15px;
                        background: #eeeeee;
                        text-align: center;
                        font-family: Arial;
                    }

                    img {
                        width: 100%;
                        max-width: 900px;
                        height: auto;
                    }

                    .aviso {
                        background: white;
                        padding: 15px;
                        border-radius: 10px;
                        margin-bottom: 15px;
                        font-weight: bold;
                    }

                </style>

            </head>

            <body>

                <div class="aviso">

                    📸 Pressione a imagem
                    e escolha
                    "Salvar imagem".

                </div>

                <img src="${url}">

            </body>

            </html>

        `);

        pagina.document.close();

    }, "image/png");

}


/* =========================
   META
========================= */

document
    .getElementById("meta")
    .addEventListener(
        "input",
        function() {

            meta =
                Number(this.value) || 24;

            salvarDados();

            calcular();

        }
    );


/* =========================
   INICIAR
========================= */

carregarDados();

</script>

</body>
</html>
