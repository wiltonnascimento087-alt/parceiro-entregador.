
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Parceiro Entregador</title>
<style>
*{box-sizing:border-box}
body{margin:0;padding:18px;background:#111;color:white;font-family:Arial}
header,.card{padding:20px;border-radius:14px;margin-bottom:16px}
header{background:#ffcc00;color:#111}
.card{background:#222}
h2{color:#ffcc00}
input,select{width:100%;padding:13px;margin:7px 0;border:0;border-radius:8px;font-size:16px}
button{width:100%;padding:14px;margin-top:10px;border:0;border-radius:8px;background:#ffcc00;color:#111;font-weight:bold;font-size:16px}
.entrega{padding:12px 0;border-bottom:1px solid #555}
</style>
</head>
<body>

<header>
<h1>🏍️ Parceiro Entregador</h1>
<p>Suas entregas em um só lugar</p>
</header>

<div class="card">
<h2>Nova entrega</h2>
<input id="cliente" placeholder="Nome do destinatário">
<input id="telefone" placeholder="Telefone">
<input id="retirada" placeholder="Endereço de retirada">
<input id="destino" placeholder="Endereço de destino">
<input id="referencia" placeholder="Ponto de referência">

<select id="transporte">
<option value="Moto">Moto</option>
<option value="Bicicleta">Bicicleta</option>
</select>

<input id="km" type="number" min="0" step="0.1" placeholder="Distância em km">

<button onclick="calcular()">Calcular preço</button>
<h2 id="valor">Valor: R$ 0,00</h2>
<button onclick="salvar()">Cadastrar entrega</button>
</div>

<div class="card">
<h2>Minhas entregas</h2>
<div id="lista">Nenhuma entrega cadastrada.</div>
</div>

<script>
let entregas=[];

function preco(){
 const km=Number(document.getElementById("km").value)||0;
 const tipo=document.getElementById("transporte").value;
 return tipo==="Moto"
 ? Math.max(2,km*2.5)
 : Math.max(9,Math.max(3,km)*3);
}

function calcular(){
 document.getElementById("valor").textContent=
 "Valor: R$ "+preco().toFixed(2).replace(".",",");
}

function salvar(){
 const cliente=document.getElementById("cliente").value.trim();
 const destino=document.getElementById("destino").value.trim();
 const retirada=document.getElementById("retirada").value.trim();
 const km=Number(document.getElementById("km").value);

 if(!cliente||!destino||!retirada||!km||km<=0){
 alert("Preencha nome, retirada, destino e distância.");
 return;
 }

 entregas.push({
 cliente,
 destino,
 retirada,
 telefone:document.getElementById("telefone").value,
 referencia:document.getElementById("referencia").value,
 transporte:document.getElementById("transporte").value,
 valor:preco(),
 status:"Pendente"
 });

 mostrar();
 alert("Entrega cadastrada!");
}

function mostrar(){
 const lista=document.getElementById("lista");
 lista.innerHTML="";

 entregas.forEach((e,i)=>{
 const item=document.createElement("div");
 item.className="entrega";

 const info=document.createElement("p");
 info.textContent=e.cliente+" — "+e.destino+
 " | R$ "+e.valor.toFixed(2)+
 " | "+e.status;

 const botao=document.createElement("button");
 botao.textContent="Concluir entrega";
 botao.onclick=()=>{
 e.status="Concluída";
 mostrar();
 };

 item.append(info,botao);
 lista.appendChild(item);
 });
}
</script>

</body>
</html>
