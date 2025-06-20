# Configuração do Ambiente iFood no Databricks

## Tabelas Disponíveis (importadas manualmente via Data Ingestion)

### 1. Tabelas de Pedidos (Divididas em 2 partes)

**Motivo**: O arquivo original era muito grande para upload único

- `order_parte_1`: Primeira parte dos pedidos
- `order_parte_2`: Segunda parte dos pedidos

**Fonte Original**: [order.json.gz](https://data-architect-test-source.s3-sa-east-1.amazonaws.com/order.json.gz)

**Como usar**:
```python
# Para usar os dados completos:
df_orders = spark.table("default.order_parte_1").union(
    spark.table("default.order_parte_2")
)
```

### 2. `consumers` (Usuários)
- **Fonte**: [consumer.csv.gz](https://data-architect-test-source.s3-sa-east-1.amazonaws.com/consumer.csv.gz)

### 3. `restaurants` (Restaurantes)
- **Fonte**: [restaurant.csv.gz](https://data-architect-test-source.s3-sa-east-1.amazonaws.com/restaurant.csv.gz)

### 4. `ab_test_ref` (Teste A/B)
- **Fonte**: [ab_test_ref.tar.gz](https://data-architect-test-source.s3-sa-east-1.amazonaws.com/ab_test_ref.tar.gz)

## Como Verificar as Tabelas

Execute em um notebook:
```python
tabelas_necessarias = [
    "order_parte_1",
    "order_parte_2", 
    "consumer",
    "restaurant",
    "ab_test_ref"
]

for tabela in tabelas_necessarias:
    if spark.catalog.tableExists(f"default.{tabela}"):
        print(f"Tabela {tabela} disponível")
    else:
        print(f"Tabela {tabela} não encontrada")
```

## Observações Importantes

1. Todas as tabelas foram importadas **manualmente** via interface do Databricks
2. Estão disponíveis no schema `default` do Catalog
3. A divisão da tabela de pedidos não afeta a análise, só fazer um union

## Dúvidas Comuns

**Q**: Preciso recriar as tabelas?
**R**: Não, elas já estão criadas via importação manual

**Q**: Por que a tabela de pedidos está dividida?
**R**: Devido ao tamanho do arquivo original
