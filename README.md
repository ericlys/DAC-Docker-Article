Atividade com Docker - respostas

Qual a diferença entre image e container?

Um container é, um agrupamento de aplicações juntas com suas dependências, que por sua vez, compartilham  o Kernel do sistema operacional do host, ou seja, da máquina onde está rodando, seja ela física ou ou virtual. 
Um container é bem similar a uma máquina virtual, entretanto, são bem mais leves e mais integradas ao sistema operacional da máquina host, uma vez que o mesmo compartilha o seu kernel, o que proporciona um melhor desempenho por conta do gerenciamento único de recursos.
 Uma das características importantes dos containers é,  eles roda de maneira totalmente isolada, com isso, proporcionando uma maior segurança uma vez que, todo o contexto do container é retido dentro do próprio container, enquanto a máquina física, ou seja, a máquina host, mantém o seu próprio contexto individualmente.      

Uma imagem é composta por sistemas de arquivos de camadas uma sobre a outra. Ela é a base para a construção de uma aplicação. Em uma Imagem temos o que chamamos de base que é um sistema de arquivos de inicialização bootfs, que é muito parecido com o sistema de boot do Linux / Unix. 
Nessa parte é onde temos toda a criação dos cgroups para fazer todo o controle e limitações de processos, namespace para blindar o container. O Docker nunca irá interagir com o sistema de arquivos de inicialização, pois quando o recipiente é iniciado ele é movido para a memória e o boot do sistema de arquivos é desmontado para liberar a memória(RAM) usada pela imagem de disco initrd. Isso parece muito com a pilha de virtualização típica do Linux.
 A próxima camada de uma imagem é a raiz na qual trabalha o rootfs, na parte superior do sistema de arquivos de inicialização, este rootfs pode ser um ou mais sistemas operacionais (Ex: Ubuntu, CentOS).
No momento do boot o sistema de arquivos raiz é montado (somente leitura) e depois de uma checagem de integridade é alterado para (leitura e gravação).

Logo, Uma imagem é um pacote leve e executável que engloba tudo que é preciso para executar um pedaço da aplicação, incluindo código, bibliotecas, variáveis de ambiente, arquivos de configuração e um ambiente de execução, já um container é uma instância desta imagem, isto é, dada a configuração da imagem, é o que todo o conjunto anterior se torna em memória quando é de fato executado. 

Qual a diferença entre os comandos COPY, EXPOSE e ADD?
R: COPY e ADD são ambas Dockerfile instruções que servem para propósitos semelhantes. Eles permitem copiar arquivos de um local específico para uma imagem do Docker.

COPY leva em um src e destino . Ele só permite copiar em um arquivo ou diretório local de seu host (a máquina que constrói a imagem do Docker) para a própria imagem do Docker.

ADD permite que você faça isso também, mas também suporta duas outras fontes. Primeiro, você pode usar um URL em vez de um arquivo / diretório local. Em segundo lugar, você pode extrair um arquivo tar da origem diretamente no destino.

EXPOSE informa ao Docker que o contêiner escuta nas portas de rede especificadas no tempo de execução. Você pode especificar se a porta escuta em TCP ou UDP, e o padrão é TCP se o protocolo não for especificado.
A EXPOSE instrução não publica realmente a porta. se houver portas definidas em EXPOSE, você poderá usar “docker run -P” e mapear automaticamente as portas listadas na imagem.

Qual a diferença entre os comandos RUN, CMD e ENTRYPOINT?
R: Vejamos primeiramente a definição de cada um deles:
Comando
Descrição
Sintaxe
RUN
RUN permite que você instale seu aplicativo e os pacotes necessários para isso. Ele executa qualquer comando sobre a imagem atual e cria uma nova camada, confirmando os resultados.
docker run [OPÇÕES] IMAGE [COMANDO] [ARGUMENTOS...]
CMD
Coloca como DEFAULT comandos ou parâmetros que podem ser sobrescritos na linha de comando quando o container estiver em execução.
CMD
[COMANDO...]
ENTRYPOINT
Configura o container que rodará como um executável, ele é parecido com o comando CMD.
ENTRYPOINT
[ARGUMENTO…]

Todos os três comandos acima podem ser executados na estratégia de SHELL ou na de EXEC. Enquanto que o run instala os pacotes necessários, o docker ignora as múltiplas instruções cmd executando apenas as últimas; já o entrypoint não são ignorados pelo docker mesmo que haja múltiplas instâncias dele.

Qual a diferença entre as estratégias de shell e exec?
R: A estratégia de SHELL nada mais é do que uma instrução seguida de um comando.
Sintaxe: [instrução] [comando]
Exemplo: CMD echo "Hello world"

A estratégia de EXEC, geralmente, utilizada com mais frequência nas instruções CMD e ENTRYPOINT. Ela faz uso de uma instrução juntamente com os seus parâmetros.
Sintaxe: [INSTRUÇÃO] [EXECUTÁVEL, PAR METRO1, PAR METRO2,...]
Exemplo: ENTRYPOINT ["/bin/echo", "Hello world"]

Vale notar que, a diferença de sintaxe entre essas duas estratégias é clara. Enquanto que a de shell é uma forma mais direta, a exec permite o uso de parâmetros.

Qual a diferença entre os comandos docker stop <container_id> e docker kill <container_id>?
R:
Docker stop : Para um contêiner em execução ( envie SIGTERM e depois SIGKILL após o período de tolerância ). O processo principal dentro do contêiner receberá SIGTERM e após um período de tolerância, SIGKILL.

killer kill : Mata um container em execução ( envia um SIGKILL, ou sinal especificado ). O processo principal dentro do container será enviado SIGKILL, ou qualquer sinal especificado com a opção --signal. 

Em ambos os casos, as alterações no sistema de arquivos serão mantidas (no momento de parar ou matar), portanto, se você executar o docker start <container> continuará, a partir de onde parou.


Referências:
https://nickjanetakis.com/blog/docker-tip-2-the-difference-between-copy-and-add-in-a-dockerile
https://docs.docker.com/engine/reference/builder/#expose
https://docs.docker.com/engine/reference/commandline/run/
https://cursos.alura.com.br/forum/topico-qual-e-diferenca-de-container-e-image
https://books.google.com.br/books?hl=pt-BR&lr=&id=6xnkDAAAQBAJ&oi=fnd&pg=PA1&dq=O+que+%C3%A9+uma+imagem+no+docker&ots=dwLrz66vzz&sig=k9nbxWSqlrbPA0tMs333OVzaZPg#v=onepage&q=O%20que%20%C3%A9%20uma%20imagem%20no%20docker&f=false
m-47761
http://goinbigdata.com/docker-run-vs-cmd-vs-entrypoint/
https://www.mundodocker.com.br/o-que-e-uma-imagem/
https://superuser.com/questions/756999/whats-the-difference-between-docker-stop-and-docker-kill

