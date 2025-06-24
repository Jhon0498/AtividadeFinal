# Microsserviço Transações

Este microsserviço é responsável por consumir mensagens da fila RabbitMQ chamada `categoria.queue`. 

## Requisitos

- Java 17+
- Maven
- RabbitMQ rodando localmente (http://localhost:15672)
- Microsserviço RabbitMQ Producer que envia mensagens para a fila `categoria.queue`

## Configuração do RabbitMQ

Certifique-se de que o RabbitMQ está instalado e em execução localmente na porta padrão 5672. O painel de controle fica disponível em:  
[http://localhost:15672](http://localhost:15672)  
Usuário e senha padrão: `guest` / `guest`

## Estrutura do projeto

- A classe `CategoriaConsumer` escuta a fila `categoria.queue` e processa as mensagens recebidas.
- Pacote: `com.seuprojeto.transacao.messaging`

## Configurações importantes (`application.properties`)

```properties
spring.rabbitmq.host=localhost
spring.rabbitmq.port=5672
spring.rabbitmq.username=guest
spring.rabbitmq.password=guest
Como rodar
Abra o terminal Git Bash (recomendado para evitar problemas com espaços no caminho).

Navegue até a pasta do projeto:

bash
Copiar
Editar
Caminho do local abaixo
...    ....
Execute o projeto com Maven Wrapper:

bash
Copiar
Editar
./mvnw spring-boot:run
Verifique no painel do RabbitMQ em http://localhost:15672/#/queues que a fila categoria.queue foi criada e tem um consumidor conectado.

Quando uma nova categoria for criada (por exemplo, via microsserviço Producer), você verá no console do Transações uma mensagem parecida com:

css
Copiar
Editar
[Consumer - Transações] Mensagem recebida: {"id":1,"nome":"Alimentação"}
Classe principal do Consumer
java
Copiar
Editar
package com.seuprojeto.transacao.messaging;

import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Component;

@Component
public class CategoriaConsumer {

    @RabbitListener(queues = "categoria.queue")
    public void ouvirCategoria(String mensagem) {
        System.out.println("[Consumer - Transações] Mensagem recebida: " + mensagem);
    }
}
Dependências Maven relevantes
xml
Copiar
Editar
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency># AtividadeFinal
