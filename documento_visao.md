# Documento de Visão — PandasVisor

Aplicativo em React Native de conferência de compras.

## INTRODUÇÃO

O dia a dia do consumidor em supermercados e lojas de varejo ainda é marcado por uma falha recorrente: a divergência
entre o preço anunciado na gôndola e o valor cobrado no caixa. Essa situação gera desconfiança, perda de tempo em
conferências manuais e, frequentemente, prejuízo financeiro para o cliente.

Grande parte do problema decorre da falta de um registro pessoal, rápido e confiável dos preços observados no momento da
escolha do produto. O consumidor depende da própria memória ou de anotações em papel, o que torna a conferência no caixa
pouco precisa e suscetível a erros. Além disso, não há uma ferramenta simples que una a captura visual do produto, o
registro de seu valor e a geração automática de um total de compras para validação imediata.

O PandasVisor surge como uma solução mobile que coloca o controle nas mãos do consumidor. Utilizando a câmera do
celular, o aplicativo permite fotografar o produto diretamente na gôndola, associar um preço e uma quantidade, e montar
uma lista de compras que calcula automaticamente o total esperado. Ao chegar ao caixa, o usuário pode conferir de forma
rápida e segura se os valores cobrados correspondem ao que foi registrado, reduzindo prejuízos e aumentando a
transparência na experiência de compra.

## CONTEXTO DE NEGÓCIO

O setor varejista lida diariamente com um grande volume de transações e uma variedade imensa de produtos. Apesar dos
avanços tecnológicos nos sistemas de ponto de venda, o consumidor final ainda carece de uma ferramenta pessoal para
auditar de forma prática os preços que lhe são cobrados.

Atualmente, o comprador comum enfrenta dois obstáculos principais: a dificuldade de lembrar o preço exato de cada item
visto na prateleira e a ausência de um histórico simples para comparar o valor registrado na compra com o total
esperado. As alternativas existentes — anotar em papel, digitar em blocos de notas ou confiar na própria memória — são
propensas a esquecimentos e erros de digitação, além de não oferecerem uma forma ágil de verificação no momento do
pagamento.

Por outro lado, o uso massivo de smartphones com câmeras de boa qualidade abre uma oportunidade clara: transformar o
celular em um assistente de conferência de compras. Ao fotografar o produto e registrar seu preço, o consumidor gera um
registro visual que facilita a identificação e, combinado com uma lista automatizada, ganha poder de auditoria sobre o
valor total da compra.

O PandasVisor nasce nesse contexto como um aplicativo mobile que integra captura de imagem, persistência em nuvem (Firebase) e cálculo automático de totais. A proposta é oferecer ao consumidor uma ferramenta simples, porém robusta,
para que ele possa criar sua lista de compras a partir das imagens da gôndola e, ao final, conferir com exatidão se o
valor cobrado corresponde ao que foi registrado, promovendo uma experiência de compra mais justa e transparente.

## POSICIONAMENTO

### Declaração do Problema

|                                | Detalhes                                                                                                                               |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| O problema de                  | Divergência entre os preços exibidos na gôndola e os valores efetivamente cobrados no caixa, gerando desconfiança e prejuízo.          |
| Afeta                          | Consumidores em supermercados, lojas de varejo e qualquer estabelecimento que utilize etiquetas de preço nas prateleiras.              |
| Cujo impacto é                 | Perda financeira, tempo gasto em reclamações, dificuldade de conferir manualmente vários itens e insatisfação com a loja.              |
| Uma solução de sucesso deveria | Um aplicativo que capture o preço do produto na prateleira via foto, monte uma lista com totais e permita conferência rápida no caixa. |

### Declaração da Visão do Software

|               | Detalhes                                                                                                                                                                                                |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Para          | Consumidores que desejam conferir se o valor cobrado no caixa está de acordo com o preço da prateleira.                                                                                                 |
| Que           | Enfrentam dificuldade em registrar preços de forma rápida e segura durante as compras, resultando em divergências não detectadas no momento do pagamento.                                               |
| O PandasVisor | É um aplicativo mobile de conferência de compras baseado em imagem.                                                                                                                                     |
| Que           | Permite fotografar o produto na gôndola, inserir preço e quantidade, calcular automaticamente o total da compra e exibir uma lista para verificação no caixa.                                           |
| Diferente de  | Anotações manuais, blocos de notas genéricos ou planilhas desconectadas, o PandasVisor oferece registro visual, armazenamento em nuvem e cálculo dinâmico em uma única experiência mobile.              |
| Nosso produto | Utiliza a câmera do dispositivo e serviços Firebase (autenticação, Firestore e Storage) para garantir que cada usuário tenha sua própria lista segura e acessível, com histórico e totalização precisa. |

