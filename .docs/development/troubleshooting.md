# Desenvolvimento — Resolução de Problemas (Troubleshooting)

Este documento reúne soluções para problemas comuns de compilação, execução e ambiente no **Nexus**.

---

## 1. Falha de Inicialização do DataSource JPA

### Sintoma:
Ao executar `./mvnw spring-boot:run`, a aplicação falha com:
```
Description:
Failed to configure a DataSource: 'url' attribute is not specified and no embedded datasource could be configured.

Reason: Failed to determine a suitable driver class
Action: Consider the following:
	If you want an embedded database (H2, HSQL or Derby), please put it on the classpath.
	If you have database settings to be loaded from a particular profile you may need to activate it.
```

### Causa:
O arquivo `pom.xml` inclui `spring-boot-starter-data-jpa` e o driver `postgresql`, mas o arquivo `application.properties` ainda não contém uma URL de conexão configurada.

### Soluções:
1. **Configurar um PostgreSQL local ou Docker**:
   Defina as variáveis de ambiente:
   ```bash
   export SPRING_DATASOURCE_URL=jdbc:postgresql://localhost:5432/nexus_db
   export SPRING_DATASOURCE_USERNAME=nexus
   export SPRING_DATASOURCE_PASSWORD=nexus
   ```
2. **Executar testes ou compilação sem subir o servidor**:
   O comando `./mvnw clean test-compile` compila sem acionar a autoconfiguração do DataSource.

---

## 2. Incompatibilidade de Versão do Java

### Sintoma:
Erro ao compilar:
```
Fatal error compiling: error: release version 21 not supported
```
ou `UnsupportedClassVersionError: ... has been compiled by a more recent version of the Java Runtime (class file version 65.0)`.

### Solução:
O Nexus exige estritamente o **Java 21 LTS**. Verifique a versão ativa no terminal:
```bash
java -version
```
Certifique-se de que a variável de ambiente `JAVA_HOME` aponte para um JDK 21 válido.

---

## 3. Problemas com o Lombok em IDEs

### Sintoma:
Erros no editor alegando que métodos como `.getId()`, `.builder()` ou construtores requeridos não existem, apesar da classe possuir as anotações do Lombok.

### Solução:
1. Garanta que o plugin de Lombok esteja instalado na IDE.
2. Certifique-se de que o **Annotation Processing** está explicitamente habilitado nas configurações da IDE.
3. No Maven, confirme que o plugin `maven-compiler-plugin` possui a configuração de `annotationProcessorPaths` para o Lombok (já configurada no `pom.xml`).

---

## 4. Conflito de Porta HTTP (Port 8080)

### Sintoma:
`Web server failed to start. Port 8080 was already in use.`

### Solução:
Defina uma porta alternativa na inicialização:
```bash
./mvnw spring-boot:run -Dspring-boot.run.arguments="--server.port=8081"
```
ou via variável de ambiente:
```bash
export SERVER_PORT=8081
```
