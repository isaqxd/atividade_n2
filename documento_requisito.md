# PandasVisor
Documento de Requisitos de Software v2.0

## INTRODUÇÃO
Este documento define e consolida os requisitos do sistema PandasVisor – Aplicativo de Conferência de Compras por Imagem, agora com suporte à inteligência artificial para sugestão de dados. Ele serve de base para as fases de projeto, desenvolvimento, testes e manutenção.

### Definições, acrônimos e abreviações
- React Native – Framework para desenvolvimento mobile multiplataforma. 
- Firebase – Plataforma do Google (Authentication, Firestore, Storage). 
- Gemini API – API de IA generativa do Google (modelo gemini-2.5-flash ou similar) utilizada para análise de imagem e extração de dados estruturados. 
- JSON – Formato de intercâmbio de dados usado na comunicação com a Gemini. 
- OCR – Optical Character Recognition (reconhecimento óptico de caracteres), funcionalidade implícita no modelo multimodal. 
- MVP – Minimum Viable Product.
### Metodologia de elicitação
O levantamento foi realizado a partir do Documento de Visão e de workshops com o time, detalhando as jornadas de usuário em passos atômicos. Utilizou-se a técnica de User Story Mapping para decompor as funcionalidades macro em entregas de valor incremental. A integração com IA foi incorporada como uma funcionalidade SHOULD para o MVP, visando acelerar o preenchimento da lista.

### Documento de Visão
Documento de Visão – PandasVisor v1.0, maio de 2026 (disponível no repositório do projeto).

## Visão geral dos requisitos funcionais
| ID    | User Story                                                                  | Valor de Negócio                 | MoSCoW | Prioridade | Pontos | Dependências |
|-------|-----------------------------------------------------------------------------|----------------------------------|--------|------------|--------|--------------|
| EP-01 | **Autenticação e Acesso**                                                   |                                  |        |            | 5      |              |
| US-01 | Fazer login com e-mail e senha                                              | Acesso seguro e personalizado    | MUST   | Alta       | 2      | –            |
| US-02 | Recuperar senha via e-mail                                                  | Autonomia do usuário             | SHOULD | Média      | 1      | US-01        |
| US-03 | Manter sessão ativa no dispositivo                                          | Fluidez no uso diário            | SHOULD | Média      | 2      | US-01        |
| EP-02 | **Captura e Tratamento da Imagem**                                          |                                  |        |            |        |              |
| US-04 | Fotografar produto com câmera, pré-visualizar e refazer se necessário       | Registro visual de qualidade     | MUST   | Alta       | 3      | US-01        |
| US-05 | Processar imagem (redimensionar e comprimir) e fazer upload para Storage    | Desempenho e persistência        | MUST   | Alta       | 2      | US-04        |
| EP-03 | **Sugestão Inteligente com Gemini**                                         |                                  |        |            |        |              |
| US-06 | Enviar imagem para Gemini API e receber sugestão de nome e preço do produto | Agilidade e redução de digitação | SHOULD | Média      | 5      | US-05        |
| US-07 | Visualizar, editar e confirmar sugestão do Gemini no formulário             | Flexibilidade e precisão         | SHOULD | Média      | 3      | US-06        |
| EP-04 | **Gerenciamento da Lista de Compras                                         |                                  |        |            |        |              |
| US-08 | Adicionar item à lista (com imagem, nome, valor unitário e quantidade)      | Construção da lista              | MUST   | Alta       | 3      | US-05        |
| US-09 | Visualizar lista com totais por item e total geral da compra                | Conferência rápida no caixa      | MUST   | Alta       | 3      | US-08        |
| US-10 | Alterar quantidade de um item na lista                                      | Correção ágil durante a compra   | SHOULD | Média      | 2      | US-09        |
| US-11 | Excluir item da lista com confirmação                                       | Gestão completa da lista         | SHOULD | Média      | 2      | US-09        |

## Especificação dos requisitos funcionais

### EP-01: Autenticação e Acesso
**US-01 — Fazer login com e-mail e senha**
**Detalhes:** Como usuário, quero fazer login com meu e-mail e senha para acessar minha lista de compras de forma segura e personalizada.

