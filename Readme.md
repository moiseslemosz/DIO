# ☁️ Lab: Gerenciamento Avançado de EC2 na AWS (AMIs e Snapshots)

Este repositório documenta o laboratório prático sobre a criação e gerenciamento do ciclo de vida de instâncias EC2, com foco na utilização de AMIs (Amazon Machine Images) e Snapshots EBS para provisionamento rápido e estratégias de backup.

---

## 2. Objetivos do Laboratório

* **Provisionamento Rápido:** Criar um "molde" (AMI) de uma instância configurada.
* **Resiliência:** Implementar backups pontuais (Snapshots) do volume de armazenamento.
* **Compreensão Arquitetural:** Entender a relação entre Volume EBS, Snapshot e AMI.

---

## 3. Conceitos Fundamentais

### Instância EC2 (Elastic Compute Cloud)
* É o serviço da AWS que fornece capacidade de computação redimensionável na nuvem (máquinas virtuais).

### Volumes EBS (Elastic Block Store)
* Volumes de armazenamento de bloco persistente que podem ser anexados a instâncias EC2. O volume raiz (Root Volume) da instância é um volume EBS.

### Snapshots EBS
* **Backup incremental:** São cópias de segurança pontuais (*point-in-time*) dos volumes EBS. Eles são armazenados no Amazon S3 e são o mecanismo subjacente usado pelas AMIs.

### AMI (Amazon Machine Image)
* Um **modelo de boot** que contém o sistema operacional, softwares, configurações e os dados necessários para criar uma instância. Uma AMI é composta por um ou mais Snapshots EBS.

---

## 4. Passo a Passo Prático Documentado

### A. Preparação da Instância Base (Golden Image)

1.  **Criação:** Uma instância EC2 (`t2.micro` com Ubuntu/Amazon Linux) foi provisionada.
2.  **Configuração:** Softwares essenciais como Servidor Web Apache foram instalados e testados para simular uma "Imagem de Ouro".
3.  **Encerrar (Stop):** A instância foi desligada (Stopped) para garantir a integridade dos dados antes de criar o Snapshot/AMI.

### B. Criação do Snapshot para Backup (Resiliência)

* **Ação:** Um Snapshot foi criado diretamente do Volume EBS raiz da instância parada.
* **Comando:** `aws ec2 create-snapshot --volume-id <volume-id> --description "Backup Manual Lab"`
* **Uso:** O Snapshot serve como um backup do disco em um momento específico.

### C. Criação e Teste da AMI (Provisionamento)

* **Criação da AMI:** Uma AMI foi gerada a partir da instância parada ou do Snapshot recém-criado.
* **Verificação:** Uma nova instância (`EC2-Clone`) foi lançada usando esta AMI personalizada.
* **Resultado:** A nova instância iniciou com todas as configurações e softwares da "Imagem de Ouro", comprovando a funcionalidade da AMI para provisionamento.
* **Relação:** Essa AMI agora está disponível para ser usada em Auto Scaling Groups ou para provisionar novos ambientes de desenvolvimento idênticos.

---

## 5. Diagrama do Fluxo de Trabalho



* O diagrama representa a relação cíclica entre a **Instância EC2**, o **Volume EBS** (armazenamento), o **Snapshot** (backup) e a **AMI** (provisionamento).  
<br>

<p align="center">
    <img src="Diagrama-aws.svg" alt="Diagrama de Fluxo de Imagem EC2 e Snapshot" style="max-width: 100%; height: auto;">
</p>

----

## 6. Cleanup (Limpeza)
* Todas as instâncias de teste, AMIs personalizadas e Snapshots EBS foram terminadas/deletadas para evitar custos indesejados (Boa prática!).