# Dicas de performance em bancos de dados para iniciantes

Trabalho com bancos de dados há mais de 30 anos e observo que para os iniciantes pode haver muitas dúvidas das escolhas para melhorar o desempenho de bancos de dados relacionais.
Aqui irei comentar algumas dicas genéricas (i.e. se aplicam a diversos SGBD) para incrementar a performance com ideias simples e assertivas. Veja se fazem sentido na sua aplicação e teste:

1) **Campos (colunas) com tipos pequenos** - escolha um tipo de dados que se adequa bem àquilo que irá armazenar. Se um tipo *integer* comporta bem o dado, por que defini-lo como *bigint* ? Pense em escalabilidade (crescimento) e escolha o menor tipo possível.
2) **Tipos numéricos preferencialmente** - os tipos numéricos (*byte*, *integer*...) são geralmente mais rápidos que tipo texto (*char*,*varchar*...). Caso possa utilizá-los, use.
3) **Troque *char* por *varchar* sempre que possível** - o tipo *char* é de tamanho fixo enquanto *varchar* utiliza tamanho variável. Assim, um *char*(50) sempre vai usar 50 bytes enquanto varchar(50) utilizará exatamente o tamanho do dado a ser armazenado. 
Por exemplo, "Fulano" vai ocupar 50 bytes no tipo *char* e apenas 6 bytes no tipo *varchar*(50).
4) **Índices** - toda chave primária de uma tabela deve possuir índice e é meio óbvio. Aqui falo de índices em campos não chave que são muito usados na aplicação. 
Ex: caso a aplicação tenha consulta por nome, é boa prática colocar também um índice nesse campo. Observe quais consultas estão demorando mais e o que elas tem nas 
condições (o famoso where). Um cuidado a se tomar é que índices ocupam espaço e devem ser criados somente se houver uma real necessidade. 
Um campo que é utilizado raramente em consultas talvez não necessite de um índice específico.
5) **Consultas ou atualização por bloco** - ao contrário do que o senso comum pode pensar, um *update* que atualiza 1000 registros de uma vez é mais eficiente que do que fazer um loop com 1 *update*, 1000 vezes na aplicação. O tempo gasto na segunda alternativa pode ser até 10x maior.
   Por último, alguns SGBDs vão deixando "espaços não usados" perdidos quando há muitas atualizações (isso visa garantir a otimização) e devem de tempos em tempos ser "ajustados", preferencialmente fora do horário de uso mais intenso da aplicação. 
   No caso do PostgreSQL, configure o *Autovacuum* ou mesmo um *Vacuum full* para executar semanalmente ou mesmo diariamente, dependendo do uso da aplicação.
   
   Sei que é um artigo pequeno mas teria me ajudado muito quando comecei lá pelos anos 90. Espero que gostem.
