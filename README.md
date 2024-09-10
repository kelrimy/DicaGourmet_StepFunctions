# Assistente de Delivery com AWS Step Functions e Bedrock

## Descrição do Projeto

Este projeto demonstra a criação de um assistente de delivery utilizando AWS Step Functions e o modelo Bedrock da AWS. O objetivo do assistente é sugerir combinações gastronômicas baseadas em um prompt fornecido, como "Para um jantar romântico com macarrão, sugira 3 itens que combinem para gerar uma experiência gastronômica."

O assistente realiza três etapas principais:
1. Sugere combinações de pratos.
2. Sugere uma bebida para acompanhar.
3. Sugere um lugar perfeito para um jantar romântico em Paris.

A máquina de estado foi criada utilizando Amazon States Language (ASL) e o modelo Bedrock foi invocado para gerar as sugestões.

## O Que Aprendi

- **AWS Step Functions**: Aprendi a criar e gerenciar fluxos de trabalho complexos usando AWS Step Functions. Compreendi como a orquestração de estados pode coordenar múltiplos serviços e tarefas de forma eficiente.
- **Amazon Bedrock**: Explorei como invocar modelos de linguagem para gerar respostas baseadas em prompts. Isso me ajudou a entender a integração de modelos de machine learning em fluxos de trabalho automatizados.
- **Design de Fluxo de Trabalho**: Descobri como estruturar um fluxo de trabalho para obter respostas contextuais e sequenciais, e como gerenciar o estado e a execução entre as etapas.

## Partes Vistas na Plataforma AWS

- **AWS Step Functions**: Utilizei para definir e gerenciar a máquina de estados que coordena o fluxo de trabalho.
- **Amazon Bedrock**: Utilizei para invocar modelos de linguagem e obter respostas baseadas em prompts.

## Possibilidades e Facilidade com AWS Step Functions

- **Orquestração Simples e Eficiente**: AWS Step Functions facilita a coordenação de múltiplas tarefas e serviços, permitindo a criação de fluxos de trabalho complexos com simplicidade.
- **Visualização de Fluxos de Trabalho**: A plataforma oferece uma visualização gráfica dos fluxos de trabalho, tornando mais fácil o monitoramento e a depuração.
- **Integração com Outros Serviços AWS**: Step Functions se integra facilmente com outros serviços da AWS, como AWS Lambda e Amazon Bedrock, para criar soluções robustas e escaláveis.

## Código ASL (Amazon States Language)

```json
{
  "Comment": "Assistente de Delivery com AWS Step Functions e Bedrock",
  "StartAt": "Start",
  "States": {
    "Start": {
      "Type": "Pass",
      "Result": {
        "prompt": "Para um jantar romântico com macarrão, sugira 3 itens que combinem para gerar uma experiência gastronômica."
      },
      "ResultPath": "$.conversation",
      "Next": "InvokeModel1"
    },
    "InvokeModel1": {
      "Type": "Task",
      "Resource": "arn:aws:bedrock:us-west-2:123456789012:invokeModel",
      "Parameters": {
        "ModelName": "example-model",
        "Input": {
          "prompt": "$.conversation.prompt"
        }
      },
      "ResultPath": "$.modelOutput1",
      "Next": "AddToConversation1"
    },
    "AddToConversation1": {
      "Type": "Pass",
      "InputPath": "$.modelOutput1",
      "ResultPath": "$.conversation.history[0]",
      "Next": "InvokeModel2"
    },
    "InvokeModel2": {
      "Type": "Task",
      "Resource": "arn:aws:bedrock:us-west-2:123456789012:invokeModel",
      "Parameters": {
        "ModelName": "example-model",
        "Input": {
          "prompt": "Sugira uma bebida que combine com a sugestão anterior."
        }
      },
      "ResultPath": "$.modelOutput2",
      "Next": "AddToConversation2"
    },
    "AddToConversation2": {
      "Type": "Pass",
      "InputPath": "$.modelOutput2",
      "ResultPath": "$.conversation.history[1]",
      "Next": "InvokeModel3"
    },
    "InvokeModel3": {
      "Type": "Task",
      "Resource": "arn:aws:bedrock:us-west-2:123456789012:invokeModel",
      "Parameters": {
        "ModelName": "example-model",
        "Input": {
          "prompt": "Sugira um lugar perfeito para jantar romântico em Paris."
        }
      },
      "ResultPath": "$.modelOutput3",
      "Next": "AddToConversation3"
    },
    "AddToConversation3": {
      "Type": "Pass",
      "InputPath": "$.modelOutput3",
      "ResultPath": "$.conversation.history[2]",
      "Next": "End"
    },
    "End": {
      "Type": "Succeed"
    }
  }
}
```

## Instruções para Uso

**Clone o Repositório**:
   ```bash
   git clone https://github.com/kelrimy/DicaGourmet_StepFunctions.git
```
## Configuração do AWS Step Functions

1. Faça login no Console de Gerenciamento da AWS.
2. Navegue até o serviço AWS Step Functions.
3. Crie uma nova máquina de estado e cole o código ASL fornecido.

## Configuração do Amazon Bedrock

1. Certifique-se de ter uma conta na AWS e acesso ao Amazon Bedrock.
2. Substitua o ARN do recurso e o nome do modelo pelos valores corretos para sua configuração.

## Executando o Pipeline

1. Inicie a execução da máquina de estado no Console do AWS Step Functions e observe os resultados na visualização gráfica.

## Conclusão

Este projeto demonstra a integração de AWS Step Functions com Amazon Bedrock para criar um assistente de delivery que sugere combinações gastronômicas. A utilização desses serviços permite criar fluxos de trabalho complexos de maneira eficiente, escalável e visualmente gerenciável.

## Contato

Para mais informações ou dúvidas, sinta-se à vontade para entrar em contato:


- **E-mail**: kelrimymbb@gmail.com