### Descrição das Partes Interessadas

| Nome                       | Descrição                                                                                                                 | Responsabilidades                                                                                                                                                                                                           |
|----------------------------|---------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Consumidor / Usuário Final | Pessoa que realiza compras em supermercados e lojas, interessada em conferir os preços cobrados no caixa.                 | • Fotografar o produto na prateleira<br>• Informar corretamente o preço e a quantidade<br>• Conferir a lista antes do pagamento<br>• Utilizar o app como ferramenta de auditoria de preços*                                 |
| Equipe de Desenvolvimento  | Responsável por projetar, implementar e manter o aplicativo PandasVisor, incluindo a integração com os serviços Firebase. | • Garantir a captura e o tratamento adequado das imagens<br>• Manter a autenticação segura e a separação de dados por usuário<br>• Assegurar o cálculo correto dos totais<br>• Prover atualizações e correções necessárias* |

### Visão Geral do Produto
**Necessidades e Funcionalidades**

| Necessidade                                             | Funcionamento                                            | Prioridade | Responsável               |
|---------------------------------------------------------|----------------------------------------------------------|------------|---------------------------|
| Acessar o aplicativo com segurança                      | Fazer login com e-mail e senha via Firebase Auth         | Alta       | Equipe de Desenvolvimento |
| Garantir que apenas o próprio usuário veja seus dados   | Associar todos os registros ao ID do usuário autenticado | Alta       | Equipe de Desenvolvimento |
| Capturar visualmente o produto na gôndola               | Tirar foto usando a câmera do dispositivo                | Alta       | Equipe de Desenvolvimento |
| Otimizar a imagem para armazenamento e desempenho       | Redimensionar e comprimir a foto antes do upload         | Alta       | Equipe de Desenvolvimento |
| Armazenar a foto de forma persistente                   | Upload da imagem para Firebase Storage                   | Alta       | Equipe de Desenvolvimento |
| Criar um item de compra com nome, preço e quantidade    | Formulário de cadastro de produto após a foto            | Alta       | Equipe de Desenvolvimento |
| Calcular automaticamente o valor de cada item           | Cálculo do total do item (valor unitário × quantidade)   | Alta       | Equipe de Desenvolvimento |
| Salvar os itens da lista na nuvem                       | Gravação dos dados no Cloud Firestore                    | Alta       | Equipe de Desenvolvimento |
| Visualizar todos os itens adicionados                   | Listagem dinâmica da lista de compras                    | Alta       | Equipe de Desenvolvimento |
| Alterar a quantidade de um item já registrado           | Edição da quantidade (com recálculo do total)            | Alta       | Equipe de Desenvolvimento |
| Remover um item da lista                                | Excluir item (com atualização automática do total geral) | Alta       | Equipe de Desenvolvimento |
| Obter o valor total da compra para conferência no caixa | Exibição do total geral calculado automaticamente        | Alta       | Equipe de Desenvolvimento |
| Recuperar a lista em futuras execuções do app           | Persistência e carregamento dos dados por usuário        | Alta       | Equipe de Desenvolvimento |

### Requisitos Não Funcionais Preliminares
| Requisito não funcional                                                                                                              | Prioridade |
|--------------------------------------------------------------------------------------------------------------------------------------|------------|
| O aplicativo deve ser desenvolvido em React Native, compatível com dispositivos Android.                                             | Alta       |
| A autenticação deve utilizar Firebase Authentication.                                                                                | Alta       |
| Os dados devem ser persistidos no Cloud Firestore e as imagens no Firebase Storage.                                                  | Alta       |
| A captura de imagem deve empregar a biblioteca expo‑camera (ou similar) e incluir tratamento de redimensionamento/compressão.        | Alta       |
| O tempo entre a captura da foto e a disponibilidade da imagem na nuvem não deve ultrapassar 5 segundos em condições normais de rede. | Média      |
| A interface deve ser simples e intuitiva, permitindo o uso rápido durante uma compra real.                                           | Alta       |
| O aplicativo deve funcionar de forma adequada em diferentes tamanhos de tela (smartphones).                                          | Média      |
| O sistema deve garantir que cada usuário acesse exclusivamente sua própria lista de compras.                                         | Alta       |
