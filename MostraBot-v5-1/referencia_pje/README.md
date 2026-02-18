# Referencia PJE Nuvem (snapshot local)

Este pacote guarda arquivos essenciais baixados da PJE para alinhar a apostila com a interface real.

## Acesso externo (URLs oficiais)
- `https://pje.ejrrobotica.com.br/index.php`
- `https://pje.ejrrobotica.com.br/modulo1.php`
- `https://pje.ejrrobotica.com.br/modulo2.php`
- `https://pje.ejrrobotica.com.br/modulo4.php`

## Arquivos
- `index.html`: pagina principal (publica)
- `index.php.html`: tela de login (ID, senha e selecao de modulo)
- `modulo1.php.html`: snapshot salvo do modulo 1 (no download veio tela de login)
- `modulo4.php.html`: snapshot salvo do modulo 4 (no download veio tela de login)
- `index.js`: logica de login e carregamento dos modulos

## Observacao sobre Modulo 2
Nao foi encontrado um arquivo `modulo2.php.html` no pacote baixado. Para manter o contexto pedagogico na apostila, o fluxo do Modulo 2 foi integrado com base em:
- layout e exemplos visuais ja existentes (`assets_full/02_pje_modulo2.png`)
- padrao de acesso por selecao de modulo no login (`index.js`)
- continuidade didatica entre Modulo 1 (cliques) e Modulo 4 (texto + logica)
