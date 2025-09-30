# 🚀 Download de Arquivos via Base64 pela URL

Este repositório contém uma única página HTML (`index.html`) com um propósito específico:  
**receber uma string Base64 compactada via fragmento da URL, decodificá-la, descompactá-la e iniciar o download do arquivo no navegador.**

---

## ✨ Motivação

Em ambientes com **restrições de execução de scripts**, como ERPs (ex: Sankhya), é comum enfrentar limitações como:

- Bloqueio de URLs em ações como `onclick`;
- Remoção de atributos HTML que utilizam JavaScript;
- Bloqueio da execução de tags `<script>` injetadas dinamicamente;
- Limites de tamanho para URLs (ex: erro `URI Too Long`).

Essa página resolve esses problemas ao **transferir a lógica de download para um ambiente neutro**, como o GitHub Pages, onde o navegador pode processar os dados e baixar o arquivo normalmente, sem interferência.

---

## 🛠️ Como Usar

A página lê os dados via **fragmento da URL** (parte após o `#`), o que evita os limites tradicionais de tamanho de parâmetros.

### ✅ Estrutura da URL

https://<usuario>.github.io/<repositorio>/#cdata=<DADOS>&filename=<NOME_DO_ARQUIVO>


**Parâmetros:**

- `cdata`: Dados do arquivo, **compactados com GZIP** e depois codificados em **Base64**.
- `filename`: Nome desejado para o arquivo a ser baixado (ex: `meus_arquivos.zip`).

---

## 💻 Exemplo de Geração da URL em Java

```java
// Supondo que 'fileBytes' seja um byte[] com o conteúdo do arquivo
ByteArrayOutputStream compressedBaos = new ByteArrayOutputStream();
try (GZIPOutputStream gzipos = new GZIPOutputStream(compressedBaos)) {
    gzipos.write(fileBytes);
}
byte[] compressedBytes = compressedBaos.toByteArray();
String base64CompressedData = Base64.getEncoder().encodeToString(compressedBytes);

String filename = "meu_arquivo.zip";
String url = "https://<usuario>.github.io/<repositorio>/#cdata=" 
           + URLEncoder.encode(base64CompressedData, "UTF-8") 
           + "&filename=" + filename;
```