**Critérios de aceite:**
```gherkin
Cenario: Fazer Login - Sucesso
    Dado que estou na tela de login
    Quando insiro e-mail e senha válidos e pressiono "Entrar"
    Então sou autenticado e redirecionado para a tela principal (minha lista).

Cenario: Fazer Login - Falha 
    Dado que insiro credenciais inválidas
    Quando pressiono "Entrar"
    Então vejo a mensagem "E-mail ou senha inválidos" e permaneço na tela de login.

Cenario: Fazer Login - Sessão expirada
    Dado que tento acessar qualquer tela interna sem estar logado
    Quando o app é iniciado
    Então sou redirecionado para a tela de login.
    
Cenario: Fazer Login - Sessão ativa
    Dado que tento acessar qualquer tela interna estando logado
    Quando o app é iniciado
    Então sou redirecionado para a tela de lista.
```
- Regras de negócio: RN-01, RN-03
- Requisitos não funcionais: RNF-04 


**US-02 — Recuperar senha via e-mail**
**Detalhes:** Como usuário que esqueceu a senha, quero solicitar recuperação por e-mail, para não perder o acesso à minha conta.

**Critérios de aceite:**
```gherkin
    Dado que estou na tela de login
    Quando pressiono "Esqueci minha senha" e informo meu e-mail
    Então vejo a mensagem "E-mail de recuperação enviado" e o Firebase envia o link.

    Dado que informo um e-mail não cadastrado
    Quando solicito recuperação
    Então vejo a mesma mensagem de sucesso (por segurança, sem revelar inexistência).
```
Regras de negócio: RN-04
US-03 — Manter sessão ativa no dispositivo

Como usuário frequente,
Quero que o aplicativo mantenha minha sessão ativa
Para não precisar fazer login toda vez que abrir o aplicativo.

Critérios de aceite:

    Dado que fiz login com sucesso
    Quando fecho e reabro o aplicativo
    Então permaneço autenticado e vejo minha lista diretamente.

    Dado que realizei logout manualmente
    Quando reabro o aplicativo
    Então sou direcionado para a tela de login.

EP-02: Captura e Tratamento da Imagem
US-04 — Fotografar produto com câmera, pré-visualizar e refazer

Como usuário,
Quero acionar a câmera, fotografar um produto na gôndola, visualizar a prévia e ter a opção de refazer a foto
Para garantir uma imagem de boa qualidade antes de prosseguir.

Critérios de aceite:

    Dado que estou na tela principal e pressiono "Adicionar item"
    Quando concedo permissão de câmera (primeira vez)
    Então a câmera é aberta.

    Dado que a câmera está aberta
    Quando pressiono o botão de captura
    Então a foto é tirada e uma tela de pré-visualização é exibida com opções "Usar foto" e "Tirar outra".

    Dado que escolho "Tirar outra"
    Quando pressiono o botão
    Então retorno para a câmera para nova captura.

    Dado que escolho "Usar foto"
    Quando confirmo
    Então a imagem é enviada para processamento (US-05) e em seguida para o formulário de cadastro.

Regras de negócio: RN-05
Requisitos não funcionais: RNF-05
US-05 — Processar imagem e fazer upload para Storage

Como sistema,
Devo redimensionar a imagem (máx. 1024px de largura), comprimir (qualidade 80%) e fazer upload para o Firebase Storage no caminho users/{uid}/images/{timestamp}.jpg
Para armazenar a imagem de forma eficiente e obter a URL pública.

Critérios de aceite:

    Dado que o usuário confirmou a foto
    Quando o processamento é executado
    Então a imagem é redimensionada e comprimida, resultando em um arquivo ≤ 400 KB.

    Dado que o processamento foi concluído
    Quando o upload é feito
    Então a URL pública da imagem é retornada e fica disponível para a US-06 e US-08.

    Dado que ocorre falha no upload
    Quando há erro de rede
    Então uma mensagem "Erro ao enviar imagem. Tente novamente." é exibida e o usuário pode repetir.

Regras de negócio: RN-05, RN-06
Requisitos não funcionais: RNF-01, RNF-07
EP-03: Sugestão Inteligente com Gemini
US-06 — Enviar imagem para Gemini API e receber sugestão

Como usuário,
Quero que o aplicativo analise a imagem do produto usando IA (Gemini) e me retorne uma sugestão de nome e preço
Para preencher os dados do item mais rapidamente.

Critérios de aceite:

    Dado que a imagem foi carregada e o upload concluído
    Quando o sistema chama a Gemini API com a imagem e o prompt apropriado
    Então um JSON válido com { "nome": "...", "preco": 0.00 } é retornado em até 6 segundos.

    Dado que a Gemini identifica o produto e o preço na etiqueta
    Quando a resposta chega
    Então os campos nome e preco são preenchidos com as sugestões.

    Dado que a Gemini não consegue identificar os dados
    Quando a resposta contém strings vazias ou zero
    Então os campos permanecem vazios e o usuário preenche manualmente.

    Dado que o usuário atingiu o limite diário de 10 chamadas
    Quando a câmera é usada
    Então o botão "Analisar com IA" fica desabilitado e a mensagem "Limite diário de análises atingido. Tente novamente amanhã." é exibida.

    Dado que ocorre timeout (> 6s) ou erro na chamada da API
    Quando a requisição falha
    Então vejo a mensagem "Não foi possível analisar a imagem. Você pode preencher os dados manualmente." e o formulário manual é aberto normalmente.

