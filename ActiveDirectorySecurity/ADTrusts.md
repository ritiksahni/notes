# Trusts

---
Trusts are relationships between multiple domains, it allows users to access resources from external domain. 

Types of trusts:

1. Unidirectional - When domain A shares its resources with domain B but can't access resources from domain B in return (one-way trust).
2. Bidirectional - When domain A shares its resources with domain B and domain B shares its resources with domain A (two-way trust).
3. Transitive - If domain A trusts domain B and domain B trusts domain C, we can enable transitivity so that domain C also trusts domain A as its trusted by Domain B. Works like the mathematics formula - If A = B and B = C then A = C.
4. Forest trusts - Trust between completely different forests.
5. Shortcut trusts - When domains of different forests have trust (uni or bidirectional) between them without the trust between root of forests. Domain can directly access another forest's domain without a forest trust.

---
