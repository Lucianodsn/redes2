# Descrição e Objetivo

Este trabalho consistirá em uma análise comparativa entre os métodos de checksum e CRC para verificação de erros em mensagens. A ideia é implementar ambos os métodos na forma de programas e utilizá-los para simular a verificação de uma grande quantidade de mensagens corrompidas. Com base nestas execuções, o grupo deve comparar checksum e CRC em termos de tempo de execução e capacidade de detecção de erros.

### Mais especificamente, este trabalho se dividirá em uma série de etapas. A saber:
- Implementar uma função que calcule o checksum de uma mensagem.
- Implementar uma função que calcule o CRC de uma mensagem (parametrizável em relação ao polinômio gerador utilizado).
- Implementar uma função que gere mensagens aleatórias.
- Implementar uma função que insira erros aleatórios em uma mensagem.
- Implementar um programa simulador que gere um grande número de mensagens aleatórias, calcule seu checksum/CRC, insira erros e realize a verificação da mensagem corrompida.
- Escrever relatório reportando a metodologia e as conclusões obtidas.
As seções a seguir detalham cada um destes pontos.

# Cálculo do Checksum

Para efeito deste trabalho, deverá ser implementada uma versão ligeiramente modificada do chamado Internet Checksum estudado durante as aulas da disciplina. De fato, a única diferença entre o método descrito durante as aulas e aquele implementado neste trabalho é o número de bits: 8, ao invés dos 16 utilizados por padrão.

###### O algoritmo de checksum a ser implementado, portanto, consiste dos seguintes passos:
- A mensagem deve ser tratada como uma sequência de números de 8 bits (1 byte).
- Estes números devem ser somados em complemento a 1, i.e., a cada parcela da soma, se houve overflow, o resultado deve ser incrementado em uma unidade.
- Ao final da soma de todos os bytes, o resultado deve ser invertido bit a bit.
- Este último valor é o checksum da mensagem.

A função de checksum a ser implementada deve receber como argumentos a mensagem e seu tamanho. Como resultado, a função deverá retornar o valor do checksum conforme descrito acima.
# Cálculo do CRC

A implementação do cálculo do CRC deverá ser realizada de acordo com o algoritmo estudado em sala de aula. Para efeito deste trabalho, a implementação deverá ser parametrizada pelo polinômio gerador, i.e., a implementação deverá funcionar com diferentes polinômios geradores especificados como argumento da função (além da mensagem e de seu tamanho). A implementação pode assumir que o polinômio gerador será sempre de grau 8.

Assim como a função de cálculo de checksum, a função de cálculo de CRC deverá retornar os bits de CRC resultantes da computação. Como o polinômio gerador é sempre de grau 8, o CRC resultante sempre terá 8 bits (1 byte).
# Geração de Mensagens Aleatórias

A função de geração de mensagens aleatórias deverá receber como argumento um tamanho, em bytes, da mensagem a ser gerada. A implementação deverá utilizar qualquer função disponível na linguagem de programação escolhida para geração de números pseudo-aleatórios para a obtenção dos valores dos bytes da mensagem resultante. A única restrição é que o tamanho especificado como argumento deverá ser respeitado.

# Inserção de Erros Aleatórios

Esta função deverá receber como argumentos uma mensagem (sequência de bytes), seu tamanho em bytes e uma probabilidade p de corrupção de bits (0 < p ≤ 1). A função deverá, então, percorrer cada bit da mensagem verificando se deve ou não alterar seu valor. Ao final do processamento, a nova mensagem (i.e., a mensagem original com bits potencialmente alterados) deverá ser retornada.

Um ponto importante nesta função para os propósitos deste trabalho é que a mensagem deve ter ao menos um bit alterado. Isto é, se ao final do percorrimento dos bits da mensagem nenhum bit tiver sido alterado, deve-se repetir o processo para garantir que a mensagem final seja diferente da mensagem original.

#### Algumas dicas/sugestões:
- Boa parte das linguagens permite a manipulação de bits individuais em um byte (ou outro tipo inteiro) através de operações bit-a-bit (ou bitwise operations, em inglês). Por exemplo, para alterar o valor i-ésimo bit de um valor inteiro n em C pode-se combinar as operações de ou-exclusivo e shift (note que esta operação serve tanto para alterar o valor do bit de 0 para 1 quanto de 1 para 0):
###### Listagem de código
`` n = n ^ (1 << i); ``

- Note que o valor de cada bit deve ser alterado com probabilidade p. Isso pode ser realizado através da geração pseudo-aleatória de números. Basicamente, sorteia-se um número pseudo-aleatório x entre 0 e 1 (inclusive). Se x ≤ p, inverte-se o valor do bit. Caso contrário, o valor do bit permanece o mesmo.
- É comum que certas linguagens forneçam como função para números pseudo-aleatórios apenas uma função que gere valores inteiros de 0 até algum valor máximo (e não um número em ponto-flutuante entre 0 e 1, como descrito no item anterior). Neste caso, pode-se simplesmente dividir o valor gerado pelo valor máximo possível pela função de geração de números pseudo-aleatórios.
# Programas Simuladores

