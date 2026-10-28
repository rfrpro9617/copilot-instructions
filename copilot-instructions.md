## Princípios de Design & Arquitetura

### Tell, Don't Ask (TDA)
- **Não peça dados para tomar decisões por fora:** Evite pegar dados de um objeto com getters/propriedades, aplicar regras de negócio no código chamador e depois alterar o objeto via setter/atribuição.
- **Diga ao objeto o que fazer:** Encapsule a lógica de negócio dentro das próprias entidades e métodos de domínio.
- **Campos Privados:** Mantenha propriedades de estado protegidas/privadas (ex: `#atributo` em JS/TS, `private` ou `_` em Python).

**Exemplo Prático (JavaScript/TypeScript):**

```javascript
// ❌ RUIM (Ask - Estado exposto e lógica de negócio vazando para fora do objeto)
class ContaBancariaRuim {
    constructor(saldo, limite) {
        this.saldo = saldo;
        this.limite = limite;
    }
}

const contaRuim = new ContaBancariaRuim(100, 500);
const valorSaque = 150;

// O código chamador precisa conhecer as regras e alterar o estado diretamente
if (contaRuim.saldo + contaRuim.limite >= valorSaque) {
    contaRuim.saldo -= valorSaque;
} else {
    throw new Error("Saldo insuficiente");
}


// ✅ BOM (Tell - Encapsulamento completo da regra de negócio na entidade)
class ContaBancaria {
    #saldo;
    #limite;

    constructor(saldo, limite) {
        this.#saldo = saldo;
        this.#limite = limite;
    }

    sacar(valor) {
        if (this.#saldo + this.#limite < valor) {
            throw new Error("Saldo e limite insuficientes.");
        }
        this.#saldo -= valor;
    }
}

// O código chamador apenas diz o que quer fazer
const conta = new ContaBancaria(100, 500);
conta.sacar(150);
