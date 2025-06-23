# Diagrama da Arquitetura
```bash
                            ┌───────────────────────┐
                            │    Fontes de Dados    │
                            │ (Imagens, Vídeos, API)│
                            └─────────┬─────────────┘
                                      │
                                      ▼
                            ┌───────────────────────┐
                            │   Ingestão de Dados   │
                            │  (Kafka, AWS Kinesis) │
                            └─────────┬─────────────┘
                                      │
                                      ▼
                            ┌───────────────────────┐
                            │   Pipeline de Pré-    │
                            │    Processamento      │
                            │   (Spark, AWS Glue,   │
                            │  Lambda Functions)    │
                            └─────────┬─────────────┘
                                      │
                                      ▼
                            ┌─────────────────────────┐
                            │ Armazenamento de Dados  │
                            │ (S3, HDFS, Blob Storage)│
                            └─────────┬───────────────┘
                                      │
          ┌───────────────────────────┼─────────────────────────────────────┐
          │                           │                                     │
          ▼                           ▼                                     ▼
 ┌─────────────────┐        ┌──────────────────────┐           ┌──────────────────────────┐
 │  Treinamento e  │        │ Serviço de Inferência│           │ Serviço de Monitoramento │
 │   Atualização   │        │   (API REST / gRPC)  │           │         e Logs           │
 │    Contínua     │        │ (TensorFlow Serving  │           │ (Prometheus, ELK Stack)  │
 │   (Kubeflow,    │        │    ou TorchServe)    │           └──────────────────────────┘
 │    SageMaker,   │        └──────────────────────┘           
 │     MLflow)     │
 └─────────────────┘
          │
          ▼
┌───────────────────────────┐
│   Modelos Versionados e   │
│    Registrados (MLflow,   │
│ S3, Artifact Repositories)│
└───────────────────────────┘
```
---

## Documento Explicativo

### Componentes do Sistema e Conexões

1. **Fontes de Dados**

   * Captura dados de múltiplas origens: imagens, vídeos, dispositivos IoT, APIs externas.
   * Podem ser disparados por uploads, streams ou eventos.

2. **Ingestão de Dados**

   * Utiliza pipelines robustos como Apache Kafka ou AWS Kinesis para receber dados em alta escala, garantindo boa taxa de transferência de dados e baixa latência.
   * Permite buffering e ordenação para processamento em lote ou em tempo real.

3. **Pipeline de Pré-Processamento**

   * Ferramentas como Apache Spark, AWS Glue ou funções serverless (AWS Lambda, Azure Functions) realizam transformação, limpeza, normalização e pré-processamento das imagens/vídeos.
   * Processamento paralelo para alta performance.

4. **Armazenamento de Dados**

   * Dados brutos e pré-processados são armazenados em sistemas escaláveis e duráveis, como Amazon S3, HDFS ou Azure Blob Storage.
   * Suporte para acesso distribuído e versionamento.

5. **Treinamento e Atualização Contínua**

   * Frameworks como Kubeflow, SageMaker, MLflow ou pipelines customizados automatizam o treinamento, validação e deploy dos modelos.
   * Integração com versionamento de modelo para rollback e auditoria.
   * Permite incorporar novos dados e atualizar o modelo periodicamente ou sob demanda.

6. **Serviço de Inferência**

   * Modelos são servidos em endpoints escaláveis via TensorFlow Serving, TorchServe, ou serviços gerenciados na nuvem.
   * APIs REST ou gRPC para consumo por sistemas cliente (web, mobile, backend).
   * Balanceamento de carga e autoescalabilidade para atender alta demanda.

7. **Monitoramento e Logs**

   * Monitoramento contínuo da performance do sistema e do modelo (latência, taxa de acerto, erros).
   * Ferramentas como Prometheus, Grafana, ELK Stack para coleta e visualização de métricas e logs.
   * Alerta para degradação da performance e disparo de re-treinamento.

---

### Justificativas das Escolhas Tecnológicas

* **Kafka / AWS Kinesis**: Suportam ingestão de dados em tempo real e alto volume, essencial para cenários de múltiplas fontes e escalabilidade.
* **Spark / AWS Glue / Lambda**: Oferecem processamento distribuído, permitindo transformar e pré-processar grandes volumes de dados de forma eficiente.
* **S3 / HDFS / Blob Storage**: Armazenamento durável, escalável e econômico para datasets de imagem e vídeo em grande escala.
* **Kubeflow / SageMaker / MLflow**: Pipelines de Machine Learning que garantem automação do ciclo de vida dos modelos, facilitando treinamentos e atualizações contínuas.
* **TensorFlow Serving / TorchServe**: Frameworks otimizados para deploy de modelos ML em produção, com baixa latência e escalabilidade.
* **Prometheus / ELK Stack / Grafana**: Soluções robustas para monitoramento, análise e alerta, fundamentais para manutenção e melhoria contínua.

---

### 🧠 Autor
**Estêvão Santos Cavalcante**
