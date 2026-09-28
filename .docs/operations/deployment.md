# Operações — Empacotamento e Implantação (Deployment)

Este documento descreve os mecanismos de compilação de artefatos, estratégias de containerização e considerações de execução em produção para o **Nexus**.

---

## 1. Geração de Artefato Executável

O projeto utiliza o plugin `spring-boot-maven-plugin` (já presente no `pom.xml`) para produzir um JAR autoexecutável contendo o Tomcat embutido:

```bash
./mvnw clean package -DskipTests
```

O arquivo final será gerado em:
`target/nexus-0.0.1-SNAPSHOT.jar`

Para executá-lo diretamente:
```bash
java -jar target/nexus-0.0.1-SNAPSHOT.jar
```

---

## 2. Containerização e Geração de Imagem OCI

### 2.1. Via Cloud Native Buildpacks (Nativo do Spring Boot)
O Spring Boot Maven Plugin suporta a geração direta de imagens OCI sem a necessidade de um `Dockerfile` local:

```bash
./mvnw spring-boot:build-image -Dspring-boot.build-image.imageName=nexus:latest
```

### 2.2. Via Dockerfile Customizado `[Questão Aberta]`
Caso seja necessária uma imagem base corporativa específica (ex: Alpine, Distroless ou Chainguard), um `Dockerfile` multistage poderá ser introduzido na raiz do repositório.
*Status Atual*: Nenhum arquivo `Dockerfile` ou manifesto Kubernetes existe atualmente no repositório.

---

## 3. Diretrizes de Execução em Produção

1. **Suporte a Containers na JVM**:
   - Garantir que a JVM respeite os limites de cgroups do container:
     ```
     -XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0
     ```
2. **Graceful Shutdown**:
   - Configurar o encerramento gracioso para permitir que requisições HTTP em andamento e tokens em processamento terminem com segurança antes da interrupção do container:
     ```properties
     server.shutdown=graceful
     spring.lifecycle.timeout-per-shutdown-phase=20s
     ```
3. **Imutabilidade do Container**:
   - O sistema de arquivos do container em produção deve ser montado como `read-only`, com exceção de diretórios temporários (`/tmp`).
