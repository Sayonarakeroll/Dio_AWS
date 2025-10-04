
# Computação em Nuvem com AWS

Documentando um pouco sobre o conteúdo abordado nas aulas...


### 1\. Modelos de Computação em Nuvem

O ponto de partida foi a compreensão da divisão de responsabilidades em cada modelo de serviço:

  * **IaaS (Infrastructure as a Service):** Oferece controle total sobre a infraestrutura virtual (servidores, redes, armazenamento). É o modelo ideal para migrar sistemas legados ou para cenários que exigem alta customização, com o provedor de nuvem gerenciando apenas a parte física.
  * **PaaS (Platform as a Service):** Abstrai a infraestrutura, fornecendo uma plataforma pronta para desenvolver, testar e implantar aplicações. Perfeito para equipes de desenvolvimento que querem focar no código, sem se preocupar com o sistema operacional ou o servidor web.
  * **SaaS (Software as a Service):** Consiste em um software pronto para uso, entregue como um serviço. É o caso de aplicações como e-mail (Gmail), CRM ou ERP, onde o fornecedor gerencia absolutamente tudo.

### 2\. Infraestrutura Global da AWS

Um ponto crucial foi o entendimento da estrutura física da AWS, projetada para oferecer alta disponibilidade e baixa latência:

  * **Regiões (Regions):** São áreas geográficas distintas ao redor do mundo. A escolha de uma região é uma decisão estratégica que deve levar em conta fatores como **compliance** (soberania de dados), **disponibilidade de serviços**, **custo** e **latência** para os usuários finais.
  * **Zonas de Disponibilidade (Availability Zones - AZs):** Cada Região é composta por múltiplas AZs, que são data centers fisicamente isolados, mas conectados por redes de altíssima velocidade. A utilização de múltiplas AZs é a chave para a criação de aplicações tolerantes a falhas.

### 3\. Segurança e Gerenciamento de Identidade

A segurança foi apresentada como a "prioridade zero", com foco no serviço **IAM (Identity and Access Management)**.

  * **Controle de Acesso:** O IAM reforça a importância de não utilizar o usuário-raiz (root) para tarefas diárias. A prática correta é criar **usuários**, agrupá-los em **grupos** (ex: "Desenvolvedores", "Analistas") e anexar **políticas de permissões**, garantindo que cada identidade tenha acesso apenas aos recursos estritamente necessários (Princípio do Menor Privilégio).
  * **Formas de Acesso:** A interação com a AWS pode ser realizada de diferentes formas: via **Console** (interface gráfica), **AWS CLI** (linha de comando) ou **CloudShell**.

### 4\. Computação e Armazenamento

Esta seção abordou os "blocos de construção" essenciais para qualquer aplicação na nuvem.

#### **Amazon EC2 (Elastic Compute Cloud)**

  * **Conceito:** São as máquinas virtuais da AWS, onde é possível configurar CPU, memória, disco (armazenamento) e rede, com sistemas operacionais Windows ou Linux.
  * **Escalabilidade:** Foi apresentada a diferença entre **escalabilidade vertical** (aumentar a potência de uma única instância, ex: mais CPU/memória) e **escalabilidade horizontal** (aumentar o número de instâncias para distribuir a carga).
  * **Modelos de Compra:**
      * **On-Demand:** Pagamento por hora, ideal para cargas de trabalho imprevisíveis e de curta duração.
      * **Reservadas:** Descontos significativos mediante o compromisso de uso por 1 ou 3 anos.
      * **Spot:** Modelo mais econômico que aproveita a capacidade ociosa da AWS, mas com o risco de interrupção da instância.

#### **Amazon EBS (Elastic Block Store)**

  * **Conceito:** Volumes de armazenamento em bloco de alto desempenho, que funcionam como "HDs externos" para as instâncias EC2.
  * **Disponibilidade vs. Durabilidade:** Uma distinção importante é que o EBS foca em **disponibilidade** (99,999%), garantindo que o serviço esteja operacional.
  * **Snapshots:** São cópias de um volume EBS em um determinado momento ("fotos"), armazenadas no Amazon S3. São fundamentais para backups, recuperação de desastres e criação de novos volumes.

#### **Amazon S3 (Simple Storage Service)**

  * **Conceito:** Serviço de armazenamento de **objetos**, projetado para guardar grandes volumes de dados (imagens, vídeos, backups, logs) de forma segura e altamente escalável.
  * **Durabilidade:** O S3 é projetado para uma **durabilidade** de 99,999999999% (onze noves), oferecendo uma proteção extremamente alta contra a perda de dados.
  * **Ciclo de Vida (Lifecycle):** Este recurso permite automatizar a migração de objetos para classes de armazenamento mais baratas (como S3 Glacier) à medida que se tornam menos acessados, gerando uma otimização significativa de custos.

-----

## 🛠️ Exemplo de Arquitetura Desenhada

A seguir um desenho do fluxo simples de uma aplicação que recebe um arquivo do usuário. Esta arquitetura demonstra como os serviços estudados se conectam.

### Explicação do Fluxo:

1.  **O Usuário (Actor):** O processo inicia com um usuário enviando um arquivo a partir de sua estação de trabalho.
2.  **A Aplicação (Instância EC2):** O arquivo é recebido por uma **instância EC2** , que atua como o cérebro da aplicação, processando a requisição.
3.  **Armazenamento em Bloco (EBS):** A instância EC2 está conectada a dois volumes **EBS** . Um poderia ser o volume raiz (para o sistema operacional) e o outro um volume de dados para processamento temporário do arquivo.
4.  **Banco de Dados (Amazon RDS):** A aplicação se comunica com o **Amazon RDS** . Essa comunicação pode ser para gravar metadados sobre o arquivo ou para consultar informações do usuário.
5.  **Visão Geral (IaaS):** Esta arquitetura é um exemplo clássico do modelo **IaaS**, onde serviços de computação (EC2), armazenamento (EBS) e banco de dados (RDS) são combinados para construir uma solução completa na nuvem da AWS.