De posse das funções descritas nas seções anteriores, o grupo deverá escrever dois programas simuladores completos (um para o CRC e outro para o checksum).

O programa simulador para o checksum deverá receber do usuário os seguintes parâmetros:
- Valor inteiro positivo denotando o tamanho, em bytes, dos pacotes a serem gerados aleatoriamente.
- Valor inteiro positivo denotando o número de pacotes aleatórios a serem gerados.
- Valor da probabilidade p (0 < p ≤ 1) de alteração de cada bit da mensagem.
- Valor inteiro a ser utilizado como semente (ou seed) do gerador de números pseudo-aleatórios.

A maneira pela qual estes valores são obtidos do usuário não é importante (e.g., leitura de parâmetros de linha de comando, leitura interativa a partir do teclado, interface gráfica), sendo de livre escolha do grupo.

Para o programa simulador de CRC, os parâmetros a serem obtidos do usuário são os mesmos, acrescidos do polinômio gerador. O polinômio deverá ser informado pelo usuário na forma de um número hexadecimal cuja representação em base 2 corresponda aos coeficientes, sendo os bits mais significativos correspondentes aos coeficientes de maior grau. Por exemplo, o valor hexadecimal 121 corresponde ao valor binário 100100001 que, na notação proposta, denota o polinômio gerador x8 + x5 + 1.

Uma vez lidos os parâmetros passados pelo usuário, o programa deverá realizar os seguintes passos:
- Inicializar o gerador de números pseudo-aleatórios com a semente especificada pelo usuário
- Gere um novo pacote aleatório do tamanho especificado pelo usuário.
- Calcule o checksum/CRC deste novo pacote. No caso do CRC, utilize o polinômio gerador especificado pelo usuário.
- Adicione erros aleatórios no pacote, considerando a probabilidade de corrupção de cada bit igual ao valor de p especificado pelo usuário.
- Calcule o checksum/CRC do pacote corrompido.
- Compare o valor do checksum/CRC do pacote corrompido ao do pacote original. Se os valores forem iguais, incremente um contador de colisões.
- Se o número de pacotes aleatórios já gerado for menor que o especificado pelo usuário, volte ao passo 2.
- Imprima o percentual de colisões obtidas, i.e., o valor do contador de colisões dividido pelo número de pacotes especificado pelo usuário.

A saída dos programas simuladores, portanto, deve ser simplesmente o número de colisões obtidas (i.e., o número de pacotes para os quais o checksum/CRC não foi alterado mesmo após a inserção de erros aleatórios).

Note que geradores de números pseudo-aleatórios recebem como argumento de inicialização uma semente (ou seed, em inglês). Se o mesmo gerador é inicializado com a mesma semente em duas execuções diferentes, a sequência de valores gerados será idêntica. Fazendo com que a semente seja obtida como parâmetro do usuário, permite-se testar o checksum e os CRCs exatamente nas mesmas condições (i.e., os mesmos pacotes serão gerados e as mesmas corrupções serão adicionadas).

# Comparações e Relatório

Além da parte de implementação, este trabalho também inclui uma breve comparação entre os métodos de checksum e CRC. Para tanto, o grupo deverá realizar diversas execuções dos programas simuladores descritos na seção anterior, coletando estatísticas de colisão. Estas estatísticas deverão ser apresentadas em um relatório a ser entregue juntamente com as implementações.

Mais especificamente, pede-se que o grupo:
- Escolha ao menos 5 valores de sementes a serem utilizadas em execuções dos programas simuladores.
- Escolha ao menos 3 valores de tamanho de pacotes entre 100 e 1500 bytes a serem utilizados em execuções dos programas simuladores.
- Escolha ao menos 3 valores de p a serem utilizados em execuções dos programas simuladores.
- Execute o programa simulador para o checksum com cada uma das combinações dos valores de p, tamanho de pacote e semente escolhidos nos itens 1-3, armazenando os percentuais de colisão obtidos e o tempo de execução.
- Escolha ao menos dois polinômios padronizados listados em neste link, além de outro polinômio não presente nesta lista criado arbitrariamente pelo grupo.
- Execute o programa simulador para o CRC com cada uma das combinações dos valores de p, tamanho de pacote, semente e polinômio gerador escolhidos nos itens 1-3 e 5, armazenando os percentuais de colisão obtidos e o tempo de execução.
-
O número de pacotes a serem gerados em cada execução dos programas simuladores é de livre escolha do grupo, mas sugere-se a utilização de ao menos 10000 pacotes por execução. Ademais, este valor deverá ser igual para todas as execuções.

O relatório deverá listar as escolhas realizadas pelo grupo para os itens 1-3 e 5 da lista acima, além do número de pacotes gerados em cada execução. Além disso, o relatório deverá apresentar, na forma de gráficos e/ou tabelas, os percentuais de colisão obtidos nas execuções e os tempos de execução. Para cada combinação de p, tamanho de pacote, método de detecção de erro/polinômio gerador, sugere-se tirar a média dos valores obtidos com as várias sementes.

