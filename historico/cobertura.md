# Cobertura de validação

**H218/v226 está integrada e salva**, SHA `521f5ce8d4fa0bad4a0a02b77bec8968eb0af3b6ec04e260136e476e9707d379`. O prefixo132 permanece literal e as 42 cápsulas foram decodificadas com seu helper real. Passaram 49 controles simbólicos do fluxo, 20 recusas do verificador numérico CPU, três interrupções por prazo e o controle direcionado do sufixo integral. **Precisão, captura, progresso e tempo H200 continuam pendentes.**

**97 avaliações oficiais de 94 códigos distintos**, sendo 94 checks e três pontuadas; 86 códigos com avaliação integral. Acervo: 231 experimentos, 237 fontes primárias e 140 entradas AOT. Nenhuma avaliação GPU ou AOT nova nesta integração. Última API: zero checks e dez pontuadas, 35º de 1.154, etapa aberta e sem jobs pendentes. Publicação v132 intacta em **1,540954514×**. Nenhum check ou publicação está selecionado automaticamente.

H218 substitui somente a cuBLASLt de unpoolerM256 quando habilitada; preserva as outras quatro Lt ativas. As cinco implementações parentais continuam literais como controle/fallback. Mantendo as outras 29 latências publicadas, o teto matemático dessa rota isolada seria 1,628894× mesmo com custo hipotético zero, abaixo do líder arredondado de 1,6367×. A liderança exige outras rotas.

[Candidata](experimentos/compartilhamento_w_h218_v226.py); [Estado e próxima ação](resultados/estado_integracao_compartilhada_h218_v226_20261027.json); [Protocolo](resultados/protocolo_compartilhamento_w_h218_v226_20261027.json); [Rotas e alcance](resultados/adendo_rotas_e_alcance_h218_v226_20261027.json).

A reserva H218 está pronta localmente para seleção experimental, sem ganho demonstrado. Antes de um check, consultar abertura, cotas e jobs e reavaliar todas as evidências disponíveis. Confirmar o perfil `h218::compartilhar_w`, a correção e a medição oficial completa. Não usar pontuada como check nem modificar a publicação por evidência CPU/AOT.

Sem checks, continuar investigação de outras cargas, com até duas hipóteses novas fundamentadas de cada vez. H217 permanece reserva CPU; H215 regressiva e H216 full-K permanecem arquivadas, sem repetição automática. O teto por rota é condicional às outras latências do job132; não prevê a velocidade da H218.

- [x] Integrar H218 ao prefixo132 e conferir fontes, aliases, protocolo e controles locais.
- [ ] Selecionar e executar check integral H200 com cota disponível.
- [ ] Investigar mecanismos de ganho também nas outras cargas.
- [ ] Publicar somente a melhor combinação integralmente validada, com ganho além da incerteza.
- [ ] Alcançar top 5, primeiro lugar e mantê-lo enquanto a etapa estiver aberta.

[Estado anterior preservado](COBERTURA_antes_integracao_h218_v226_20261027.md).
