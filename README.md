<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checklist de Pagamento - Retiro</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background: url('https://www.transparenttextures.com/patterns/aged-paper.png');
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }
        .container {
            background: #fdf5e6;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 800px;
            border: 2px solid #d4af37;
        }
        h2, h3 {
            text-align: center;
            color: #5a3e1b;
            font-family: 'Georgia', serif;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #d4af37;
            padding: 10px;
            text-align: center;
        }
        th {
            background-color: #c4a484;
            color: white;
        }
        .despesas-container {
            margin-top: 20px;
            padding: 15px;
            background: #fdf5e6;
            border-radius: 8px;
            box-shadow: 0px 5px 10px rgba(0, 0, 0, 0.1);
            text-align: left;
            border: 2px solid #d4af37;
        }
        .despesas-container h3 {
            text-align: center;
            border-bottom: 2px solid #c4a484;
            padding-bottom: 5px;
            margin-bottom: 10px;
        }
        .despesas-container p {
            font-size: 16px;
            font-weight: bold;
            margin-left: 20px;
        }
        .despesas-container span {
            color: red;
        }
        .verde { color: green; font-weight: bold; }
        .amarelo { color: orange; font-weight: bold; }
        .preto { color: black; }
        #totalPago { color: black !important; font-weight: bold; }
        #totalFaltante { color: red !important; font-weight: bold; }
        #saldoTotal { color: blue !important; font-weight: bold; }
        .obs-text { color: black; font-weight: bold; text-align: center; margin-top: 10px; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Checklist de Pagamento - Retiro</h2>
        <table>
            <tr>
                <th>Nome</th>
                <th>1ª Parcela (16/04) - R$ 115,00</th>
                <th>2ª Parcela (19/05) - R$ 115,00</th>
                <th>À Vista - R$ 230,00</th>
            </tr>
            <tbody id="tabelaPagamentos"></tbody>
        </table>
        
        <h3>Total Pago: R$ <span id="totalPago">0,00</span></h3>
        <h3>Faltando: R$ <span id="totalFaltante">0,00</span></h3>
        
        <div class="despesas-container">
            <h3>Despesas</h3>
            <p>Casa de Retiro: R$ <span id="despesaCasa">0,00</span> (R$ 60,00 por pessoa)</p>
            <p>Alimentação: R$ <span id="despesaAlimentacao">0,00</span> (R$ 75,00 por pessoa)</p>
        </div>
        
        <h3>Saldo Total: R$ <span id="saldoTotal">0,00</span></h3>
        <p class="obs-text"> OBS: "O saldo total será utilizado para a compra de flores e lembranças."</p>
    </div>
    
    <script>
        let participantes = [
            "Alex Fernando", "Kelly Cristina", "Bartholomeu Francisco", "Bianca Cristina da Cunha", "Edivaldo José",
            "Bianca Cristina Vidal", "Érika Pereira", "Flávia Maria", "Laura Dias", "Lilian Beatriz", "Luis Alexandre",
            "Mara Rúbia", "Maria Helena", "Pedro Francisco", "Raissa Souza", "Sabrina Magalhães", "Walkner Pinheiro",
            "Bárbara Evelin", "Rafael", "Silmara", "Nayara"
        ];
        
        let tabela = document.getElementById("tabelaPagamentos");
        let totalPagoElement = document.getElementById("totalPago");
        let totalFaltanteElement = document.getElementById("totalFaltante");
        let despesaCasaElement = document.getElementById("despesaCasa");
        let despesaAlimentacaoElement = document.getElementById("despesaAlimentacao");
        let saldoTotalElement = document.getElementById("saldoTotal");
        
        participantes.forEach(nome => {
            let row = document.createElement("tr");
            row.innerHTML = `
                <td class="nome preto">${nome}</td>
                <td><input type="checkbox" class="parcela1" data-value="115"></td>
                <td><input type="checkbox" class="parcela2" data-value="115"></td>
                <td><input type="checkbox" class="avista" data-value="230"></td>
            `;
            tabela.appendChild(row);
        });
        
        function calcularTotal() {
            let totalPago = 0;
            let totalPessoas = participantes.length;
            let pessoasPagas = 0;

            document.querySelectorAll("input[type='checkbox']:checked").forEach(checkbox => {
                totalPago += parseFloat(checkbox.dataset.value);
                pessoasPagas++;
            });

            let totalEsperado = totalPessoas * 230;
            let totalFaltante = totalEsperado - totalPago;

            totalPagoElement.textContent = totalPago.toFixed(2);
            totalFaltanteElement.textContent = totalFaltante.toFixed(2);

            let despesaCasa = pessoasPagas * 60;
            let despesaAlimentacao = pessoasPagas * 75;

            despesaCasaElement.textContent = despesaCasa.toFixed(2);
            despesaAlimentacaoElement.textContent = despesaAlimentacao.toFixed(2);

            let saldoTotal = totalPago - despesaCasa - despesaAlimentacao;
            saldoTotalElement.textContent = saldoTotal.toFixed(2);
            
            document.querySelectorAll("input[type='checkbox']").forEach(checkbox => {
                let row = checkbox.closest("tr");
                let nomeCell = row.querySelector(".nome");
                
                if (checkbox.checked) {
                    if (checkbox.classList.contains("avista")) {
                        nomeCell.classList.remove("amarelo", "preto");
                        nomeCell.classList.add("verde");
                    } else if (checkbox.classList.contains("parcela1") || checkbox.classList.contains("parcela2")) {
                        nomeCell.classList.remove("verde", "preto");
                        nomeCell.classList.add("amarelo");
                    }
                } else {
                    let outrosMarcados = row.querySelector(".parcela1").checked || row.querySelector(".parcela2").checked || row.querySelector(".avista").checked;
                    if (!outrosMarcados) {
                        nomeCell.classList.remove("verde", "amarelo");
                        nomeCell.classList.add("preto");
                    }
                }
            });
        }
        
        document.querySelectorAll("input[type='checkbox']").forEach(checkbox => {
            checkbox.addEventListener("change", calcularTotal);
        });
        
        calcularTotal();
    </script>
</body>
</html>