Regras de negócio: RN-08, RN-10, RN-11, RN-12
Requisitos não funcionais: RNF-02, RNF-08
US-07 — Visualizar, editar e confirmar sugestão do Gemini

Como usuário,
Quero ver as sugestões da IA em um formulário editável
Para corrigir eventuais imprecisões antes de salvar o item.

Critérios de aceite:

    Dado que a Gemini retornou sugestões
    Quando o formulário é exibido
    Então os campos "Nome do produto" e "Valor unitário" aparecem preenchidos com os dados sugeridos, e são editáveis.

    Dado que altero manualmente qualquer campo
    Quando pressiono "Salvar item"
    Então os dados editados são os que persistem no Firestore.

    Dado que a Gemini não retornou sugestão válida
    Quando o formulário abre
    Então os campos estão vazios e vejo um indicador sutil "Preenchimento manual".

Regras de negócio: RN-09
EP-04: Gerenciamento da Lista de Compras
US-08 — Adicionar item à lista

Como usuário,
Quero preencher os campos de nome do produto, valor unitário e quantidade (com ou sem auxílio da IA) e salvar o item na minha lista
Para construir minha lista de compras com imagem, descrição e preço.

Critérios de aceite:

    Dado que estou no formulário de cadastro (após foto e/ou sugestão)
    Quando preencho nome, valor unitário (> 0) e quantidade (≥ 1) e pressiono "Adicionar à lista"
    Então o item é salvo no Firestore associado ao meu uid, com a URL da imagem e os dados preenchidos.

    Dado que deixo campos obrigatórios vazios ou inválidos
    Quando tento salvar
    Então vejo mensagens de validação específicas ("Nome obrigatório", "Valor deve ser maior que zero", "Quantidade mínima: 1").

    Dado que o salvamento é bem-sucedido
    Quando o Firestore confirma a gravação
    Então sou redirecionado para a tela da lista e vejo o novo item no topo.

Regras de negócio: RN-07, RN-13, RN-14
US-09 — Visualizar lista com totais por item e total geral

Como usuário,
Quero ver minha lista de compras completa, com foto, nome, quantidade, valor unitário, total por item e o total geral da compra
Para conferir os valores antes de passar no caixa.

Critérios de aceite:

    Dado que tenho itens salvos na minha lista
    Quando acesso a tela principal
    Então vejo todos os itens em uma lista rolável, cada um exibindo: miniatura da foto, nome, quantidade, valor unitário e total do item (qtd × valor).

    Dado que a lista possui itens
    Quando os totais são calculados
    Então o total geral da compra (soma de todos os totalItem) é exibido em destaque no rodapé da tela.

    Dado que minha lista está vazia
    Quando acesso a tela principal
    Então vejo uma mensagem "Sua lista está vazia" e um botão para adicionar o primeiro item.

Regras de negócio: RN-14, RN-15
Requisitos não funcionais: RNF-06
US-10 — Alterar quantidade de um item na lista

Como usuário,
Quero poder aumentar ou diminuir a quantidade de um item diretamente na lista
Para ajustar rapidamente durante a compra sem precisar remover e readicionar.

Critérios de aceite:
    Dado que estou visualizando a lista
    Quando pressiono "+" ou "-" ao lado da quantidade de um item
    Então a quantidade é incrementada/decrementada e o total do item e o total geral são recalculados instantaneamente.

    Dado que a quantidade atual é 1
    Quando pressiono "-"
    Então o botão deve ser desabilitado ou exibir confirmação para remover o item.

Regras de negócio: RN-17
US-11 — Excluir item da lista com confirmação

Como usuário,
Quero poder remover um item da lista, com uma confirmação para evitar exclusões acidentais
Para manter a lista sempre atualizada.

Critérios de aceite:

    Dado que estou visualizando a lista
    Quando deslizo o item para a esquerda ou pressiono o ícone de lixeira
    Então uma caixa de diálogo aparece: "Tem certeza que deseja excluir este item?".

    Dado que confirmo a exclusão
    Quando pressiono "Sim"
    Então o item é removido do Firestore, some da lista e o total geral é recalculado.

    Dado que cancelo a exclusão
    Quando pressiono "Não"
    Então a caixa de diálogo fecha e o item permanece na lista.

Regras de negócio: RN-16