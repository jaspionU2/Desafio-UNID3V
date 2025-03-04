# Desafio-UNID3V

![UNID3V-BACKEND](https://github.com/user-attachments/assets/d9876864-5c3f-428f-9177-1f01bc120cc1)


## QUERIES

```
-- Total de Vendas por Mês
SELECT 
    DATE_FORMAT(RV.data_compra, '%M-%Y') AS mes_ano,
    COUNT(VB.ID_venda_bolo) AS vendas_mes
FROM Registro_Venda as RV
JOIN Venda_Bolo VB ON RV.ID_registro_venda = VB.FK_registro_venda
GROUP BY mes_ano
ORDER BY vendas_mes DESC;

-- Total de Unidades Vendidadas por Mês
SELECT
	DATE_FORMAT(RV.data_compra, '%M-%Y') AS mes_ano,
    SUM(VB.quantidade) as venda_bolos_mes
FROM Registro_Venda as RV
JOIN Venda_Bolo as VB on RV.ID_registro_venda = VB.FK_registro_venda
GROUP BY mes_ano
ORDER BY venda_bolos_mes DESC;

-- Receita e Total de Vendas por Ano
SELECT
    DATE_FORMAT(RV.data_compra, '%Y') AS ano,
    SUM(VB.quantidade) AS total_vendas,
	SUM(VB.quantidade * B.preco_bolo) AS receita_ano
FROM Registro_Venda AS RV
JOIN Venda_Bolo AS VB ON RV.ID_registro_venda = VB.FK_registro_venda
JOIN Bolo as B ON VB.FK_bolo = B.ID_bolo
GROUP BY ano
ORDER BY total_vendas DESC;

-- Gastos e Compras por Cliente
SELECT
	C.nome_cliente as nome,
	COUNT(RV.FK_cliente) as total_compras_cliente,
    SUM(VB.quantidade * B.preco_bolo) as gasto_cliente
FROM Registro_Venda as RV
join Venda_Bolo as VB on RV.ID_registro_venda = VB.FK_registro_venda
join Bolo as B on VB.FK_bolo = B.ID_bolo
join Cliente as C ON RV.FK_cliente = C.ID_cliente
GROUP BY nome 
ORDER BY gasto_cliente DESC;

-- Total de Pagamentos por Tipo
SELECT
	FP.forma_pagamento as tipo_pagamento,
    COUNT(RV.FK_forma_pagamento) as pagamentos_feitos
FROM Registro_Venda as RV
JOIN Forma_Pagamento as FP on RV.FK_forma_pagamento = FP.ID_forma_pagamento
GROUP BY tipo_pagamento
ORDER BY pagamentos_feitos DESC;

-- Validade dos Bolos no Estoque
SELECT
	B.sabor_bolo as bolo,
	TB.prazo_validade as validade,
    EB.data_de_entrada as recebido,
    date_add(EB.data_de_entrada, interval TB.prazo_validade day) as vencimento_estimado
from Estoque_Bolo as EB
JOIN Bolo as B on EB.FK_bolo = B.ID_bolo
join Tipo_Bolo as TB on B.FK_tipo_bolo = TB.ID_tipo_bolo
ORDER BY vencimento_estimado ASC;

-- Clientes que Compraram no Último Mês
SELECT
	C.nome_cliente as nome,
    C.telefone_cliente as telefone,
    C.email_cliente as email,
    RV.data_compra,
    curdate() as data_atual
FROM Cliente as C
JOIN Registro_Venda AS RV ON C.ID_cliente = RV.FK_cliente
where RV.data_compra >= date_sub(curdate(), interval 1 month)
ORDER BY data_compra ASC;

-- Quantidade de Clientes por Cidade e Estado
SELECT
	E.cidade,
    E.estado,
    COUNT(CE.FK_cliente) as quant_clientes
FROM Cliente_Endereco as CE
JOIN Cliente as C on CE.FK_cliente = C.ID_cliente
JOIN Endereco as E ON CE.FK_endereco = E.ID_endereco
GROUP BY estado, cidade
ORDER BY quant_clientes DESC;

SELECT
    E.cidade,
    E.estado,
    COUNT(CE.FK_cliente) AS quant_clientes
FROM Cliente_Endereco AS CE
JOIN Cliente AS C ON CE.FK_cliente = C.ID_cliente
JOIN Endereco AS E ON CE.FK_endereco = E.ID_endereco
GROUP BY E.estado, E.cidade
ORDER BY quant_clientes DESC;

-- Tipos de bolos mais vendidos
SELECT 
    TB.tipo_bolo, 
    SUM(VB.quantidade) AS total_vendas
FROM Venda_Bolo AS VB
JOIN Bolo AS B ON VB.FK_bolo = B.ID_bolo
JOIN Tipo_Bolo AS TB ON B.FK_tipo_bolo = TB.ID_tipo_bolo
GROUP BY TB.ID_tipo_bolo
ORDER BY total_vendas DESC;

``
