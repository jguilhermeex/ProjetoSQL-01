-- (Query 1) Receita, leads, conversão e ticket médio mês a mês
-- Colunas: mês, leads (#), vendas (#), receita (k, R$), conversão (%), ticket médio (k, R$)
-- mes e lead

WITH leads as (

SELECT 
date_trunc ('month', visit_page_date)::date as visit_page_month,
COUNT (*) as contagem
FROM sales.funnel
GROUP BY visit_page_month
ORDER BY 1 
    ),
payments as (

--vendas e receita

SELECT 
        date_trunc('month',f.paid_date)::date as paid_month,
        count(f.paid_date) as paid_count,
        Sum (p.price * (1+f.discount)) as receita
FROM sales.funnel as f
LEFT JOIN sales.products as p
ON f.product_id = p.product_id
WHERE f.paid_date is not null
GROUP BY paid_month
ORDER BY paid_month
)

SELECT 

    l.visit_page_month as mes,
    l.contagem as leads,
    p.paid_count as vendas,
    (p.receita/1000) as receita,
    (p.paid_count::float / l.contagem::float) as conversao,
    (p.receita / p.paid_count/1000) as ticket_medio
   
    
FROM leads as l
LEFT JOIN payments as p
ON l.visit_page_month = p.paid_month








-- (Query 2) Estados que mais venderam
-- Colunas: país, estado, vendas (#)
SELECT 
    'Brasil' as país,
    c.state as estado,
    count (f.paid_date) as vendas

FROM sales.funnel as f
LEFT JOIN sales.customers as c
ON f.customer_id = c.customer_id
WHERE f.paid_date between '2021-08-01' AND '2021-08-31'
GROUP BY país,estado
ORDER BY vendas DESC
LIMIT 5

-- (Query 3) Marcas que mais venderam no mês
-- Colunas: marca, vendas (#)

SELECT p.brand, count (f.paid_date) as vendas
FROM sales.funnel as f
LEFT JOIN sales.products as p
ON f.product_id = p.product_id
GROUP BY p.brand
ORDER BY vendas DESC

-- (Query 4) Lojas que mais venderam
-- Colunas: loja, vendas (#)
SELECT s.store_name, count (f.paid_date) as vendas
FROM sales.stores as s
LEFT JOIN sales.funnel as f
ON s.store_id = f.store_id
WHERE f.paid_date is not null
GROUP BY s.store_name
ORDER BY vendas DESC


-- (Query 5) Dias da semana com maior número de visitas ao site
-- Colunas: dia_semana, dia da semana, visitas (#)

SELECT
EXTRACT ('dow' from visit_page_date) as dia_semana,
CASE
    WHEN EXTRACT ('dow' from visit_page_date) = 0 Then 'Domingo'
        WHEN EXTRACT ('dow' from visit_page_date) = 1 Then 'Segunda'
            WHEN EXTRACT ('dow' from visit_page_date) = 2 Then 'Terça'
                WHEN EXTRACT ('dow' from visit_page_date) = 3 Then 'Quarta'
                    WHEN EXTRACT ('dow' from visit_page_date) = 4 Then 'Quinta'
                        WHEN EXTRACT ('dow' from visit_page_date) = 5Then 'Sexta'
    Else 'Sábado'
    END AS nome_dia,
    Count (*) as visitas

FROM sales.funnel
WHERE visit_page_date between '2021-08-01' AND '2021-08-31'
GROUP BY dia_semana
ORDER BY dia_semana

--FIM