Além de apresentar os dados supracitados, o relatório deverá possuir uma parte de análise, na qual o grupo deverá apresentar conclusões acerca dos experimentos realizados. Em particular, espera-se que o relatório analise os seguintes aspectos:
- Qual foi o melhor método de detecção? CRC ou checksum?
- Para o CRC, fez diferença a escolha do polinômio gerador?
- O polinômio escolhido arbitrariamente pelo grupo obteve bons resultados, em comparação aos polinômios padronizados?
- Houve diferença perceptível entre os tempos de execução? Neste caso, qual método foi mais rápido?
- Houve diferença perceptível entre os tempos de execução para diferentes valores de polinômio?
- A escolha de p influenciou a capacidade de detecção dos métodos avaliados? De que forma?

# Requisitos e Restrições

A parte de implementação do trabalho pode ser feita em qualquer linguagem, desde que observadas as seguintes restrições:
- Os códigos de geração de pacotes, inserção de erros, cálculo de checksum/CRC deverão ser de autoria dos membros do grupo, sem que se recorra à trechos de códigos de terceiros ou bibliotecas que implementem total ou parcialmente as funcionalidades aqui listadas (funções/métodos de geração de números aleatórios e outras manipulações básicas da linguagem podem ser utilizadas).
- O código final deve ser compilável/executável sem a necessidade de bibliotecas, frameworks ou ferramentas pagas.

De maneira análoga, o relatório é de formato livre, sem limites inferiores ou superiores de páginas. Apenas como um guia geral, 5 páginas devem ser suficientes (embora não necessárias) para contemplar todos os itens listados na seção anterior.

Os grupos poderão ser formados com no mínimo 2 e no máximo 5 alunos.

# Apresentação

Como parte da avaliação, o grupo deverá realizar uma apresentação curta do trabalho desenvolvido para o restante da turma durante o horário da aula em data a ser combinada com o professor posteriormente. A apresentação deverá ser realizada por todos os integrantes e contemplar os seguintes itens:
- Visão geral do funcionamento do código.
- Exemplo de execução do código.
- Parâmetros selecionados para o experimento.
- Resultados encontrados e conclusões.

O tempo máximo de apresentação será de 15 minutos. A apresentação poderá ser baseada em slides ou não, de acordo com decisão de cada grupo.
# Critério de Avaliação

Cada trabalho receberá uma nota variando de 0 a 10. Esta pontuação será dividida nas seguintes proporções:
- Até 5 pontos para a implementação dos programas simuladores, dos quais:
  - Até 1,0 pontos para a implementação correta da função que computa CRC.
  - Até 1,0 pontos para a implementação correta da função que calcula checksum.
  - Até 0,5 ponto para a implementação correta da função que gera pacotes aleatoriamente.
  - Até 0,5 ponto para a implementação correta da função que insere erros aleatórios nos pacotes.
  - Até 1,0 ponto para a implementação correta dos programas simuladores.
  - Até 1,0 ponto para a existência e qualidade da documentação das implementações.
- Até 3 pontos para o relatório, dos quais:
  - Até 0,5 ponto para a descrição completa dos parâmetros escolhidos para a avaliação.
  - Até 1,0 ponto para a apresentação dos resultados obtidos para as métricas de colisão e tempo de execução.
  - Até 1,5 pontos para a análise dos resultados e conclusões.
- Até 2 pontos para a apresentação.

As implementações serão avaliadas apenas em relação à correção do código e aderência a esta especificação. Não haverá pontuação associada ao desempenho/eficiência da implementação.
# Entrega

A data limite para a entrega do trabalho está disponível no calendário da página da disciplina. A entrega deverá ser realizada por e-mail, através do endereço dpassos@ic.uff.br. O e-mail deverá conter:
- Identificador do trabalho (e.g., “Trabalho de Redes II”).
- Lista dos integrantes do grupo.
- Código fonte da implementação.
- Instruções de compilação/execução/uso da implementação.
- Relatório.

Serão aceitos, sem penalidade, e-mails enviados até as 19:59 da data limite (i.e., até o horário da aula). Os e-mails de entrega de trabalho terão seus recebimentos devidamente confirmados. É responsabilidade do grupo garantir que o trabalho seja recebido, aguardando pela confirmação e reenviando a mensagem caso não a recebam em tempo razoável. Em caso de dúvidas ou correções relacionadas a esta especificação, também é responsabilidade de cada grupo entrar em contato (seja pessoalmente, ou através do mesmo endereço de e-mail) requisitando esclarecimentos dentro do prazo de entrega do trabalho.

Uma vez entregue o trabalho, não serão aceitas alterações (nem inclusões, nem remoções) na lista de integrantes do grupo em nenhuma hipótese. Por isso, sugere-se atenção no momento do envio da mensagem para que a lista contenha todos os integrantes do grupo.
