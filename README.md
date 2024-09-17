# Criando-um-Assistente-de-Delivery-com-AWS-Step-Functions-e-Bedrock

{
  "Comment": "Assistente de Delivery com AWS Step Functions e Bedrock",
  "StartAt": "ReceberPedido",
  "States": {
    "ReceberPedido": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "seu-modelo-id", // ID do modelo Bedrock usado para processar o pedido
        "Input": {
          "Texto": "Receber pedido do cliente" // Texto de entrada para o modelo
        }
      },
      "Next": "SugerirPratoRomantico" // Próxima etapa após receber o pedido
    },
    "SugerirPratoRomantico": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "seu-modelo-id", // ID do modelo Bedrock usado para sugerir pratos românticos
        "Input": {
          "Texto": "Sugerir prato romântico em Monte Verde - MG" // Texto de entrada para o modelo
        }
      },
      "Next": "SugerirVinho" // Próxima etapa após sugerir o prato
    },
    "SugerirVinho": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "seu-modelo-id", // ID do modelo Bedrock usado para sugerir vinhos
        "Input": {
          "Texto": "Sugerir vinho apropriado para o prato" // Texto de entrada para o modelo
        }
      },
      "Next": "ProcessarPagamento" // Próxima etapa após sugerir o vinho
    },
    "ProcessarPagamento": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "processarPagamentoLambda", // Nome da função Lambda para processar o pagamento
        "Payload": {
          "Pedido": "$.Pedido" // Dados do pedido passados para a função Lambda
        }
      },
      "Next": "PrepararPedido" // Próxima etapa após processar o pagamento
    },
    "PrepararPedido": {
      "Type": "Task",
      "Resource": "arn:aws:states:::bedrock:invokeModel",
      "Parameters": {
        "ModelId": "seu-modelo-id", // ID do modelo Bedrock usado para preparar o pedido
        "Input": {
          "Texto": "Preparar pedido" // Texto de entrada para o modelo
        }
      },
      "Next": "RealizarEntrega" // Próxima etapa após preparar o pedido
    },
    "RealizarEntrega": {
      "Type": "Task",
      "Resource": "arn:aws:states:::lambda:invoke",
      "Parameters": {
        "FunctionName": "realizarEntregaLambda", // Nome da função Lambda para realizar a entrega
        "Payload": {
          "Pedido": "$.Pedido" // Dados do pedido passados para a função Lambda
        }
      },
      "End": true // Indica que esta é a etapa final do fluxo de trabalho
    }
  }
}
