# Avaliação de teste A/B — sistema de recomendação

Análise de um experimento de uma loja online para avaliar uma alteração no sistema de recomendação. O projeto examina a execução do teste, compara conversões entre os grupos e apresenta uma análise complementar do funil sequencial.

**Conclusão:** os dados analisados não sustentam recomendar a implementação com base no benefício esperado. As limitações do experimento impedem atribuir as diferenças observadas exclusivamente à alteração testada.

[Consultar o notebook completo](analise_ab.ipynb)

## Objetivo

Avaliar o teste `recommender_system_test`, com grupo A de controle e grupo B de tratamento.

O planejamento previa 6.000 participantes, público formado por 15% dos novos usuários da União Europeia e aumento relativo de pelo menos 10% nas conversões indicadas, em até 14 dias após o cadastro.

## Dados

Foram utilizados quatro arquivos:

| Arquivo | Conteúdo |
|---|---|
| `ab_project_marketing_events_us.csv` | Campanhas de marketing, regiões e períodos |
| `final_ab_new_users_upd_us.csv` | Cadastros, regiões e dispositivos |
| `final_ab_events_upd_us.csv` | Eventos dos usuários |
| `final_ab_participants_upd_us.csv` | Participantes, testes e grupos |

Os arquivos de origem são preservados. Os filtros e as transformações são aplicados em cópias de trabalho.

## Diagnóstico do experimento

Foram identificadas divergências e limitações relevantes:

- 3.675 participantes, abaixo dos 6.000 esperados.
- 194 participantes de fora da União Europeia.
- 887 participantes vinculados também a outro teste.
- Participação de 8,82% dos novos usuários da UE, contra os 15% previstos.
- Eventos disponíveis somente até 30/12/2020, antes do término planejado de 01/01/2021.
- Ausência de eventos em 25/12 na base completa, sugerindo uma possível lacuna nos registros.
- Diferenças na distribuição das datas de cadastro entre os grupos.
- Campanha de Natal coincidente com parte do experimento, sem informação de exposição individual.

Esses pontos limitam a interpretação causal dos resultados.

## População de análise

Foram mantidos participantes:

1. Da União Europeia.
2. Sem vínculo com o outro teste.
3. Cadastrados até 17/12/2020, para que a janela nominal de 14 dias coubesse no período disponível.

Os eventos foram limitados ao dia do cadastro e aos 13 dias seguintes.

| Indicador | Quantidade |
|---|---:|
| Participantes de A | 1.115 |
| Participantes de B | 476 |
| Total de participantes | 1.591 |
| Eventos na janela de análise | 10.201 |

O corte temporal afetou os grupos em proporções diferentes. Além disso, uma janela nominal completa não garante coleta completa dos eventos.

## Conversões por evento

Cada conversão representa os usuários únicos que realizaram o evento divididos pelo total de participantes do grupo, sem exigir passagem pelas etapas anteriores.

| Evento | Grupo A | Grupo B | Variação relativa de B frente a A |
|---|---:|---:|---:|
| Página de produto | 64,84% | 55,67% | −14,14% |
| Carrinho | 31,39% | 29,20% | −6,97% |
| Compra | 29,87% | 27,73% | −7,15% |

A meta de aumento relativo de pelo menos 10% não foi atingida nas estimativas observadas de nenhum evento.

## Avaliação estatística

Foram aplicados testes z bilaterais para duas proporções, com nível de significância global de 5% e correção de Bonferroni para três comparações.

| Evento | Valor-p | Decisão com α ≈ 0,0167 |
|---|---:|---|
| Página de produto | 0,000555 | Rejeitar H₀ |
| Carrinho | 0,386326 | Não rejeitar H₀ |
| Compra | 0,391368 | Não rejeitar H₀ |

Apenas página de produto apresentou diferença estatisticamente significativa, com conversão observada menor em B.

Não rejeitar H₀ para carrinho e compra não comprova igualdade ou ausência de efeito. Os testes também não avaliam diretamente a meta de aumento relativo de 10%.

## Análise complementar — funil sequencial

O funil acompanha os mesmos usuários pela sequência página → carrinho → compra, exigindo horários estritamente posteriores dentro da janela individual de análise.

| Etapa ou conversão | Grupo A | Grupo B |
|---|---:|---:|
| Usuários com página de produto | 723 | 265 |
| Usuários com carrinho posterior à página | 207 | 62 |
| Usuários com compra posterior ao carrinho elegível | 49 | 16 |
| Página → carrinho | 28,63% | 23,40% |
| Carrinho → compra | 23,67% | 25,81% |

A transição de página para carrinho apresentou a maior perda proporcional observada nos dois grupos e merece investigação.

A comparação é descritiva. Os usuários que chegam ao carrinho constituem grupos selecionados pelo próprio avanço no funil; a conversão seguinte não demonstra vantagem causal de B.

Compras sem a sequência completa registrada permanecem na análise por evento, mas não integram o funil completo. Horários iguais não permitem confirmar a ordem, e a ausência de uma etapa registrada não comprova abandono.

## Recomendação

Investigar as divergências de recrutamento, alocação e coleta antes de realizar um novo experimento, com público elegível, acompanhamento completo e planejamento amostral adequado.

Os resultados se aplicam à população selecionada e aos registros fornecidos, sem generalização automática para todos os usuários da loja.

## Como executar

1. Baixe o projeto, mantendo os quatro CSVs na mesma pasta de `analise_ab.ipynb`.
2. Abra essa pasta no VS Code com as extensões Python e Jupyter.
3. Instale as dependências no ambiente escolhido:

   ```bash
   python -m pip install -r requirements.txt
   ```

4. Abra o notebook e selecione o kernel desse ambiente.
5. Reinicie o kernel e execute todas as células na ordem.

## Tecnologias

Python · Pandas · NumPy · Matplotlib · SciPy · Jupyter Notebook

## Autoria

Denise Duarte  