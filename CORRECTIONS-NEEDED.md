# CORREÇÕES NECESSÁRIAS NO FIRMWARE
## Limitações Críticas Identificadas

### 🚨 **PRIORIDADE ALTA - Parser de Comandos**

#### **Problema 1: Espaço Obrigatório**
**Localização:** `executarPrograma()` linhas 598-611
**Problema:** Parser exige espaço mesmo para valores padrão
```cpp
// ATUAL (problemático):
if (lendoComando && corpo.charAt(i) == ' ') {
    // Só processa comando SE encontrar espaço
}
```

**Comportamento problemático:**
- ❌ `pf` → comando ignorado (não funciona)
- ✅ `pf ` → funciona (usa padrão 120ms)
- ✅ `pf 500` → funciona (usa 500ms)

**Solução proposta:**
```cpp
// Aceitar comando no final da linha OU com espaço
if (lendoComando && (corpo.charAt(i) == ' ' || corpo.charAt(i) == '\n')) {
    // Processa comando
    if (corpo.charAt(i) == '\n') {
        valores[totalComandos] = "";  // Sem parâmetro = usar padrão
        totalComandos++;
        lendoComando = true;
        inicio = i + 1;
    } else {
        // Continua lógica atual para espaço
    }
}
```

**Impacto educacional:**
- Sintaxe atual confusa para iniciantes
- `pf` é mais intuitivo que `pf ` (espaço invisível)
- Reduz erros de digitação

---

### 🔧 **MELHORIAS SUGERIDAS**

#### **Melhoria 1: Feedback de Erro**
**Problema:** Comandos inválidos são silenciosamente ignorados
**Solução:** Adicionar feedback visual/sonoro para comandos não reconhecidos

#### **Melhoria 2: Validação de Parâmetros**
**Problema:** Valores inválidos usam padrão sem aviso
**Solução:** Validar ranges e dar feedback

#### **Melhoria 3: Parser Case-Insensitive Robusto**
**Problema:** Apenas `toLowerCase()` pode não cobrir todos os casos
**Solução:** Validação mais robusta de entrada

---

### 📋 **TESTES DE REGRESSÃO NECESSÁRIOS**

Após correções, validar:

1. **Compatibilidade existente:**
   - `pf 500` continua funcionando
   - `sensor linha` continua funcionando
   - Estruturas `repita`/`fim repita` funcionam

2. **Nova funcionalidade:**
   - `pf` funciona (usa padrão)
   - `pt` funciona (usa padrão)
   - `seguir-linha` funciona (usa padrão)

3. **Casos extremos:**
   - Comandos vazios não causam crash
   - Múltiplos espaços são tratados
   - Linhas em branco são ignoradas

---

### 💡 **IMPLEMENTAÇÃO RECOMENDADA**

**Fase 1:** Corrigir parser para aceitar comandos sem espaço
**Fase 2:** Adicionar validação e feedback de erro
**Fase 3:** Testes extensivos com programas PJE existentes
**Fase 4:** Documentação atualizada

**Arquivo alvo:** `/src/firmware_template.ino` linhas 587-674

---

**Data de identificação:** 2026-01-18
**Identificado por:** Análise de documentação de comandos
**Prioridade:** Alta (afeta usabilidade educacional)