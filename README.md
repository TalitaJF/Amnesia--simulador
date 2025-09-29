# Simulações de Arquitetura — Amnesia (Trabalhos de Arquitetura 3)

**Resumo**  
Este repositório contém configurações, traces e relatórios para experimentos com o simulador **Amnesia** — estudando hierarquia de memória, cache, memória virtual, TLB, políticas de substituição e localidade. O conteúdo e os exemplos foram organizados a partir do documento *Arquitetura 3.pdf*.


## Visão Geral
O objetivo é investigar o impacto das configurações de cache, políticas de escrita/substituição, associatividade e tamanho de linha sobre o tempo total de execução e número de misses/hits. Os resultados mostram como escolhas de arquitetura influenciam localidade temporal e espacial.

---

## Requisitos
- Java 8+ (ou versão compatível com o `amnesia.jar`)  
- `amnesia.jar` (coloque no diretório do projeto ou referencie o caminho completo)  
- Traces de execução (arquivo texto com pares tipo/endereço — formato descrito abaixo)

---

## Como executar
Abra um terminal no diretório do JAR e rode:
```bash
# exemplo (Windows)
"C:\Program Files (x86)\Common Files\Oracle\Java\java8path\java.exe" -jar amnesia.jar

# ou
java -jar amnesia.jar
```

## Cenários abordados:
1. **Hierarquia completa** — Comparar execução sem cache, apenas com L1 e depois com L1 + L2.  
2. **Tamanho da cache** — Alterar memória cache pequena vs. grande e observar impacto em misses e tempo total.  
3. **Política de substituição** — Testar FIFO vs. LRU e avaliar qual lida melhor com localidade temporal.  
4. **Associatividade** — Comparar mapeamento direto (1-way) vs. associatividade maior (ex.: 4-way).  
5. **Tamanho da linha de cache** — Usar linhas pequenas vs. grandes para estudar localidade espacial e miss penalty.  

---

## Formato de configuração (XML)
O simulador usa arquivos XML com parâmetros. Exemplo mínimo:

```xml
<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE AmnesiaConfiguration SYSTEM "Configuration/amnesia.dtd">
<AmnesiaConfiguration>
  <Processor>
    <processorContains>0</processorContains>
    <createTraceFile>0</createTraceFile>
  </Processor>

  <Trace>
    <wordSize>4</wordSize>
  </Trace>

  <CPU>
    <wordSize>4</wordSize>
  </CPU>

  <MainMemory>
    <blockSize>1</blockSize>
    <memorySize>256</memorySize>
    <ciclesPerAccessRead>1</ciclesPerAccessRead>
    <ciclesPerAccessWrite>2</ciclesPerAccessWrite>
    <timeCicle>2</timeCicle>
  </MainMemory>

  <Cache>
    <cacheType>Unified</cacheType>
    <unifiedCache>
      <lineSize>1</lineSize>
      <memorySize>64</memorySize>
      <associativityLevel>1</associativityLevel>
      <writePolicy>WriteBack</writePolicy>
      <replacementAlgorithm>LRU</replacementAlgorithm>
      <ciclesPerAccessRead>1</ciclesPerAccessRead>
      <ciclesPerAccessWrite>2</ciclesPerAccessWrite>
      <timeCicle>1</timeCicle>
    </unifiedCache>
  </Cache>
</AmnesiaConfiguration>


# ou
java -jar amnesia.jar
